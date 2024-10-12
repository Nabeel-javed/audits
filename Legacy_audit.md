# Legacy Security Review

A security review of the [Legacy](https://app.legacy.diamonds/) smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

# About Legacy

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
| Project Name  | Legacy                                                                                       |
| Repository    | https://github.com/devdomsos/legacy-smart-contracts-audit                                              |
| Commit hash   | 16b761544a46624630642df993419d8ee604c12d |
| Methods       | Manual review                                                                                |
|               |


### Issues found

| Severity      |                                                     Count |
| :------------ | --------------------------------------------------------: |
| High risk     |       0 |
| Medium risk   |     1 |
| Low risk      |       N/A |
| Informational | N/A |

### Scope

| File                                                                                                    | 
| :------------------------------------------------------------------------------------------------------ | 
| _Contracts (8)_                                                  |
| /Legacy.sol |
| /BurnInfo.sol |
| /GlobalInfo.sol |
| /MintInfo.sol |
| /StakeInfo.sol |
| /BuyAndBurn.sol |
| /OwnerInfo.sol |
| /calcFunctions.sol |
| /constant.sol |
| /enum.sol |

# Findings

## Medium severity

### [M-01] Centralized Risk of Trusted Owner

#### **Description**
The contract owner is entrusted with sensitive operations such as changing important contract addresses and transferring ownership. This increases the risk of misuse, either due to malicious intent or compromise of the owner's account.

```solidity

  function setBuyAndBurnContractAddress(address contractAddress) external onlyOwner {

  function setTitanXBuyAndBurnContractAddress(address contractAddress) external onlyOwner {

  function renounceOwnership() public onlyOwner {

  function transferOwnership(address newOwner) public onlyOwner {
```

MoreOver, The burnLPTokens() function grants the owner the ability to burn liquidity pool (LP) tokens held by the contract but it also opens a door for the owner where the owner can remove liquidity from Uniswap and steal the underlying assets (such as WETH and Legacy tokens).

```solidity
function burnLPTokens() external dailyDifficultyClock {
    _burn(s_buyAndBurnAddress, balanceOf(s_buyAndBurnAddress));
}
```


#### **Recommended Mitigation Steps**
It is recommended to use Multi-Sig wallet

#### **Resolution**

Users should be aware that the contract will be managed by an Externally Owned Account (EOA)



