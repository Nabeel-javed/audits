# EMPOWER TOKEN Security Review

A security review of the Empower Token smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

## About Legacy

EmpowerCoin’s unique model redistributes a portion of transaction fees to EMPC token holders, creating a steady stream of passive income. This means users earn rewards simply by holding EMPC tokens, making it an attractive investment that continuously generates returns.

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
| Project Name  | Empower Token                                                                                   |
| Website    | https://empowercoin.finance/  
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

| File | 
| :------------------------------------------------------------------------------------------------------ | 
| *Contracts (12)*                                                  |
| /UpgradeableProxies/BorrowLendProxy.sol |
| /UpgradeableProxies/DonationProxy.sol |
| /UpgradeableProxies/EmpowerTokenProxy.sol |
| /UpgradeableProxies/EscrowProxy.sol |
| /UpgradeableProxies/LoanPaymentProxy.sol |
| /UpgradeableProxies/TreasuryProxy.sol |
| /BorrowLendUpgradeable.sol |
| /DonationUpgradable.sol |
| /EmpowerTokenUpgradeable.sol |
| /EscrowUpgradeable.sol |
| /LoanPaymentUpgradeable.sol |
| /TreasuryUpgradeable.sol |

# Findings

## High Severity

### Issue: Oracle data feed can be outdated yet used anyways

### Description
Chainlink classifies their data feeds into four different groups regarding how reliable is each source thus, how risky they are. The groups are *Verified Feeds, Monitored Feeds, Custom Feeds and Specialized Feeds* (they can be seen [here](https://docs.chain.link/docs/selecting-data-feeds/#data-feed-categories)). The risk is the lowest on the first one and highest on the last one.

A strong reliance on the price feeds has to be also monitored as recommended on the [Risk Mitigation section](https://docs.chain.link/docs/selecting-data-feeds/#risk-mitigation). There are several reasons why a data feed may fail such as unforeseen market events, volatile market conditions, degraded performance of infrastructure, chains, or networks, upstream data providers outage, malicious activities from third parties among others.

Chainlink recommends using their data feeds along with some controls to prevent mismatches with the retrieved data. Along some recommendations, the feed can include circuit breakers (for extreme price events), contract update delays (to ensure that the injected data into the protocol is fresh enough), manual kill-switches (to cease connection in case of found bug or vulnerability in an upstream contract), monitoring (control the deviation of the data) and soak testing (of the price feeds).

```solidity
function convertEthToUsd() public view returns (uint256) {
        (
            ,
            /* uint80 roundID */
            int256 answer,
            ,
            ,
        ) = /*uint startedAt*/
        /*uint timeStamp*/
        /*uint80 answeredInRound*/
         s_ethUsdPriceFeed.latestRoundData(); //@audit

        uint256 additionalPrecision = s_ethUsdPriceFeed.decimals() - ERC20Upgradeable(i_usdcToken).decimals();
        return uint256(answer) / (10 ** additionalPrecision);
    }
```

Regarding Empower Token itself, only the `answer` is used. The retrieved price of the `priceFeed` can be outdated and used anyways as a valid data because no timestamp tolerance of the update source time is checked while storing the return parameters of `feed.latestRoundData()` as recommended by Chainlink. The usage of outdated data can impact on how the Payment terminals work regarding pricing calculation and value measurement.


### **Recommendation**
It is recommended to add a tolerance that compares the `updatedAt` return timestamp from `latestRoundData()` with the current block timestamp and ensure that the `priceFeed` is being updated with the required frequency.

 `ETH/USD` is the only one that is needed to retrieve, because it is the most popular and available pair. It can also be useful to add other oracle to get the price feed (such as Uniswap's). This can be used as a redundancy in the case of having one oracle that returns outdated values (what is outdated and what is up to date can be determined by a tolerance as mentioned).



## Medium severity

### Issue: No Withdraw Function for Ether in the Contract

#### **Summary**
The contract `Rounter.sol` includes a `receive()` function to accept Ether (ETH) transfers, but it does not have a corresponding withdraw function to allow users to retrieve or withdraw their funds. 

#### **Issue Description**
In Solidity, a `receive()` function allows a contract to accept Ether payments. The function is automatically triggered when Ether is sent to the contract. However, in this contract, there is no mechanism to allow users to withdraw their funds after sending them.

The absence of a withdraw function can be problematic in the following ways:
1. **Accidental ETH Transfers**: If a user mistakenly sends ETH to the contract, they will not be able to recover it because there is no withdraw function implemented.
2. **No Recovery Mechanism**: Without a dedicated function to withdraw Ether, the contract cannot offer a safe way to reverse transactions in case of mistakes or unintended transfers.

#### **Recommendation**
It is recommended to implement a **withdraw function** that allows the contract owner or users to withdraw Ether from the contract. 

#### **Resolution:**

### Issue: ETH transfer uses deprecated `.transfer()` instead of `.call()`

### Description
The contract uses `.transfer()` to send ETH. This method is deprecated and has a hard-coded gas limit of 2300 gas units, which could potentially cause transactions to fail if the receiving address is a contract with a more complex receive function.

### Proof of Concept (PoC)
```solidity
// Current implementation
function retrieve() public onlyOwner {
    if (ethBal > 0) {
        payable(msg.sender).transfer(ethBal); //@audit - using deprecated transfer
    }
}
```

### Technical Details
1. `.transfer()` forwards exactly 2300 gas
2. `.call()` forwards all available gas by default
3. `.call()` returns a boolean indicating success/failure
4. Important to check the return value of `.call()`

### Recommended Mitigation
Use `.call()` with value instead of `.transfer()` and implement checks-effects-interactions pattern:

```solidity
function retrieve() public onlyOwner {

    if (ethBal > 0) {
        // Using call instead of transfer
        (bool success, ) = payable(msg.sender).call{value: ethBal}("");
        require(success, "ETH transfer failed");
    }
}
```


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

Acknowledged. No further action will be taken
