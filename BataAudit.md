# BATA Smart Contract Security Audit Report

### Disclaimer

This audit represents a point-in-time assessment. Smart contracts require ongoing security monitoring and regular audits. No audit can identify 100% of vulnerabilities, so multiple audits are recommended.

---

## Table of Contents

1. [Methodology](#methodology)
2. [Executive Summary](#executive-summary)
3. [Critical Issues](#critical-issues)
   - [1. Staking Lock Duration Reset Attack](#1-critical-staking-lock-duration-reset-attack)
4. [Medium Issues](#medium-issues)
   - [2. Missing Slippage Protection for BATA Price Changes](#2-medium-missing-slippage-protection-for-bata-price-changes)
   - [3. Unbounded Loop DoS](#3-medium-unbounded-loop-dos)
5. [Admin-Related Issues](#admin-related-issues)
   - [4. RescueToken Can Drain User Funds](#4-admin-rescuetoken-can-drain-user-funds)
   - [5. Merkle Root Updates After Claims](#5-admin-merkle-root-updates-after-claims)
   - [6. TGE Timestamp Changes](#6-admin-tge-timestamp-changes)
6. [Low Issues](#low-issues)
   - [7. Use Ownable2Step instead of Ownable](#7-low-use-ownable2step-instead-of-ownable)
7. [Gas Optimization Issues](#gas-optimization-issues)
   - [8. Nesting if-statements is cheaper than using &&](#8-gas-saving-nesting-if-statements-is-cheaper-than-using-)
   - [9. Cache array length outside of loop](#9-gas-saving-cache-array-length-outside-of-loop)
   - [10. Use Custom Errors](#10-gas-saving-use-custom-errors)
   - [11. Using `private` rather than `public` for constants, saves gas](#11-gas-saving-using-private-rather-than-public-for-constants-saves-gas)

---

## Methodology

This audit was conducted using three-phase approach to ensure thorough coverage of potential vulnerabilities:

### Phase 1: Automated Static Analysis

- Utilized a custom AI-powered bot for initial vulnerability detection
- Performed automated static analysis to quickly identify common security patterns and potential issues

### Phase 2: Invariant Testing

- Conducted systematic testing to verify that all contract invariants remain intact
- Validated core business logic and security assumptions
- Ensured that fundamental contract properties hold under various conditions

### Phase 3: Manual Code Review

- Performed deep manual analysis of the entire codebase
- Identified vulnerabilities that automated tools miss
- Validated findings from previous phases and discovered additional issues

This multi-layered approach combines the efficiency of automated tools with the thoroughness of manual review, ensuring comprehensive coverage of both common and sophisticated vulnerabilities.

---

## Executive Summary

### Total Issues Found: 7

- 1 CRITICAL severity issue
- 2 MEDIUM severity issues
- 3 ADMIN-RELATED issues
- 1 LOW severity issue
- 4 GAS Related issues

---

## Critical Issues

### 1. [CRITICAL] Staking Lock Duration Reset Attack

**Contract**: `BATAStaking.sol`  
**Severity**: CRITICAL

#### Description

The `stake()` function unconditionally resets the lock timestamp for ALL previously staked tokens whenever new tokens are added. This completely breaks the 60-day holding requirement for profit-sharing eligibility.

#### Vulnerable Code

```solidity
function stake(uint256 amount) external nonReentrant {
    require(amount > 0, "amount zero");
    StakeInfo storage info = stakes[msg.sender];
    info.amount += amount;
    info.timestamp = block.timestamp; // <-- RESETS ENTIRE LOCK PERIOD
    bata.safeTransferFrom(msg.sender, address(this), amount);
    emit Staked(msg.sender, amount);
}
```

#### Attack Scenario

1. Alice stakes 10,000 BATA on day 1
2. On day 59 (1 day before unlock), Alice stakes 1 wei of BATA
3. Lock timer resets to day 59 for ALL 10,000+ tokens
4. Alice must wait another 60 days to withdraw anything

#### Proof of Concept

```solidity
// Day 1: Stake 10,000 BATA
staking.stake(10000 ether);
// stakes[alice].timestamp = Day 1

// Day 59: Stake 1 wei
staking.stake(1);
// stakes[alice].timestamp = Day 59 (RESET!)
// stakes[alice].amount = 10000 ether + 1 wei

// Day 60: Try to unstake
staking.unstake(); // REVERTS: "lock not expired"
// Must wait until Day 119 (59 + 60)
```

#### Impact

- Users lose accumulated lock time
- Makes incremental staking impossible

#### Recommendation

Track individual stake entries with separate timestamps:

```solidity
struct Stake {
    uint256 amount;
    uint256 unlockTime;
}
mapping(address => Stake[]) public stakes;
```

---

## Medium Issues

### 2. [MEDIUM] Missing Slippage Protection for BATA Price Changes

**Contract**: `BATAStageSale.sol`  
**Severity**: MEDIUM

#### Description

Users have no protection against BATA token price changes between transaction submission and execution. **Important: This is NOT about USDT/USDC volatility** (they are stablecoins). This is about the admin changing the **BATA sale price** via governance.

#### Understanding the Issue

The `buy()` function uses a configurable price to calculate how many BATA tokens the user receives:

```solidity
function buy(uint8 idx, uint256 amount, bool useUSDT) external nonReentrant {
    // 'price' = How much USDT/USDC for 1 BATA token
    // Example: price = 2e18 means 2 USDT per 1 BATA
    uint256 price = useUSDT ? s.priceUSDT : s.priceUSDC;

    // Calculate BATA tokens based on payment amount
    // If user pays 100 USDT and price = 2e18, they get 50 BATA
    uint256 tokensOut = amount * 1e18 / price;
    // NO CHECK: require(tokensOut >= minExpected)
}
```

#### The Problem Scenario

1. **User checks price**: 1 USDT = 1 BATA (price = 1e18)
2. **User submits transaction**: Pays 100 USDT expecting 100 BATA
3. **Admin updates price**: Changes to 10 USDT = 1 BATA (price = 10e18)
4. **User's transaction executes**: Gets only 10 BATA instead of 100 (90% loss!)

#### Why This Can Happen

- Admin can change BATA price through `setStagePriceUSDT()` function
- Even with timelock delay, pending transactions use the new price
- Users cannot specify minimum acceptable BATA tokens

#### Impact

- Users can receive up to 90% fewer BATA tokens than expected

#### Recommendation

```solidity
function buy(uint8 idx, uint256 amount, bool useUSDT, uint256 minTokensOut) external {
    uint256 tokensOut = amount * 1e18 / price;
    require(tokensOut >= minTokensOut, "Price changed - slippage exceeded");
}
```

---

### 3. [MEDIUM] Unbounded Loop DoS

**Contract**: `BATAStageSale.sol`  
**Severity**: MEDIUM

#### Description

The whitelist functions have unbounded loops. An attacker with governance votes could propose massive arrays to DoS the contract.

#### Vulnerable Code

```solidity
function addWhitelist(address[] calldata addrs) external onlyOwner {
    for (uint256 i=0;i<addrs.length;i++) {  // No limit!
        whitelist[addrs[i]] = true;
    }
}
```

#### Impact

- Gas exhaustion causes failed transactions

#### Recommendation

```solidity
require(addrs.length <= 100, "Too many addresses");
```

---

## Admin-Related Issues

### 4. [ADMIN] RescueToken Can Drain User Funds

**Contract**: `BATAStageSale.sol`  
**Severity**: Critical (if admin is malicious)

#### Description

The `rescueToken()` function is intended to recover accidentally sent tokens, but it can also withdraw BATA tokens that belong to users waiting to claim after purchase.

#### The Problem

```solidity
function rescueToken(address token, address to, uint256 amount) external onlyOwner {
    IERC20(token).safeTransfer(to, amount);  // No restrictions on which token!
}
```

#### How User Funds Are At Risk

1. **Before TGE**: When users buy tokens before TGE, their BATA is stored in the contract
2. **Contract holds BATA**: The sale contract must hold BATA tokens for future claims
3. **Admin can withdraw**: Nothing prevents admin from calling `rescueToken(BATA_ADDRESS, adminWallet, totalAmount)`

#### Attack Scenario

1. Users purchase 10,000 BATA total (stored as `claimable` in contract)
2. Contract holds 10,000 BATA for these pending claims
3. Admin calls `rescueToken(bataAddress, adminWallet, 10000e18)`
4. All BATA tokens are withdrawn to admin wallet
5. When users try to claim after TGE, transaction fails - no tokens left!

---

### 5. [ADMIN] Merkle Root Updates After Claims

**Contract**: `MerkleAirdropTimelock.sol`  
**Severity**: High (if admin is malicious)

#### Description

Admin can change the merkle root after the airdrop has started, potentially removing users who haven't claimed yet from the distribution list.

#### How Merkle Airdrops Work

- A merkle tree contains all eligible addresses and their allocated amounts
- The root hash of this tree is stored in the contract
- Users provide merkle proofs to claim their allocation

#### The Problem

```solidity
function updateMerkleRoot(bytes32 root) external onlyOwner {
    merkleRoot = root;  // Can be changed anytime!
}
```

#### Attack Scenario

1. Airdrop starts with 1000 eligible users
2. 500 users claim their tokens successfully
3. Admin updates merkle root with a new tree excluding remaining 500 users
4. Remaining users' proofs no longer work - they can't claim
5. Admin can redirect unclaimed tokens elsewhere

---

### 6. [ADMIN] TGE Timestamp Changes

**Contract**: `BATAStageSale.sol`  
**Severity**: Medium (if admin is malicious)

#### Description

Admin can change the Token Generation Event (TGE) timestamp, affecting when users can claim their purchased tokens.

#### How TGE Works

```solidity
if (tgeTimestamp != 0 && block.timestamp >= tgeTimestamp) {
    // After TGE: Immediate token delivery
    bata.safeTransfer(msg.sender, tokensOut);
} else {
    // Before TGE: Tokens locked for claiming later
    claimable[msg.sender][idx] += tokensOut;
}
```

#### The Problem

Admin can change TGE timestamp via `setTgeTimestamp()`:

- Setting to past: New buyers get instant tokens while early buyers wait
- Setting to future: Delays token claims for everyone

#### Attack Scenario

1. Users buy tokens expecting TGE in January
2. Admin changes TGE to December (past)
3. New buyers get tokens immediately
4. Early buyers still need to manually claim
5. Creates unfair advantage for later participants

---

## Low Issues

### 7. [Low] Use Ownable2Step instead of Ownable

**Contract**: `BATAStageSale.sol`  
**Severity**: low

#### Description:

The `Ownable2Step` pattern involves a two-step process where the current owner proposes a new owner, and the new owner must accept the proposal to complete the transfer. This mechanism requires active participation from both parties, significantly reducing the risk of unintended or unauthorized ownership changes.

#### Recommendation:

use `Ownable2Step` instead of `Ownable`

```solidity
contract BATAStageSale is Ownable, ReentrancyGuard {
```

## Gas Optimization Issues

### 8. [Gas Saving] Nesting if-statements is cheaper than using &&

**Contract**: `BATAStageSale.sol`  
**Severity**: Gas

#### Description:

Nesting if-statements avoids the stack operations of setting up and using an extra jumpdest, and saves 6 [gas](https://gist.github.com/IllIllI000/7f3b818abecfadbef93b894481ae7d19)

There are total 5 instances of this issue

### 9. [Gas Saving] Cache array length outside of loop

**Contract**: `BATAStageSale.sol`  
**Severity**: Gas

#### Description:

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

There are total 2 instances of this issue

### 10. [Gas Saving] Use Custom Errors

**Contract**: `Whole Codebase`  
**Severity**: Gas

#### Description:

[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

There are total 62 instances of this issue

### 11. [Gas Saving] Using `private` rather than `public` for constants, saves gas

**Contract**: `BATA.sol, BATAStaking.sol `  
**Severity**: Gas

#### Description:

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

There are total 10 instances of this issue

#### Resolution:

Acknowledge
