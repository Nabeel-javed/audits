# GMCoin Smart Contracts Security Audit Report


## Disclaimer: This audit represents a point-in-time assessment. Smart contracts require ongoing security monitoring and regular audits. No audit can point out 100% of the vulnerabilities so its best to go for multiple audits to increase your chances.


### Key Findings Summary:
- **2 Critical Vulnerabilities**
- **2 High Risk Issues**  
- **2 Low Risk Issues**
- **1 Informational Issue**

---

## Scope

### Audited Contracts:
- **GMCoin.sol** 
- **Storage.sol** 
- **TwitterOracle.sol**

### Audit Methodology:
- Static code analysis
- Manual security review
- Architecture assessment
- Business logic validation
- Access control verification
- Edge case analysis

---


## CRITICAL SEVERITY VULNERABILITIES (21)

### CRITICAL-001: Double Registration Exploit - Unlimited Token Generation
**Location:** TwitterOracle.sol:137-153  
**Severity:** CRITICAL  
**Business Impact:** COMPLETE ECONOMIC COLLAPSE

**Vulnerable Code:**
```solidity
function verifyTwitter(string calldata userID, address wallet) public onlyGelato {
    mintingData.usersByWallets[wallet] = userID;          // Always executed
    mintingData.registeredWallets[wallet] = true;        // Always executed
    
    if (mintingData.walletsByUserIDs[userID] == address(0)) {  // Check only userID mapping
        mintingData.walletsByUserIDs[userID] = wallet;
        mintingData.allTwitterUsers.push(userID);
        mintingData.userIndexByUserID[userID] = mintingData.allTwitterUsers.length - 1;
        
        _mintForUserByIndex(
            mintingData.allTwitterUsers.length - 1,
            mintingConfig.COINS_MULTIPLICATOR * mintingConfig.POINTS_PER_TWEET
        ); // ALWAYS MINTS WELCOME COINS when userID mapping is empty!
        
        emit TwitterVerificationResult(userID, wallet, true, '');
    }
}
```

**The Fatal Flaw:**
The function checks if `walletsByUserIDs[userID] == address(0)` but the `removeMe()` function deletes this mapping while keeping the user in `allTwitterUsers`. This allows infinite re-registration with the same userID.

**Attack Scenario:**
```solidity
// Infinite money glitch:
// 1. User gets verified -> receives coins for tweet
// 2. User calls removeMe() -> walletsByUserIDs[userID] becomes address(0)  
// 3. User gets verified again -> condition passes, mints welcome coins AGAIN
// 4. Repeat infinitely -> unlimited tokens!

// Example exploitation:
// - Coins = 1000 * COINS_MULTIPLICATOR  
// - After 100 iterations = 100,000 * COINS_MULTIPLICATOR tokens
// - Cost: Just gas fees for removeMe() calls
// - Time: Minutes to become majority token holder
```


**Remediation:**
```solidity
function verifyTwitter(string calldata userID, address wallet) public onlyGelato {
    // Check if wallet was EVER registered before
    require(!mintingData.hasEverBeenRegistered[wallet], "Wallet already received welcome coins");
    
    mintingData.usersByWallets[wallet] = userID;
    mintingData.registeredWallets[wallet] = true;
    
    if (mintingData.walletsByUserIDs[userID] == address(0)) {
        mintingData.walletsByUserIDs[userID] = wallet;
        mintingData.allTwitterUsers.push(userID);
        mintingData.userIndexByUserID[userID] = mintingData.allTwitterUsers.length - 1;
        
        // Only mint if wallet never received welcome coins before
        mintingData.hasEverBeenRegistered[wallet] = true;
        _mintForUserByIndex(
            mintingData.allTwitterUsers.length - 1,
            mintingConfig.COINS_MULTIPLICATOR * mintingConfig.POINTS_PER_TWEET
        );
        
        emit TwitterVerificationResult(userID, wallet, true, '');
    }
}
```


---

### CRITICAL-002:  Missing User Index Cleanup in `removeMe()`
**Location:** TwitterOracle.sol:369  
**Severity:** Critical  

**Vulnerable Code:**
```solidity
function removeMe() public {
    // ... validation and cleanup logic
    string memory userID = mintingData.usersByWallets[wallet];
    uint userIndex = mintingData.userIndexByUserID[userID];
    
    delete mintingData.registeredWallets[wallet];
    delete mintingData.walletsByUserIDs[userID];
    delete mintingData.usersByWallets[wallet];
    
    // Array manipulation
    string memory lastIndexUserID = mintingData.allTwitterUsers[mintingData.allTwitterUsers.length - 1];
    mintingData.allTwitterUsers[userIndex] = lastIndexUserID;
    mintingData.allTwitterUsers.pop();
    
    mintingData.userIndexByUserID[lastIndexUserID] = userIndex;
    // BUG: mintingData.userIndexByUserID[userID] is never deleted!
}
```

**The Flaw:**
The `userIndexByUserID[userID]` mapping is never cleaned up, leaving stale references that point to wrong array positions after user removal.


**Remediation:**
```solidity
function removeMe() public {
    // ... existing logic
    delete mintingData.userIndexByUserID[userID]; // Add this cleanup
}
```

---

## 🟡 MEDIUM SEVERITY VULNERABILITIES (1)


### Medium-001: Off-by-One Error in Index Validation
**Location:** TwitterOracle.sol:263  
**Severity:** Medium  

**Vulnerable Code:**
```solidity
if (userData[i].userIndex > mintingData.allTwitterUsers.length) {
    revert('wrong userIndex');
}
```

**Description:**
Should use `>=` instead of `>` since array indexes are zero-based. Valid indexes are `0` to `length-1`, so `length` itself is invalid.


**Proof of Concept:**
```solidity
// If allTwitterUsers.length = 10, valid indexes are 0-9
// Current code allows userIndex = 10, which will cause out-of-bounds access
// in walletByTwitterUserIndex(10) -> allTwitterUsers[10] (doesn't exist)
```

**Remediation:**
```solidity
if (userData[i].userIndex >= mintingData.allTwitterUsers.length) {
    revert('wrong userIndex');
}
```
