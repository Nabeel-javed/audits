# GELT Security Review

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Assessment Summary](#assessment-summary)
3. [Test Approach and Methodology](#test-approach-and-methodology)
4. [Disclaimer](#disclaimer)
5. [About GELT](#about-gelt)
6. [Risk Classification](#risk-classification)
   - [Impact](#impact)
   - [Likelihood](#likelihood)
   - [Actions Required by Severity Level](#actions-required-by-severity-level)
7. [Overview](#overview)
8. [Issues Found](#issues-found)
9. [Findings](#findings)
   - [Medium Severity](#medium-severity)
     - [M-01 Unsafe ERC20 Token Transfers Using `.call()`](#issue-2-unsafe-erc20-token-transfers-using-call)
   - [Low Severity](#low-severity)
     - [L-01 Lack of Validation in `updateTLWallet()` and `updateTaxWallet()` Functions](#issue-2-lack-of-validation-in-updatetlwallet-and-updatetaxwallet-functions-already-in-known-issues)
   - [Gas Saving](#gas-severity)
     - [G-01 Cache Array Length Outside of Loop](#issue-1-cache-array-length-outside-of-loop)
     - [G-02 Nesting if-statements is Cheaper than Using &&](#issue-2-nesting-if-statements-is-cheaper-than-using-)

## Executive Summary

A security review of the Gelt smart contract protocol was done by [0xepley](https://twitter.com/0xepley).  
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

## Assessment Summary

The GELT protocol security review revealed a moderate level of security with one medium-severity issue related to unsafe ERC20 token transfers using `.call()` which could lead to failures with non-standard tokens like USDT, BNB, and MKR. One low-severity issue was identified regarding a lack of validation in wallet address update functions, which could result in setting zero addresses. Additionally, two gas optimization opportunities were found that could improve contract efficiency. All identified issues have been resolved by implementing the recommended fixes, indicating a responsive development approach to security concerns.

## Test Approach and Methodology

The security review was conducted using a combination of static analysis tools and comprehensive manual review of the smart contract code. The audit focused on identifying potential vulnerabilities, edge cases, and code optimization opportunities in the GELT protocol implementation.

Key components of the methodology included:
- Automated scanning using industry-standard static analysis tools
- Manual code review line-by-line with particular attention to token handling mechanisms
- Evaluation of function validations and input sanitization
- Analysis of gas optimization opportunities
- Assessment of adherence to smart contract development best practices

The review was conducted against the code in the [TreasuryLari.sol](https://github.com/MDulquerS/TreasuryLari-StableCoin/blob/main/src/TreasuryLari.sol) repository.

## Disclaimer

"Audits are a time, resource and expertise bound effort where trained experts evaluate smart
contracts using a combination of automated and manual techniques to find as many vulnerabilities
as possible. Audits can show the presence of vulnerabilities **but not their absence**."

\- Secureum

## About GELT
GELT is built on a decentralized blockchain infrastructure designed to ensure transparency, security, and immutability of transactions. By leveraging the power of blockchain, GELT aims to create an efficient financial ecosystem.


## Risk classification

| Severity           | Impact: High | Impact: Medium | Impact: Low |
| :----------------- | :----------: | :------------: | :---------: |
| Likelihood: High   |   Critical   |      High      |   Medium    |
| Likelihood: Medium |     High     |     Medium     |     Low     |
| Likelihood: Low    |    Medium    |      Low       |     Low     |

### Impact

- **High** - leads to a significant material loss of assets in the protocol or significantly harms a group of users.
- **Medium** - only a small amount of funds can be lost (such as leakage of value) or a core functionality of the protocol is affected.
- **Low** - can lead to any kind of unexpected behaviour with some of the protocol's functionalities that's not so critical.

### Likelihood

- **High** - attack path is possible with reasonable assumptions that mimic on-chain conditions and the cost of the attack is relatively low to the amount of funds that can be stolen or lost.
- **Medium** - only conditionally incentivized attack vector, but still relatively likely.
- **Low** - has too many or too unlikely assumptions or requires a huge stake by the attacker with little or no incentive.

### Actions required by severity level

- **High** - client **must** fix the issue.
- **Medium** - client **should** fix the issue.
- **Low** - client **could** fix the issue.

## Overview

| Attribute     | Details |
|--------------|---------|
| **Project Name** | GELT |
| **Repository** | [TreasuryLari.sol](https://github.com/MDulquerS/TreasuryLari-StableCoin/blob/main/src/TreasuryLari.sol) |
| **Methods** | Static Analysis + Manual Review |


### Issues found

| Severity      |                                                     Count |
| :------------ | --------------------------------------------------------: |
| High risk     | 0      |
| Medium risk   | 1 |
| Low risk      | 1    |
| Gas Saving | 2 |

### Issues Summary


| **Severity**    | **Title** |
|---------------|-----------------------------------------------------------------|
| **Medium Risk** | [M-01] Unsafe ERC20 Token Transfers Using `.call() |
| **Low Risk** | [L-01] Lack of Validation in `updateTLWallet()` and `updateTaxWallet()` Functions |
| **Gas Saving** | [G-01] Cache array length outside of loop |
| **Gas Saving** | [G-02] Nesting if-statements is cheaper than using && |


# Findings

## Medium severity

## Issue 2: Unsafe ERC20 Token Transfers Using `.call()`
### Overview
In Solidity, using `.call()` to transfer ERC20 tokens is **not recommended** because it can fail for **non-standard ERC20 tokens** like USDT, BNB, and MKR. Some ERC20 tokens do not return a boolean (`true`) on `transfer()`, causing `.call()` to fail unexpectedly.  

```solidity
(bool success, bytes memory data) = address(token).call(
    abi.encodeWithSelector(IERC20.transfer.selector, to, tbalance)
);
if (!success || (data.length != 0 && !abi.decode(data, (bool)))) {
    revert TransferFailed();
}
```


### Why `.call()` is Problematic?
1. **Non-Standard ERC20 Behavior**  
   - Some tokens do not return a boolean (`true`) on `transfer()`.
   - `.call()` expects a return value and fails if the token does not provide it.

2. **Lack of Compatibility with Certain ERC20 Tokens**  
   - Tokens like **USDT, MKR, and BNB** do not strictly adhere to ERC20's expected `transfer()` return value.
   - `.call()` will **fail on these tokens**.


### Recommended Fix:
```solidity
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

function withdrawStuckTokens(address token, address to) external onlyOwner {
    require(to != address(0), "Invalid recipient");

    uint256 tbalance = IERC20(token).balanceOf(address(this));
    require(tbalance > 0, "No token balance");

    SafeERC20.safeTransfer(IERC20(token), to, tbalance);
}
```
### Resolution
Solved by removing `.call` function.

## Low Severity

## Issue 2: Lack of Validation in `updateTLWallet()` and `updateTaxWallet()` Functions (Already in known issues)

### Overview
The `updateTLWallet()` and `updateTaxWallet()` functions allow the owner to change the treasury and tax wallet addresses. However, these functions **do not validate the new addresses**, which introduces potential risks.  


## Issue Details  
**Risk of Setting `address(0)` (Zero Address)**
- The contract **does not check** whether the new wallet address is `address(0)`.  
- If the owner mistakenly or maliciously sets the treasury or tax wallet to `0x0000000000000000000000000000000000000000`, all future operations related to these wallets will **fail**.  


## Proposed Fix

**Add Input Validation in `updateTLWallet()`**
```solidity
function updateTLWallet(address newWallet) external onlyOwner {
    require(newWallet != address(0), "Invalid wallet address");
    require(newWallet != TLWallet, "New wallet must be different from the current one");
    
    TLWallet = newWallet;
    emit UpdatedTLWallet(newWallet);
}
```

 **Add Input Validation in `updateTaxWallet()`**
```solidity
function updateTaxWallet(address newWallet) external onlyOwner {
    require(newWallet != address(0), "Invalid tax wallet address");
    require(newWallet != taxWallet, "New wallet must be different from the current one");

    taxWallet = newWallet;
    emit UpdatedTaxWallet(newWallet);
}
```


### Resolution
Solved by adding 0 address check

## Gas Severity

## Issue 1: Cache array length outside of loop

## Overview:
In the `blockUsers` and `unblockUsers` function the array is not being cached before the loop and is being read from memory on every iteration. So the solidity compiler will always read the length of the array during each iteration. This is an extra mload operation (3 additional gas for each iteration except for the first).

### Resolution
Solved by caching the array outside the loop

## Issue 2: Nesting if-statements is cheaper than using &&

## Overview:

In `_update`, `enableRTFee` and `enableSFee` functions are using `And` condition which will consume more gas as compared to nested if-statements. Nesting if-statements avoids the stack operations of setting up and using an extra jumpdest, and saves 6 [gas](https://gist.github.com/IllIllI000/7f3b818abecfadbef93b894481ae7d19)
