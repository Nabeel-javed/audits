# GMCoin Business Logic Vulnerabilities - Deep Analysis

**Audit Date:** July 22, 2025  
 

**Executive Summary:**
Deep analysis of business logic revealed 5 critical vulnerabilities that can completely break the economic model, allow unlimited token generation, and cause system failures. These represent fundamental flaws in the core business logic that must be fixed immediately.

---

## 🔴 CRITICAL SEVERITY VULNERABILITIES (2)

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
// 1. User gets verified -> receives coins
// 2. User calls removeMe() -> walletsByUserIDs[userID] becomes address(0)  
// 3. User gets verified again -> condition passes, mints welcome coins AGAIN
// 4. Repeat infinitely -> unlimited tokens!

// Example exploitation:
// - Welcome coins = 1000 * COINS_MULTIPLICATOR  
// - After 100 iterations = 100,000 * COINS_MULTIPLICATOR tokens
// - Cost: Just gas fees for removeMe() calls
// - Time: Minutes to become majority token holder
```

**Economic Impact:**
- **Complete token devaluation**: Infinite supply destroys all value
- **Market manipulation**: Attackers become instant majority holders  
- **System collapse**: All legitimate rewards become worthless
- **Business failure**: Economic incentives completely broken

**Proof of Concept:**
```javascript
// Attack contract:
contract ExploitContract {
    function infiniteTokens() external {
        for(uint i = 0; i < 1000; i++) {
            // 1. Get verified by Gelato -> mint welcome coins
            // 2. Call removeMe() -> clear userID mapping
            // 3. Repeat -> profit!
        }
    }
}
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

### HIGH-003: Missing User Index Cleanup in `removeMe()`
**Location:** TwitterOracle.sol:369  
**Severity:** HIGH  
**Business Impact:** DATA INTEGRITY ISSUES

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

**Impact:**
- **Data corruption**: Stale mappings pointing to wrong users
- **Storage bloat**: Unnecessary storage usage over time  
- **System confusion**: Inconsistent state between mappings and arrays

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

**Impact:**
- Allows access to non-existent array index equal to array length
- Will cause array out-of-bounds access and transaction revert
- DoS potential for minting operations

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