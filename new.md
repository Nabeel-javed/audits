# WyvernBuyAndBurn Liquidity Creation Audit Report

## Scope

This audit focuses on the liquidity creation mechanism in the WyvernBuyAndBurn contract, specifically:

1. `createUniswapLiquidity` function in WyvernBuyAndBurn contract
2. `mintInitialLiquidity` function in WyvernX contract
3. Related internal functions: `_createTitanWyvernPool` and `_mintLP`

## Contract Interactions

The liquidity creation process involves interaction between two contracts:

1. **WyvernBuyAndBurn**: Controls the liquidity creation process
2. **WyvernX**: Mints WyvernX tokens for liquidity provision

## Function Analysis

### WyvernBuyAndBurn.createUniswapLiquidity

```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner {
    address wyvernAddress_ = wyvernAddress;

    if (wyvernAddress_ == address(0)) revert InvalidWyvernAddress();

    WyvernX wyvernX = WyvernX(payable(wyvernAddress_));
    IERC20 titanX = IERC20(TITANX_ADDRESS);

    // Get current TitanX balance in the contract
    uint256 titanContractBalanceForInitialLiquidity = titanX.balanceOf(address(this));
    require(titanContractBalanceForInitialLiquidity > 0, "Provide TitanX Liquidity");

    wyvernX.mintInitialLiquidity(initialWyvernXLiquidityAmount);

    titanX.safeIncreaseAllowance(
        UNI_NONFUNGIBLEPOSITIONMANAGER,
        titanContractBalanceForInitialLiquidity
    );

    wyvernX.safeIncreaseAllowance(
        UNI_NONFUNGIBLEPOSITIONMANAGER,
        initialWyvernXLiquidityAmount
    );

    _createTitanWyvernPool(titanContractBalanceForInitialLiquidity, initialWyvernXLiquidityAmount);
    _mintLP(titanContractBalanceForInitialLiquidity, initialWyvernXLiquidityAmount);
}
```

### WyvernX.mintInitialLiquidity

```solidity
function mintInitialLiquidity(uint256 amount) external {
    address buyAndBurnAddress_ = s_buyAndBurnAddress;

    require(msg.sender == buyAndBurnAddress_, "must be B&B contract");

    require(
        s_manaPoolMinted == WyvernManaPoolMinted.NO,
        "mana already minted"
    );

    _mint(buyAndBurnAddress_, amount);
    s_manaPoolMinted = WyvernManaPoolMinted.YES;
}
```

## Workflow Understanding

1. The owner calls `createUniswapLiquidity` on the WyvernBuyAndBurn contract
2. The function gets the current TitanX balance in the contract
3. It calls `mintInitialLiquidity` on the WyvernX contract to mint WyvernX tokens
4. It approves token spending for the Uniswap position manager
5. It creates a Uniswap V3 pool if it doesn't exist
6. It creates a full-range liquidity position with both tokens

## Findings

### High Severity Issues

#### 1. No One-Time Execution Check in WyvernBuyAndBurn

**Issue**: While the WyvernX contract prevents multiple mintings with `s_manaPoolMinted`, the WyvernBuyAndBurn contract doesn't have a similar check to prevent calling `createUniswapLiquidity` multiple times.

**Code Location**:
```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner {
    // No check if liquidity has already been initialized
```

**Recommendation**: Add a state variable and check to prevent multiple executions:

```solidity
bool private liquidityInitialized;

function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner {
    require(!liquidityInitialized, "Liquidity already initialized");
    // Existing code...
    liquidityInitialized = true;
}
```

### Medium Severity Issues

#### 1. Insufficient TitanX Balance Validation

**Issue**: The function only checks that TitanX balance is greater than 0, but doesn't validate if it's sufficient for balanced liquidity.

**Code Location**:
```solidity
uint256 titanContractBalanceForInitialLiquidity = titanX.balanceOf(address(this));
require(titanContractBalanceForInitialLiquidity > 0, "Provide TitanX Liquidity");
```

**Recommendation**: Add a proportionality check:

```solidity
uint256 titanContractBalanceForInitialLiquidity = titanX.balanceOf(address(this));
require(titanContractBalanceForInitialLiquidity > 0, "Provide TitanX Liquidity");
// Ensure reasonable ratio between tokens (possibly using oracle price data)
require(titanContractBalanceForInitialLiquidity >= initialWyvernXLiquidityAmount / REASONABLE_RATIO, 
        "Insufficient TitanX for balanced liquidity");
```

#### 2. No Validation for WyvernX Amount

**Issue**: There's no validation on the `initialWyvernXLiquidityAmount` parameter in either function, potentially allowing excessive minting.

**Code Location**:
```solidity
// In WyvernBuyAndBurn.createUniswapLiquidity
function createUniswapLiquidity(uint256 initialWyvernXLiquidityAmount) external onlyOwner {
    // No validation of initialWyvernXLiquidityAmount

// In WyvernX.mintInitialLiquidity
function mintInitialLiquidity(uint256 amount) external {
    // No validation of amount
```

**Recommendation**: Add range validation in both functions:

```solidity
// In WyvernBuyAndBurn
require(initialWyvernXLiquidityAmount > 0 && 
        initialWyvernXLiquidityAmount <= MAX_INITIAL_WYVERN_LIQUIDITY,
        "Invalid WyvernX liquidity amount");

// In WyvernX
require(amount > 0 && amount <= MAX_MINT_AMOUNT, "Invalid mint amount");
```

#### 3. Missing Event Emission

**Issue**: No event is emitted for this critical one-time initialization.

**Code Location**: End of `createUniswapLiquidity` function

**Recommendation**: Add and emit an event:

```solidity
event LiquidityInitialized(
    uint256 indexed titanAmount, 
    uint256 indexed wyvernAmount, 
    uint256 tokenId, 
    uint256 liquidity
);

// At the end of createUniswapLiquidity
emit LiquidityInitialized(
    titanContractBalanceForInitialLiquidity,
    initialWyvernXLiquidityAmount,
    tokenLiquidityInfo.tokenId,
    tokenLiquidityInfo.liquidity
);
```

### Low Severity Issues

#### 1. No Reentrancy Protection

**Issue**: `createUniswapLiquidity` lacks a nonReentrant modifier, unlike other external functions in the contract.

**Code Location**:
```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner {
    // No nonReentrant modifier
```

**Recommendation**: Add the nonReentrant modifier for consistency:

```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner nonReentrant {
```

#### 2. No Pool Existence Check

**Issue**: The function doesn't check if the pool already exists before attempting to create it.

**Code Location**:
```solidity
_createTitanWyvernPool(titanContractBalanceForInitialLiquidity, initialWyvernXLiquidityAmount);
```

**Recommendation**: Add a check before pool creation:

```solidity
// Before _createTitanWyvernPool call
address computed = PoolAddress.computeAddress(
    UNI_FACTORY,
    PoolAddress.getPoolKey(
        TITANX_ADDRESS < wyvernAddress ? TITANX_ADDRESS : wyvernAddress,
        TITANX_ADDRESS < wyvernAddress ? wyvernAddress : TITANX_ADDRESS,
        FEE_TIER
    )
);
require(computed == address(0) || !_isContract(computed), "Pool already exists");
```

#### 3. No Return Values

**Issue**: The function doesn't return any values to indicate success or position details.

**Code Location**:
```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner {
    // No return values
```

**Recommendation**: Return position details:

```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount
) external onlyOwner returns (uint256 tokenId, uint256 liquidity) {
    // Existing code...
    return (tokenLiquidityInfo.tokenId, tokenLiquidityInfo.liquidity);
}
```

### Informational Issues

#### 1. Price Calculation Mechanism

**Issue**: Initial price is calculated based on token amounts rather than market prices.

**Code Location**:
```solidity
uint160 sqrtPX96 = uint160((sqrt((amount1 * 1e18) / amount0) * 2 ** 96) / 1e9);
```

**Recommendation**: Consider providing a mechanism to set initial price based on external data:

```solidity
function createUniswapLiquidity(
    uint256 initialWyvernXLiquidityAmount,
    uint160 initialSqrtPrice // Optional parameter
) external onlyOwner {
    // Use provided initialSqrtPrice or calculate based on amounts
}
```

#### 2. Documentation for Two-Step Process

**Issue**: The need to transfer TitanX to the contract before calling `createUniswapLiquidity` isn't explicitly documented.

**Recommendation**: Add explicit documentation:

```solidity
/**
 * @notice Initializes liquidity in the pool
 * @dev REQUIRES: TitanX tokens must be transferred to this contract before calling
 * @param initialWyvernXLiquidityAmount WyvernX liquidity amount to determine price ratio
 */
```

## Cross-Contract Analysis

1. **Synchronized One-Time Execution**: The WyvernX contract prevents multiple calls to `mintInitialLiquidity` through the `s_manaPoolMinted` flag. However, if this flag is somehow bypassed or the WyvernX contract is redeployed, the WyvernBuyAndBurn contract has no protection against re-minting.

2. **Centralized Control**: Both functions rely on access control (onlyOwner in WyvernBuyAndBurn and sender check in WyvernX), which means the process is highly centralized.

3. **Trust Assumptions**: The WyvernX contract trusts the WyvernBuyAndBurn contract to request a reasonable amount of tokens. There's no limit on how many tokens can be minted.

## Benefits of the Current Implementation

The primary change (using pre-transferred TitanX tokens) has several benefits:

1. **Clear Separation of Concerns**: Clearly separates funding (transferring TitanX to the contract) from liquidity creation.
2. **Reduced Transaction Complexity**: No need to handle token transfers during liquidity creation.
3. **Improved Test Isolation**: Easier to test the liquidity creation process independently.

## Conclusion

The implementation correctly handles the use of pre-transferred TitanX tokens for liquidity provision. The WyvernX token minting and subsequent liquidity provisioning appear to work as intended, though several improvements could be made to enhance security, validation, and usability.

The most significant recommendation is to add a one-time execution check in the WyvernBuyAndBurn contract to match the protection already present in the WyvernX contract. Additionally, adding validation for the token amounts and event emissions would improve the contract's robustness and transparency.
