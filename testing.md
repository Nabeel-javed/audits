# Security Review — Revert Lend (Aerodrome Slipstream)

---

## Scope

|                                  |                                                        |
| -------------------------------- | ------------------------------------------------------ |
| **Mode**                         | ALL / default                                          |
| **Files reviewed**               | `GaugeManager.sol` · `InterestRateModel.sol` · `V3Oracle.sol`<br>`V3Vault.sol` · `AutoExit.sol` · `Automator.sol`<br>`AutoRangeAndCompound.sol` · `LeverageTransformer.sol`<br>`Transformer.sol` · `V3Utils.sol` · `ChainlinkFeedCombinator.sol`<br>`Constants.sol` · `FlashloanLiquidator.sol` · `Swapper.sol` |
| **Confidence threshold (1-100)** | 80                                                     |

---

## Findings

[95] **1. Staked Position Liquidation Uses Stale Fee Valuation — Liquidator Receives Unpriced Fee Windfall**

`V3Vault.liquidate` · Confidence: 95

**Description**

When liquidating a staked position, `_checkLoanIsHealthy` (line 742) calls the oracle with `ignoreFees=true`, returning `feeValue=0` and a `fullValue` that excludes accumulated swap fees. Then `_unstakeIfNeeded` (line 748) returns the NFT to the vault, making those fees collectable again. But `_sendPositionValue` (line 770) still uses the stale `feeValue=0`. In a partial liquidation the code enters the else branch at line 1127 and sets `fees0=fees1=type(uint128).max` (collect ALL fees) plus proportional liquidity. The liquidator receives the computed proportional liquidity PLUS all now-collectable fees — value that was never priced into the liquidation economics. This has three sub-impacts: (a) the liquidator extracts unaccounted fee value from the borrower at zero cost, (b) positions can be prematurely liquidated because fee-excluded collateral value is lower than total value, and (c) `_calculateLiquidation` may enter the undercollateralized branch and trigger reserve socialization (lender haircuts) when the true position value including fees would have been sufficient.

Concrete example: staked position with 1000 USDC liquidity value + 50 USDC accumulated fees, debt = 800 USDC. Oracle returns `fullValue=1000, feeValue=0`. Partial liquidation with `liquidationValue=880`. After unstaking, `collect` with `fees=max` extracts ALL 50 USDC of fees + 88% of liquidity (~880 USDC). Liquidator receives ~930 USDC while only paying for 880 USDC worth. [agents: 6]

**Fix**

```diff
  // V3Vault.sol – liquidate()
  _unstakeIfNeeded(params.tokenId);
+ // Re-query oracle after unstaking to get accurate fee-inclusive valuation
+ (fullValue, feeValue, , collateralValue) = _checkLoanIsHealthy(
+     params.tokenId, state.debt, false  // ignoreFees=false post-unstake
+ );
  (state.liquidationValue, state.liquidatorCost, state.reserveCost) =
      _calculateLiquidation(state.debt, fullValue, collateralValue);
```

---

[85] **2. Multi-Hop Reward Swap Compounds Per-Hop Slippage Tolerance (~4% vs Intended ~2%)**

`GaugeManager._swapAeroToTarget` · Confidence: 85

**Description**

When no direct reward base pool exists for a target token, `_swapAeroToTarget` (line 414-416) performs a two-hop swap: AERO->otherToken via `intermediatePool`, then otherToken->targetToken via `positionPool`. Each hop independently validates TWAP with `REWARD_MAX_TWAP_TICK_DIFFERENCE=200` ticks and `REWARD_MAX_PRICE_DIFFERENCE_X64=Q64/50` (~2%). The combined worst-case slippage compounds: `1-(1-1/50)^2 = 3.96%`, nearly double the intended per-hop protection. A sandwich attacker can manipulate both pools within their individual TWAP tolerances to extract up to ~4% of compounded rewards per operation. [agents: 2]

**Fix**

```diff
  // GaugeManager.sol – _swapAeroToTarget(), after both hops complete
+ // End-to-end slippage check on combined swap output
+ uint256 expectedOut = _getExpectedOutputFromOracle(aeroAmount, targetToken);
+ uint256 combinedMinOut = expectedOut * (Q64 - REWARD_MAX_PRICE_DIFFERENCE_X64) / Q64;
+ if (amountOut < combinedMinOut) revert CombinedSlippageExceeded();
```

---

[80] **3. Transformer.setVault Is Irrevocable — Compromised Vaults Cannot Be Deactivated**

`Transformer.setVault` · Confidence: 80

**Description**

`setVault` (line 21-24) sets `vaults[_vault] = true` but provides no mechanism to set it to `false`. The `vaults` mapping gates authorization in `_validateCaller`, `_validateOwner`, and all vault-interaction entry points across `AutoRangeAndCompound`, `V3Utils`, and `LeverageTransformer`. If a vault address is compromised, deprecated, or needs rotation, it remains permanently trusted. A compromised vault could return attacker-controlled `ownerOf` values to bypass authorization and manipulate positions. Compare with `V3Vault.setTransformer` which correctly has an `active` boolean toggle. [agents: 2]

**Fix**

```diff
- function setVault(address _vault) external onlyOwner {
-     vaults[_vault] = true;
+ function setVault(address _vault, bool _active) external onlyOwner {
+     vaults[_vault] = _active;
  }
```

---

[75] **4. autoCompound Pool Swap Uses Zero Minimum Output**

`AutoRangeAndCompound.autoCompound` · Confidence: 75

**Description**

`_poolSwap` is called with `amountOutMin: 0` (line 470) relying solely on TWAP deviation checks. The TWAP check validates the pre-swap pool price is within `maxTWAPTickDifference` of the TWAP, but does not bound the post-swap execution price. For large `amountIn` relative to pool liquidity, the swap can experience significant price impact beyond what the TWAP deviation check validated. The operator controls `amountIn` without upper bound relative to available fees, enabling potential value extraction through intentionally poor swap execution while staying within TWAP bounds. Partially mitigated by the operator being a semi-trusted role. [agents: 3]

---

Findings List

| # | Confidence | Title |
|---|---|---|
| 1 | [95] | Staked Position Liquidation Uses Stale Fee Valuation |
| 2 | [85] | Multi-Hop Reward Swap Compounds Per-Hop Slippage |
| 3 | [80] | Transformer.setVault Is Irrevocable |
| 4 | [75] | autoCompound Pool Swap Uses Zero Minimum Output |

---

## Leads

_Vulnerability trails with concrete code smells where the full exploit path could not be completed in one analysis pass. These are not false positives — they are high-signal leads for manual review. Not scored._

- **Reserve Socialization Underflow Can Block Liquidations** — `V3Vault._handleReserveLiquidation` — Code smells: missing underflow guard — If `missing >= totalLent` (bad debt exceeds entire lending pool), `(totalLent - missing)` underflows and reverts, permanently blocking that liquidation; requires catastrophically undercollateralized position.

- **setMaxPoolPriceDifference Accepts Extreme Values** — `V3Oracle.setMaxPoolPriceDifference` — Code smells: no lower/upper bound validation — Setting to `type(uint16).max` disables oracle manipulation protection entirely; setting to 0 blocks most operations. Owner-only but no misconfiguration guard.

- **AERO-As-Position-Token Creates Fragile Balance Accounting** — `GaugeManager.compoundRewards` — Code smells: overlapping balance tracking between leftoverAero, leftover0/1, and deposit deltas — When AERO is token0 or token1, `_sendDepositDeltas` and `_sendLeftoversAndRewards` draw from the same AERO balance; math appears to balance but is fragile and may revert under specific deposit-realized-fee scenarios.

- **Retroactive Collateral Factor Reduction** — `V3Vault.setTokenConfig` — Code smells: no timelock or gradual reduction — Owner can instantly reduce `collateralFactorX32`, making many positions simultaneously liquidatable without borrower recourse; no mechanism for borrowers to add collateral before the change takes effect.

- **unstakePosition Accessible By Any Address During Transform** — `V3Vault.unstakePosition` — Code smells: loose transform-mode guard — During active `_transform`, the check `transformedTokenId != tokenId` allows ANY caller to pass; practical impact is limited since `_unstakeIfNeeded` already unstakes before the transformer callback, but it is an access control violation.

- **Daily Borrow Limit Not Restored After Bad-Debt Liquidation** — `V3Vault.liquidate` — Code smells: `dailyDebtIncreaseLimitLeft += liquidatorCost` where `liquidatorCost` can be 0 for free liquidations — After bad-debt events, the full debt shares are removed but borrow headroom is not restored, progressively restricting protocol throughput (conservative behavior, not fund loss).

---

> This review was performed by an AI assistant. AI analysis can never verify the complete absence of vulnerabilities and no guarantee of security is given. Team security reviews, bug bounty programs, and on-chain monitoring are strongly recommended. For a consultation regarding your projects' security, visit [https://www.pashov.com](https://www.pashov.com)
