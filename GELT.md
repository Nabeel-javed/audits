# GELT Security Review

A security review of the Gelt smart contract protocol was done by [0xepley](https://twitter.com/0xepley). \
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
| Project Name  | GELT                                                                                  |
| Repository    | https://github.com/MDulquerS/TreasuryLari-StableCoin/blob/main/src/TreasuryLari.sol
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


## High Severity

### **Issue: Missing `burn()` and `burnFrom()` Functions**  

#### **Overview**  
The `TreasuryLari` contract does **not implement token burning functions**, meaning tokens cannot be permanently removed from circulation. This limits token supply management and may affect long-term economic strategies like deflationary mechanics.  


## **Issue Details**
### ** No `burn()` Function for Users**
- In standard ERC20 implementations, users should be able to **destroy their own tokens** to reduce supply.
- The absence of a `burn()` function means users **cannot voluntarily remove tokens** from their balance.

### ** No `burnFrom()` for Approved Spending**
- ERC20 tokens typically include a `burnFrom()` function that allows an **approved spender** to burn tokens on behalf of an address.
- Since the contract lacks `burnFrom()`, even if a user **approves** another wallet (or a contract) to use their tokens, the approved spender **cannot burn them**.


## Recommended Fix: 
### Add `burn()` for Users to Burn Their Own Tokens
```solidity
function burn(uint256 amount) external {
    require(amount > 0, "Burn amount must be greater than zero");
    _burn(msg.sender, amount);
}
```

---

### ** Add `burnFrom()` for Approved Spenders**
```solidity
function burnFrom(address account, uint256 amount) external {
    uint256 currentAllowance = allowance(account, msg.sender);
    require(currentAllowance >= amount, "Burn amount exceeds allowance");

    _approve(account, msg.sender, currentAllowance - amount);
    _burn(account, amount);
}
```



## Medium severity

## **Issue: Unsafe ERC20 Token Transfers Using `.call()`**

### **Overview**
In Solidity, using `.call()` to transfer ERC20 tokens is **not recommended** because it can fail for **non-standard ERC20 tokens** like USDT, BNB, and MKR. Some ERC20 tokens do not return a boolean (`true`) on `transfer()`, causing `.call()` to fail unexpectedly.  

```solidity
(bool success, bytes memory data) = address(token).call(
    abi.encodeWithSelector(IERC20.transfer.selector, to, tbalance)
);
if (!success || (data.length != 0 && !abi.decode(data, (bool)))) {
    revert TransferFailed();
}
```


### **Why `.call()` is Problematic?**
1. **Non-Standard ERC20 Behavior**  
   - Some tokens do not return a boolean (`true`) on `transfer()`.
   - `.call()` expects a return value and fails if the token does not provide it.

2. **Lack of Compatibility with Certain ERC20 Tokens**  
   - Tokens like **USDT, MKR, and BNB** do not strictly adhere to ERC20’s expected `transfer()` return value.
   - `.call()` will **fail on these tokens**.


### **Recommended Fix:**
```solidity
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

function withdrawStuckTokens(address token, address to) external onlyOwner {
    require(to != address(0), "Invalid recipient");

    uint256 tbalance = IERC20(token).balanceOf(address(this));
    require(tbalance > 0, "No token balance");

    SafeERC20.safeTransfer(IERC20(token), to, tbalance);
}
```


## Low Severity

### Issue 4: Lack of Validation in `updateTLWallet()` and `updateTaxWallet()` Functions**

#### **Overview**  
The `updateTLWallet()` and `updateTaxWallet()` functions allow the owner to change the treasury and tax wallet addresses. However, these functions **do not validate the new addresses**, which introduces potential risks.  


## **Issue Details**  
**Risk of Setting `address(0)` (Zero Address)**
- The contract **does not check** whether the new wallet address is `address(0)`.  
- If the owner mistakenly or maliciously sets the treasury or tax wallet to `0x0000000000000000000000000000000000000000`, all future operations related to these wallets will **fail**.  


## **Proposed Fix**

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


### **Issue 5: Missing `permit()` Function for Off-Chain Approvals (EIP-2612 Support)**  

#### **Overview**  
The contract extends `ERC20Permit`, but **does not explicitly define the `permit()` function**, making it impossible to use **off-chain approvals** that reduce gas costs for users.  

---

## **Issue Details**  
- The contract inherits `ERC20Permit`, but it **does not expose the `permit()` function**.  
- This means **wallets and dApps cannot interact with it properly**, making the permit functionality **unusable**.



## **Proposed Fix**

```solidity
function permit(
    address owner,
    address spender,
    uint256 value,
    uint256 deadline,
    uint8 v,
    bytes32 r,
    bytes32 s
) external {
    _permit(owner, spender, value, deadline, v, r, s);
}
```

