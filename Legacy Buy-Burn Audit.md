# Legacy Buy-Burn Security Review

A security review of the Legacy Buy-Burn smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

## About Legacy

Legacy is a decentralized smart contract protocol on Ethereum designed to be a digital store of value with Bitcoin-like tokenomics, offering a finite supply, self-minting rewards mechanism, and the potential to outperform Bitcoin, all while providing an inflation-resistant asset that empowers users to preserve and grow their wealth in a secure, decentralized, and energy-efficient manner.

## Disclaimer

"Audits are a time, resource and expertise bound effort where trained experts evaluate smart
contracts using a combination of automated and manual techniques to find as many vulnerabilities
as possible. Audits can show the presence of vulnerabilities **but not their absence**."

\- Secureum

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

## Executive summary

### Overview

|               |                                                                                              |
| :------------ | :------------------------------------------------------------------------------------------- |
| Project Name  | Legacy Buy-Burn                                                                                     |
| Repository    | https://github.com/devdomsos/legacy-smart-contracts-audit/  
| Methods       | Static + Manual review                                                                                |
|               |


### Issues found

| Severity      |                                                     Count |
| :------------ | --------------------------------------------------------: |
| High risk     |       0 |
| Medium risk   |     1 |
| Low risk      |       0 |
| Informational      |       3 |



### Scope

| File                                                                                                    | 
| :------------------------------------------------------------------------------------------------------ | 
| _Contracts (1)_                                                  |
| /BuyAndBurn.sol |

# Findings


## Medium severity

### [M-01] `Require` condition that ensures call is from EOA might not hold true in the future

**Description**:  

In the `buynBurn` function the code is using the check `require(msg.sender == tx.origin)` to prevent contract accounts from calling the function. While this approach ensures that only Externally Owned Accounts (EOAs) can call the function, this check is not future-proof due to **EIP-3074** (a proposed Ethereum Improvement Proposal).

This [EIP](https://eips.ethereum.org/EIPS/eip-3074#abstract) introduces two EVM instructions **AUTH** and **AUTHCALL**. The first sets a context variable authorized based on an ECDSA signature. The second sends a call as the authorized account. **This essentially delegates control of the externally owned account (EOA) to a smart contract.**

**Recommendation**:

You can use `isContract()` modifier by openzeppelin but there is also a catch with that: If the function is called in the constructor. `isContract()` checks for the code length, but during construction code length is 0. So this check can be bypassed.

Moreover If you want to restrict a Bot calling this fuction multiple time we can place a `require` condition that can make sure there is a gap of some time after every call (this check is already implemented)

**Resolution**

Acknowledged. Buy&Burn contract implements interval which should prevent the function from being abused. However, it is not future-proof solution for bot prevention. Users should be aware of this topic.

## Informational

### [I-01] safeApprove() is deprecated

#### Summary
According to Openzeppelin `safeApprove()` is [deprecated](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)

```solidity
/**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
```
**Resolution**

Acknowledged. The safeApprove is used once in the function createInitialLiquidity and in this case does not pose a race condition threat.

### [I-02] Consider moving msg.sender checks to modifiers

#### Summary
Some functions are only allowed to be called by some specific users (s_ownerAddress), consider using a modifier instead of checking with a require statement, especially if this check is done in multiple functions.  

**Resolution**

Acknowledged. No further action will be taken for this contract



### [I-03] Use the latest solidity version for deployment

### Summary 
Upgrading to a newer Solidity release can optimize gas usage and solve bugs that previous version can have take advantage of new features and improve overall contract efficiency. Where possible, based on compatibility requirements, it is recommended to use newer/latest solidity version to take advantage of the latest optimizations and features. You can see the latest version [here](https://soliditylang.org/blog/category/releases/)

**Resolution**

Acknowledge
