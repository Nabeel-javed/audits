# DeadToken Smart Contract Security Audit Report


## Executive Summary

The `OracleManager` contract is a multi-oracle price aggregator that fetches token prices from Chainlink, Pyth, and Uniswap V3 with fallback logic. The audit identified **1 High** and **3 Medium** severity issues related to price manipulation, stale data, and configuration logic flaws.


### Disclaimer

This audit represents a point-in-time assessment. Smart contracts require ongoing security monitoring and regular audits. No audit can identify 100% of vulnerabilities, so multiple audits are recommended.


## Table of Contents

- [Disclaimer](#disclaimer)
- [Scope and Context](#scope-and-context)
- [Findings](#findings)
  - [1) Immediate drain of platform/vesting/initial-mint allocations via idle-claim due to missing lastTransferTime updates on mint](#1-immediate-drain-of-platformvestinginitial-mint-allocations-via-idle-claim-due-to-missing-lasttransfertime-updates-on-mint)
  - [2) Medium — Overpayment beyond fee.nativeFee is trapped](#2-medium--overpayment-beyond-feenativefee-is-trapped)
  - [3) Medium — Idle-claim timer can be indefinitely refreshed via zero-value transfers](#3-medium--idle-claim-timer-can-be-indefinitely-refreshed-via-zero-value-transfers)
  - [4. [GAS] Nesting if-statements is cheaper than using &&](#4-gas-nesting-if-statements-is-cheaper-than-using-)


## Scope and Context

- In-scope file: `OracleManager.sol` (contract `UsDead.sol`, Solidity 0.8.20)

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

- Performed deep manual analysis of the entire code
- Identified vulnerabilities that automated tools miss
- Validated findings from previous phases and discovered additional issues

This multi-layered approach combines the efficiency of automated tools with the thoroughness of manual review, ensuring comprehensive coverage of both common and sophisticated vulnerabilities.

---



## Findings

### [H-01] Uniswap V3 `slot0()` Price Is Manipulatable via Flash Loans

**Severity:** High
**Location:** [OracleManager.sol:162-174](OracleManager.sol#L162-L174)

**Description:**
The `_getUniswapPriceRaw()` function uses `slot0()` to fetch the current spot price from Uniswap V3. This price represents the **instantaneous** price at the current block and can be trivially manipulated using flash loans.

```solidity
try IUniswapV3Pool(pool).slot0() returns (
    uint160 s,  // sqrtPriceX96 - manipulatable spot price
    ...
) {
    sqrtPriceX96 = s;
}
```

**Attack Scenario:**
1. Attacker takes a flash loan
2. Attacker performs a large swap on the Uniswap pool to skew the price
3. Chainlink and Pyth feeds are either stale or unavailable (common during high volatility)
4. The contract falls back to the manipulated Uniswap price
5. Any protocol using this oracle makes decisions based on the manipulated price

**Recommendation:**
Use Uniswap V3's TWAP (Time-Weighted Average Price) via the `observe()` function instead of `slot0()`:

```solidity
function _getUniswapTWAP(address pool, uint32 twapInterval) internal view returns (uint256) {
    uint32[] memory secondsAgos = new uint32[](2);
    secondsAgos[0] = twapInterval;
    secondsAgos[1] = 0;

    (int56[] memory tickCumulatives, ) = IUniswapV3Pool(pool).observe(secondsAgos);
    int56 tickCumulativesDelta = tickCumulatives[1] - tickCumulatives[0];
    int24 avgTick = int24(tickCumulativesDelta / int56(uint56(twapInterval)));

    // Convert tick to price...
}
```

A TWAP interval of 30 minutes is generally recommended to resist manipulation.

---

### [M-01] Missing Staleness Check on ETH/USD Chainlink Feed

**Severity:** Medium
**Location:** [OracleManager.sol:216-228](OracleManager.sol#L216-L228)

**Description:**
The `_getEthUsdPriceInternal()` function does NOT check if the ETH/USD price is stale, unlike `_getChainlinkPriceInternal()` which properly validates `block.timestamp - updatedAt < 1 hours`.

```solidity
function _getEthUsdPriceInternal() internal view returns (uint256) {
    try ethUsdFeed.latestRoundData() returns (
        uint80,
        int256 price,
        uint256,
        uint256,  // updatedAt - IGNORED!
        uint80
    ) {
        if (price > 0)
            return _scaleTo1e18(uint256(price), ethUsdFeed.decimals());
    } catch {}
    return 0;
}
```

**Impact:**
If the ETH/USD Chainlink feed becomes stale (e.g., during network congestion or oracle outages), the contract will return outdated ETH prices. Since this feed is used to convert ETH-based prices to USD, all tokens configured with `chainlinkIsEthBased = true` would return incorrect prices.

**Recommendation:**
Add staleness validation consistent with the token price feeds:

```solidity
function _getEthUsdPriceInternal() internal view returns (uint256) {
    try ethUsdFeed.latestRoundData() returns (
        uint80,
        int256 price,
        uint256,
        uint256 updatedAt,
        uint80
    ) {
        if (price > 0 && block.timestamp - updatedAt < 1 hours)
            return _scaleTo1e18(uint256(price), ethUsdFeed.decimals());
    } catch {}
    return 0;
}
```

---

### [M-02] `uniswapIsToken0` Configuration Parameter Is Never Used

**Severity:** Medium
**Location:** [OracleManager.sol:44](OracleManager.sol#L44), [OracleManager.sol:183-184](OracleManager.sol#L183-L184)

**Description:**
The `TokenOracleConfig` struct contains a `uniswapIsToken0` field that is set via `setTokenConfig()`, but this value is **never read**. Instead, `_getUniswapPriceRaw()` dynamically determines `isToken0` by comparing addresses:

```solidity
// Config field exists but is never used:
bool uniswapIsToken0; // If true, we calculate Price = Token1/Token0

// In _getUniswapPriceRaw():
address t0 = IUniswapV3Pool(pool).token0();
isToken0 = (token == t0);  // Determined dynamically, config ignored
```

**Impact:**
This is either:
1. **Dead code** - wasting gas on storage
2. **A logic bug** - if the intention was to allow manual override of the price direction

If the pool's token order doesn't match expectations (e.g., wrapped tokens, proxy tokens), the admin has no way to correct the price calculation direction.

**Recommendation:**
Either remove the unused field or implement the intended logic:

```solidity
function _getUniswapPriceRaw(
    address token,
    address pool,
    bool useConfigIsToken0,
    bool configIsToken0
) internal view returns (uint256) {
    // ...
    bool isToken0 = useConfigIsToken0 ? configIsToken0 : (token == t0);
    // ...
}
```

---

### [M-03] `chainlinkIsEthBased` Flag Incorrectly Applied to Uniswap Price

**Severity:** Medium
**Location:** [OracleManager.sol:145-149](OracleManager.sol#L145-L149)

**Description:**
The `_getUniswapPriceInternal()` function reuses `cfg.chainlinkIsEthBased` to determine whether to multiply the Uniswap price by ETH/USD:

```solidity
function _getUniswapPriceInternal(...) internal view returns (uint256) {
    if (cfg.uniswapPool != address(0)) {
        uint256 uniPrice = _getUniswapPriceRaw(token, cfg.uniswapPool);
        if (uniPrice > 0) {
            if (cfg.chainlinkIsEthBased) {  // Wrong assumption!
                uint256 ethUsd = _getEthUsdPriceInternal();
                if (ethUsd == 0) return 0;
                return (uniPrice * ethUsd) / 1e18;
            }
            return uniPrice;
        }
    }
    return 0;
}
```

**Impact:**
A Uniswap pool might be TOKEN/USDC (USD-denominated) while the Chainlink feed is TOKEN/ETH (ETH-denominated). The current logic assumes both oracles have the same denomination, which may not be true. This would result in:
- Double conversion (ETH→USD applied to already USD-denominated price)
- Or missing conversion (USD price returned when ETH conversion was needed)

**Recommendation:**
Add a separate configuration flag for Uniswap price denomination:

```solidity
struct TokenOracleConfig {
    // ... existing fields ...
    bool uniswapIsEthBased;  // Separate flag for Uniswap
}
```

---

---




### [M-08] Pyth `getPriceUnsafe` Doesn't Validate Price Authenticity

**Severity:** Medium
**Location:** [OracleManager.sol:112](OracleManager.sol#L112)

**Description:**
The contract uses `getPriceUnsafe()` which, per Pyth documentation, returns the price **without verifying** that it has been properly updated on-chain:

```solidity
try pyth.getPriceUnsafe(cfg.pythId) returns (IPyth.Price memory p) {
```

From Pyth docs:
> "This function returns the price without any sanity checks... It is vulnerable to returning stale or unverified prices."

While the contract checks `publishTime` for staleness, it doesn't verify that the price update transaction was actually submitted. An attacker could potentially exploit race conditions where the "latest" price in storage is stale or manipulated.

**Recommendation:**
Use `getPrice()` or `getPriceNoOlderThan()` which include verification:

```solidity
try pyth.getPriceNoOlderThan(cfg.pythId, threshold) returns (IPyth.Price memory p) {
```

---
---

### [M-09] Chainlink `roundId` Not Validated

**Severity:** Medium
**Location:** [OracleManager.sol:85-91](OracleManager.sol#L85-L91)

**Description:**
The contract ignores `roundId` from Chainlink's response:

```solidity
try cfg.chainlinkFeed.latestRoundData() returns (
    uint80,        // roundId - IGNORED
    int256 price,
    uint256,       // startedAt - IGNORED
    uint256 updatedAt,
    uint80         // answeredInRound - IGNORED
) {
```

**Missing validations:**
1. `roundId > 0` - Ensures valid round data

Without these checks, the oracle might return stale data from a previous round that hasn't been updated, even if `updatedAt` appears recent due to Chainlink's internal mechanics.

**Recommendation:**
```solidity
try cfg.chainlinkFeed.latestRoundData() returns (
    uint80 roundId,
    int256 price,
    uint256,
    uint256 updatedAt,
    uint80 answeredInRound
) {
    if (price > 0 &&
        block.timestamp - updatedAt < 1 hours &&
        roundId > 0) {
        // ... process price
    }
}
```

---

### [M-10] No Confidence Interval Check for Pyth Prices

**Severity:** Medium
**Location:** [OracleManager.sol:112-133](OracleManager.sol#L112-L133)

**Description:**
Pyth provides a confidence interval (`conf`) with each price, representing the uncertainty range. The contract ignores this:

```solidity
struct Price {
    int64 price;
    uint64 conf;    // IGNORED - confidence interval
    int32 expo;
    uint256 publishTime;
}
```

During high volatility, Pyth may report prices with very wide confidence intervals (e.g., price = $100, conf = $50, meaning actual price is somewhere between $50-$150). Using such uncertain prices without validation can lead to exploits.

**Recommendation:**
Add confidence threshold check:

```solidity
// Reject if confidence is > 5% of price
if (p.conf > uint64(p.price) / 20) return 0;
```

---

## Informational / Low Severity (Not Detailed)

| ID | Issue | Severity |
|----|-------|----------|
| L-01 | No zero-address validation in `setEthUsdFeed()` and `setTokenConfig()` | Low |
| L-02 | No event emission on configuration changes | Low |
| L-05 | No validation that `ethUsdFeed` is set before use | Low |
| I-01 | Consider adding a circuit breaker for emergency price freezing | Informational |

---


