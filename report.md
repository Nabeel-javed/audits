# MProfy-Dao-Web3 Security Review

A security review of the MProfy-Dao-Web3 smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
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
| Project Name  | MProfy-Dao-Web3                                                                                       |
| Repository    | https://github.com/DhruvGodambe/mprofy-dao-web3  
| Methods       | Static + Manual review                                                                                |
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
| /Deed.sol |
| /FounderNFT.sol |
| /Governance.sol |
| /PriceConvertor.sol |
| /Treasury.sol |

# Findings

## High severity

### [H-01] Excessive Proposal Fee sent by user is lost

#### Description:
In the `createProposal()` function, the requirement is that users send a minimum fee of `CREATE_PROPOSAL_COST` to create a proposal. However, if a user sends more than the required fee there is no advantage of it and, the excess amount is not refunded. This could lead to user may unintentionally overpay without being refunded.

```solidity
require(
    msg.value >= CREATE_PROPOSAL_COST,
    "Amount too low for creating proposal"
);
```

There is no logic to handle cases where the user sends more than `CREATE_PROPOSAL_COST`. As a result, any excess ETH sent is effectively "lost" to the contract.

#### Recommendation:
Introduce a logic to refund the excess ETH amount back to the user if they send more than the required proposal creation fee.


```solidity
require(
    msg.value >= CREATE_PROPOSAL_COST,
    "Amount too low for creating proposal"
);

// If user sends more than the required fee, refund the excess
uint excessAmount = msg.value - CREATE_PROPOSAL_COST;
if (excessAmount > 0) {
    payable(msg.sender).call{value: excessAmount}(""); // Refund excess

}
```

This way, users will only be charged the exact amount needed to create the proposal, and any extra ETH sent will be safely refunded to their wallet.



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


### [M-02] Chainlink's `latestRoundData` might return stale or incorrect results

#### **Description**
Chainlink's latestRoundData is used here to retrieve price feed data; however, there is insufficient protection against price staleness.

Return arguments other than `int256 answer` are necessary to determine the validity of the returned price, as it is possible for an outdated price to be received. [See here](https://ethereum.stackexchange.com/questions/133242/how-future-resilient-is-a-chainlink-price-feed/133843#133843) for reasons why a price feed might stop updating.

The return value `updatedAt` contains the timestamp at which the received price was last updated, and can be used to ensure that the price is not outdated. See more information about `latestRoundID` in the [Chainlink docs](https://docs.chain.link/data-feeds/api-reference#latestrounddata). Inaccurate price data can lead to functions not working as expected and/or loss of funds.

```solidity
function getPrice(AggregatorV3Interface priceFeed) public view returns (uint256) {
        (, int256 answer,,,) = priceFeed.latestRoundData(); //@audit 
        // Chainlink returns price with 8 decimal places
        return uint256(answer);
    }

```

Chainlink [recommends](https://docs.chain.link/data-feeds/api-reference#latestrounddata) using their data feeds along with some controls to prevent mismatches with the retrieved data
```solidity
function latestRoundData() external view
    returns (
        uint80 roundId,             //  The round ID.
        int256 answer,              //  The price.
        uint256 startedAt,          //  Timestamp of when the round started.
        uint256 updatedAt,          //  Timestamp of when the round was updated.
        uint80 answeredInRound      //  The round ID of the round in which the answer was computed.
    )

```
only the `answer` is used in the `getPrice()` implementation. The retrieved price of the priceFeed can be outdated and used anyways as a valid data because no timestamp tolerance of the update source time is checked while using the return parameters of feed.latestRoundData().


#### **Recommended Mitigation Steps**
It is recommended both to add also a tolerance that compares the updatedAt return timestamp from latestRoundData() with the current block timestamp and ensure that the priceFeed is being updated with the required frequency.




## Low severity

### [M-01] Missing Events for Critical Functions

#### Description:
The contract lacks event emissions for critical functions, such as minting tokens and burning tokens during transfers. Events are crucial for providing transparency and facilitating off-chain monitoring of contract activities. Without events, it becomes challenging to track important state changes, like minting and burning, which could lead to a lack of accountability and hinder debugging efforts.
  
#### Recommendation:
Emit events in critical functions to log important actions:

