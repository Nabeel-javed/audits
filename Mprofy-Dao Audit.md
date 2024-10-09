# MProfy-Dao-Web3 Security Review

A security review of the MProfy-Dao-Web3 smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.


## Disclaimer

"Audits are a time, resource and expertise bound effort where trained experts evaluate smart
contracts using a combination of automated and manual techniques to find as many vulnerabilities
as possible. Audits can show the presence of vulnerabilities **but not their absence**."

\- Secureum

## About Mprofy
Mprofy DAO is a decentralized platform designed to empower the existing Mprofy community, facilitating their transition from Web2 to Web3. The DAO enables users to actively participate in the governance of the platform by voting on and submitting proposals. If a proposal is approved, it can receive funding from the Mprofy treasury, which holds a variety of crypto assets such as USDT, DAI, WETH, WBTC, and more. These assets are commissions earned by Mprofy users and contributed to the treasury. Additionally, the treasury includes the Deed token, Mprofy's native platform token, further driving the ecosystem's growth and sustainability.



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
| High risk     |       4 |
| Medium risk   |     5 |
| Low risk      |       3 |
| Gas Saving | 4 |

### Issues Summary

| Severity      | Title                                                                                                                                   |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------------- |
| High Risk     | [H-01] Excessive Proposal Fee sent by user is lost                                                                                   |
| High Risk     | [H-02] Unrestricted Access to Change Swap Router Address                                                                              |
| High Risk     | [H-03] Treasury can not received the Locked NFT                                                                                       |
| High Risk     | [H-04] Incorrect Transfer of Non-DAI Assets                                                                                      |
| Medium Risk   | [M-01] Excessive Centralization Risk by Owner                                                                                         |
| Medium Risk   | [M-02] Chainlink's `latestRoundData` might return stale or incorrect results                                                          |
| Medium Risk   | [M-03] Hardcoding slippage to `0` can cause loss of funds                                                                             |
| Medium Risk   | [M-04] Incorrect Placement of Total Deed Tokens Calculation for Each User                                                              |
| Medium Risk   | [M-05] Transfer of ERC20 tokens will fail                                                             |
| Low Risk      | [L-01] Missing Events for Critical Functions                                                                                          |
| Low Risk      | [L-02] Use Ownable2Step instead of Ownable                                                                                             |
| Low Risk      | [L-03] Unspecific Compiler Version Pragma                                                                                              |
| Gas Saving    | [G-01] Nesting if-statements is cheaper than using &&                                                                                  |
| Gas Saving    | [G-02] Cache array length outside of loop                                                                                              |
| Gas Saving    | [G-03] Use Custom Errors                                                                                                               |
| Gas Saving    | [G-04] Using `private` rather than `public` for constants, saves gas                                                                   |


### Scope

| File                                                                                                    | 
| :------------------------------------------------------------------------------------------------------ | 
| _Contracts (5)_                                                  |
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

#### Resolution:
Solved 

### [H-02] Unrestricted Access to Change Swap Router Address


#### Description:
The `setSwapRouter` function allows **anyone** to change the swap router address (`SWAP_ROUTER_02`) without any restrictions. This is a critical issue, as a malicious actor could change the router to their own contract, enabling them to steal all user's funds.


```solidity
function setSwapRouter(address _router) public { 
    SWAP_ROUTER_02 = _router; // Unrestricted access to modify critical contract address
}
```

#### Recommendations:

It is recommended to restrict this function explicitly to the owner that only authorized person can change the router address.

```solidity
   modifier onlyOwner() {
       require(msg.sender == owner, "Caller is not the owner");
       _;
   }

   function setSwapRouter(address _router) public onlyOwner {
       SWAP_ROUTER_02 = _router;
   }
 ```

#### Resolution:
Solved


### [H-03] Treasury can not received the Locked NFT

#### Description:
In the `lockDeedNFT()` function, The NFT is being sent to `address(this)` which is treasury contract. The Transfer is being made using `transferFrom` 

```solidity
  IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); 
```
but since the Treasury Contract do not have `OnERC721Received` function it will not received the NFT and the NFT will be lost in limbo.

According to [openzeppelin docs](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721#ERC721-transferFrom-address-address-uint256-):

`transferFrom()`

`Transfers the ownership of a given token ID to another address. Usage of this method is discouraged, use safeTransferFrom whenever possible. Requires the msg.sender to be the owner, approved, or operator.`

As per the documentation of `EIP-721`:

`A wallet/broker/auction application MUST implement the wallet interface if it will accept safe transfers.`

Ref: https://eips.ethereum.org/EIPS/eip-721



#### Recommendation:
Call the safeTransferFrom() method instead of transferFrom() for NFT transfers. 
```solidity
        IERC721(deedNFT).safeTransferFrom(msg.sender, address(this), tokenId); 
```

#### Resolution:
Solved


### [H-04] Incorrect Transfer of Non-DAI Assets

#### Description:
In the `withdraw()` function, non-DAI assets are swapped for DAI using the `swapExactInputSingle` function. After swapping, the code intends to transfer the resulting DAI (referred to as `usershareInDai`) to the `companyWallet`. 

```solidity
else {
                uint256 usershareInDai = swapExactInputSingle(
                    assets[i],
                    assets[0],
                    userShare,
                    poolFeeTier[i - 1]
                );
                // q should transfer only usershare or userShareInDai
                IERC20(assets[i]).transfer(companyWallet, usershareInDai);
            }
```
However, the transfer code is incorrectly written:

```solidity
IERC20(assets[i]).transfer(companyWallet, usershareInDai);
```

The above line is transferring tokens from the `assets[i]` contract (i.e., the non-DAI token), but after the swap, the variable `usershareInDai` represents an amount in **DAI** (not the original asset). This means that after the asset has been swapped into DAI, the transfer should be using the **DAI token contract** (`assets[0]`), not `assets[i]`.

Assume `assets[i]` is USDC, and after swapping USDC for DAI, the value is stored in `usershareInDai`. This amount is in DAI, but the original code tries to transfer it from the USDC contract (`assets[i]`), which does not have DAI tokens. Instead, the transfer should be done through the DAI contract (`assets[0]`).


If the function continues using `assets[i]`, it will be trying to transfer DAI amounts (`usershareInDai`) from the non-DAI token contract, which is incorrect and will likely fail because `assets[i]` does not hold DAI.

#### Recommendation:
The correct contract for transferring DAI is `assets[0]`. Thus, the transfer line should be updated as follows:

```solidity
IERC20(assets[0]).transfer(companyWallet, usershareInDai);
```


#### Resolution:
Solved

## Medium severity

### [M-01] Excessive Centralization Risk by Owner

#### Description:
The function `drainTreasury()` allows the contract owner to drain all ERC20 tokens and NFTs from the treasury to a company wallet. This introduces significant centralization risks as the owner has unrestricted control over the assets. If the private key of the owner or admin is compromised, or if there is any malicious intent, the entire treasury can be drained. This creates a single point of failure and is devastating for the platform's ecosystem.

```solidity
    function drainTreasury() external onlyOwner {
        // Drain all ERC20 tokens to the admin wallet
        for (uint256 i = 0; i < assets.length; i++) {
            IERC20 token = IERC20(assets[i]);
            uint256 balance = token.balanceOf(address(this));
            if (balance > 0) {
                token.transfer(companyWallet, balance); // Risky centralization
            }
        }
        // Transfer all NFTs
        _transferAllNFTs(deedNFT);
        _transferAllNFTs(founderNFT);
        _transferAllNFTs(genesisNFT);
    }
```


#### **Recommendation**
It is recommended to use Multi-Sig wallet

#### Resolution:
Acknowledged

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


#### **Recommendation**
It is recommended both to add also a tolerance that compares the updatedAt return timestamp from latestRoundData() with the current block timestamp and ensure that the priceFeed is being updated with the required frequency.


#### Resolution:
Solved

### [M-03] Hardcoding slippage to `0` can cause loss of funds

#### Description:

```solidity
IV3SwapRouter.ExactInputSingleParams memory params = IV3SwapRouter
            .ExactInputSingleParams({
                tokenIn: tokenIn,
                tokenOut: tokenOut,
                fee: feeTier,
                recipient: address(this), 
                amountIn: amountIn,
                amountOutMinimum: 0, 
                sqrtPriceLimitX96: 0
            });
```
The `swapExactInputSingle()` function in `treasury.sol` is performing the swap from uniswap pool. In this function `0` value is assigned to `amountOutMinimum` argument which is used for slippage tolerance. 0 value here essentially means 100% slippage tolerance. This is a very easy target for MEV and bots to do a flash loan sandwich attack. 

These occur when MEV algorithms purchase significant amounts of the asset involved in the swap. This action drives the price of the asset up before the swap occurs. These algorithms then sell the asset at this higher price, making a profit. As a result, the user conducting the swap might end up paying more for the asset than they initially expected

### Recommendation

It is recommended to consider slippage when calculating the minimum amount of tokens that should receive.


#### Resolution:
Solved


### [M-04]  Incorrect Placement of Total Deed Tokens Calculation for Each User


**Description**:  
The `amountToMint` represents the total Deed tokens to be minted for the entire deposit. However, in the current implementation, the same `amountToMint` is assigned to each user, regardless of their individual contributions. This leads to an incorrect distribution of Deed tokens, as each user should receive Deed tokens proportional to the amount they deposited.

```solidity
uint256 amountToMint = (totalAmount * DEED_EMISSION_RATE * 10 ** 8) / (10 ** 18);
```

```solidity
for (uint256 i = 0; i < users.length; i++) {
            User memory newUserDeposit = User({
                treasuryDeposit: usdcAmounts[i],
                firstDepositTime: block.timestamp,
                deedMint: amountToMint
            });
```
If Deed tokens are assigned equally to all users, it will result in unfair token distribution. Users who contributed more should receive more tokens, and this misallocation can distort rewards or token ownership.

**Recommendation**:  
Calculate the Deed tokens for each user individually based on their contribution, and assign the correct amount in the loop:

```solidity
uint256 userAmountToMint = (usdcAmounts[i] * DEED_EMISSION_RATE * 10 ** 8) / (10 ** 18); // Proportional calculation
User memory newUserDeposit = User({
    treasuryDeposit: usdcAmounts[i],
    firstDepositTime: block.timestamp,
    deedMint: userAmountToMint // Assign based on individual deposit
});
```

#### Resolution:
Solved

### [M-05] Transfer of ERC20 tokens will fail

**Description**:  
The contract is attempting to transfer `assets[0]` from the msg.sender to the treasury, but there is no approval process in place to ensure that the contract can spend the user's tokens. The function does not request approval from the user before calling:

```solidity
        IDeedToken(deedToken).mintForTreasury(address(this), amountToMint);
        //need approval of user
        bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

        require(success, "Transfer to treasury failed");
```

Since the contract is not taking the approval of assets from the user, the function will most probably fail everytime unless user itself give the persmission to the contract(which is not likely).

`NOTE: In the natspec it is written that "//need approval of user " but the code isn't asking for approval"`

**Recommendation**:  
It is recommended to ask for approval from user to spend their token before trying to transfer it


#### Resolution:
Solved


## Low severity

### [L-01] Missing Events for Critical Functions

#### Description:
The contract lacks event emissions for critical functions, such as minting tokens and burning tokens during transfers. Events are crucial for providing transparency and facilitating off-chain monitoring of contract activities. Without events, it becomes challenging to track important state changes, like minting and burning, which could lead to a lack of accountability and hinder debugging efforts.
  
#### Recommendation:
Emit events in critical functions to log important actions:

#### Resolution:
Solved


###  [L-02] Use Ownable2Step instead of Ownable

#### Description:
The `Ownable2Step` pattern involves a two-step process where the current owner proposes a new owner, and the new owner must accept the proposal to complete the transfer. This mechanism requires active participation from both parties, significantly reducing the risk of unintended or unauthorized ownership changes.

#### Recommendation:
use `Ownable2Step` instead of `Ownable`
```solidity
contract Treasury is Ownable2Step, ReentrancyGuard {
```
#### Resolution
Acknowledge

###  [L-03] Unspecific Compiler Version Pragma

#### Description:
Avoid floating pragmas for non-library contracts.

```solidity
pragma solidity >=0.7.0 <0.9.0;
pragma solidity ^0.8.19;
pragma solidity ^0.8.0;
pragma solidity >=0.8.0 <0.9.0;

```

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

#### Recommendation:
It is recommended to pin to a concrete compiler version.


#### Resolution:
Acknowledged


## Gas Saving

### [G-01] Nesting if-statements is cheaper than using &&

#### Description:
Nesting if-statements avoids the stack operations of setting up and using an extra jumpdest, and saves 6 [gas](https://gist.github.com/IllIllI000/7f3b818abecfadbef93b894481ae7d19)  

There are total 11 instances of this issue

#### Resolution:
Acknowledge


### [G-02] Cache array length outside of loop

#### Description:
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

There are total 23 instances of this issue

#### Resolution:
Solved


### [G-03] Use Custom Errors

#### Description:
[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

There are total 62 instances of this issue

#### Resolution:
Acknowledge


### [G-04] Using `private` rather than `public` for constants, saves gas

#### Description:
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

There are total 10 instances of this issue

#### Resolution:
Acknowledge

