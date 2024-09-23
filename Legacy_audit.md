# Legacy Security Review

A security review of the [Legacy](https://app.legacy.diamonds/) smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

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
| High risk     |       1 |
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

## High Severity

### [H-01] 

### **Incorrect Scaling in `_calculateClaimReward` When Using `calculateMintPowerBonus`**

**Issue Description:**

In the `_calculateClaimReward` function, the `calculateMintPowerBonus` function is called to compute the user's mint power bonus. However, there is a scaling issue related to how the bonus is processed, potentially leading to an incorrect final reward.

The `calculateMintPowerBonus` function scales the result by `SCALING_FACTOR_1e18` for precision. However, when the bonus is used in `_calculateClaimReward`, it is divided only by `SCALING_FACTOR_1e7`, which accounts for the `mintPowerBonus` scaling, as stated in Natspec `//mintPowerBonus has scaling factor of 1e7, so divide by 1e7`

this **does not account for the additional scaling introduced by `SCALING_FACTOR_1e18`** in the bonus calculation.


### **Impact:**

The `bonus` in `_calculateClaimReward` is divided only by `1e7`, but the scaling from `1e18` applied in `calculateMintPowerBonus` is not factored in. This causes the bonus to be over-scaled by a factor of `1e11` (`1e18 / 1e7`).

### **POC**
A working POC has been added [here](https://gist.github.com/Nabeel-javaid/bbdc4ed214ba2fcbefd69a572d4eaf1a)

Here is the result of POC.

<img width="690" alt="image" src="https://github.com/user-attachments/assets/1ba7216a-8f04-4a3e-8fde-037664ce5611">



### **Recommedation:**
  
  Modify the reward calculation in `_calculateClaimReward` to include division by both `SCALING_FACTOR_1e18` and `SCALING_FACTOR_1e7`:
  
  ```solidity
  reward = uint256(userMintInfo.mintableLegacy) + (bonus / (SCALING_FACTOR_1e18 * SCALING_FACTOR_1e7));
  ```

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

