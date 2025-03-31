

# WyvernBuyAndBurn Audit Report 

## Executive Summary

A security review of the smart contract protocol was done by [0xepley](https://twitter.com/0xepley).  
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.


## Test Approach and Methodology

The security review was conducted using a combination of static analysis tools and comprehensive manual review of the smart contract code. The audit focused on identifying potential vulnerabilities, edge cases, and code optimization opportunities in the `WyvernBuyAndBurn` implementation.

Key components of the methodology included:
- Automated scanning using industry-standard static analysis tools
- Manual code review line-by-line with particular attention to token handling mechanisms
- Evaluation of function validations and input sanitization
- Analysis of gas optimization opportunities
- Assessment of adherence to smart contract development best practices


## Disclaimer

"Audits are a time, resource and expertise bound effort where trained experts evaluate smart
contracts using a combination of automated and manual techniques to find as many vulnerabilities
as possible. Audits can show the presence of vulnerabilities **but not their absence**."



## Findings

### Medium Severity Issues

#### 1. Insufficient TitanX Balance Validation

**Issue**: The function only checks that TitanX balance is greater than 0, but doesn't validate if it's sufficient for balanced liquidity.

**Code Location**:
```solidity
uint256 titanContractBalanceForInitialLiquidity = titanX.balanceOf(address(this));
require(titanContractBalanceForInitialLiquidity > 0, "Provide TitanX Liquidity");
```

**Detailed Explanation**:
When creating a Uniswap pool, the ratio between the two tokens determines the initial price. With only a check that the TitanX balance is greater than zero, there's a risk of creating a pool with severely imbalanced liquidity. For example:

- If `initialWyvernXLiquidityAmount` is 1,000,000 tokens but the TitanX balance is only 1 token, the pool would be created with an extreme price skew.
- This imbalance could lead to:
  - Suboptimal liquidity concentration (most liquidity would be outside the active trading range)
  - Arbitrage opportunities against the pool immediately after creation
  - Poor price discovery and execution for users

Even with a trusted owner, having proportional validation serves as a safeguard against accidental misconfiguration.

**Recommendation**: Add a proportionality check

**Status**: Pending



#### 2. Tight Deadline Parameter in Uniswap Swaps

**Issue**: The deadline for Uniswap operations in the `buyAndBurnWyvernX` function is set to just 1 second after the current block timestamp.

**Code Location**:
```solidity
ISwapRouter.ExactInputParams memory params = ISwapRouter
    .ExactInputParams({
    path: path,
    recipient: address(this),
    deadline: block.timestamp + 1,
    amountIn: amountIn,
    amountOutMinimum: amountOutMinimum
});
```

**Detailed Explanation**:
This extremely tight deadline creates several operational risks:

- **Transaction Failure**: Ethereum blocks are mined approximately every 12 seconds, making it highly likely that transactions will fail due to expired deadlines during normal network operation.
  
- **Network Congestion Impact**: During periods of high network congestion, transactions may take minutes or even hours to be included in a block, making the 1-second deadline practically impossible to meet.


**Recommendation**: Extend the deadline to a more reasonable timeframe:

```solidity
deadline: block.timestamp + 15 minutes,
```

This balances providing sufficient time for transaction inclusion while protecting against executing at extremely unfavorable prices after long delays.

**Status**: Acknowledged


### Informational Issues

#### 1. Missing Event Emission

**Issue**: No event is emitted for this critical one-time initialization.

**Code Location**: End of `createUniswapLiquidity` function

**Recommendation**: Add and emit an event for better off-chain tracking:

```solidity
event LiquidityInitialized(
    uint256 indexed titanAmount, 
    uint256 indexed wyvernAmount, 
    uint256 tokenId, 
    uint256 liquidity
);

// At the end of createUniswapLiquidity
emit LiquidityInitialized(
    titanContractBalanceForInitialLiquidity,
    initialWyvernXLiquidityAmount,
    tokenLiquidityInfo.tokenId,
    tokenLiquidityInfo.liquidity
);
```



