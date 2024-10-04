# Report


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Centralization Risk for trusted owners | 14 |
| [M-2](#M-2) | No check for chainlink stale price | 1 |
| [M-3](#M-3) | No check for minimum Or maximum answer circuit breaker | 1 |
| [M-4](#M-4) | Unsafe use of ERC20 transferFrom() | 8 |
| [M-5](#M-5) | Unsafe use of ERC20 transfer() | 8 |

## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | complex casting | 26 |
| [L-2](#L-2) | Functions calling contracts/addresses with transfer hooks are missing reentrancy guards | 7 |
| [L-3](#L-3) | Tokens may be minted to address(0x0) | 3 |
| [L-4](#L-4) | Do not use deprecated library functions | 4 |
| [L-5](#L-5) | Division by zero not prevented | 13 |
| [L-6](#L-6) | Use Ownable2Step instead of Ownable | 2 |
| [L-7](#L-7) | Owner can renounce Ownership   | 2 |
| [L-8](#L-8) | Loss of precision | 13 |
| [L-9](#L-9) | safeApprove() is deprecated | 4 |
| [L-10](#L-10) | State variables not capped at reasonable values | 72 |
| [L-11](#L-11) | Some tokens may revert when zero value transfers are made | 8 |
| [L-12](#L-12) |  Functions calling contracts/addresses with transfer hooks should be protected by reentrancy guard   | 8 |
| [L-13](#L-13) | Some tokens may revert when large transfers are made | 9 |
| [L-14](#L-14) | Unsafe casting | 12 |
| [L-15](#L-15) | Unsafe ERC20 operation(s) | 16 |
| [L-16](#L-16) | Unspecific compiler version pragma | 3 |

## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Contracts should have full test coverage | 8 |
| [NC-2](#NC-2) | Consider adding formal verification proofs | 8 |
| [NC-3](#NC-3) | Large or complicated code bases should implement invariant tests | 8 |
| [NC-4](#NC-4) | NatSpec: Interface declarations should have NatSpec descriptions | 2 |
| [NC-5](#NC-5) | Variables without visibility specifier | 44 |
| [NC-6](#NC-6) | Array is push()ed but not pop()ed | 12 |
| [NC-7](#NC-7) | Constants in comparisons should appear on the left side | 46 |
| [NC-8](#NC-8) | constants should be defined rather than using magic numbers | 8 |
| [NC-9](#NC-9) | Contract declarations should have NatSpec @author annotations | 9 |
| [NC-10](#NC-10) | Contract declarations should have NatSpec @Title annotations | 9 |
| [NC-11](#NC-11) | NatSpec: Contract declarations should have @dev tags | 9 |
| [NC-12](#NC-12) | NatSpec: Contract declarations should have NatSpec descriptions | 9 |
| [NC-13](#NC-13) | NatSpec: Contract declarations should have @notice tags | 9 |
| [NC-14](#NC-14) | Consider using delete rather than assigning zero to clear value | 38 |
| [NC-15](#NC-15) | Consider using delete rather than assigning false to clear value | 2 |
| [NC-16](#NC-16) | Consider adding a block/deny-list" | 12 |
| [NC-17](#NC-17) | Use bytes.concat() on bytes instead of abi.encodePacked() for clearer semantic meaning | 1 |
| [NC-18](#NC-18) | Consider adding emergency-stop functionality | 9 |
| [NC-19](#NC-19) | Events are missing sender information | 14 |
| [NC-20](#NC-20) | NatSpec: Event declarations should have NatSpec descriptions | 12 |
| [NC-21](#NC-21) | contracts should use fixed compiler versions | 6 |
| [NC-22](#NC-22) | NatSpec: function declarations should have NatSpec descriptions | 101 |
| [NC-23](#NC-23) | NatSpec: function declarations should have @Notice tags | 101 |
| [NC-24](#NC-24) | NatSpec: function declarations should have NatSpec descriptions | 101 |
| [NC-25](#NC-25) | If-statement can be converted to a ternary | 50 |
| [NC-26](#NC-26) | Large multiples of ten should use scientific notation | 1 |
| [NC-27](#NC-27) | Consider using named mappings | 2 |
| [NC-28](#NC-28) | Consider combining multiple address/ID mappings into a single mapping of an address/ID to a struct | 10 |
| [NC-29](#NC-29) | Use of override is unnecessary | 5 |
| [NC-30](#NC-30) | Consider using descriptive constants when using 0 in the code | 24 |
| [NC-31](#NC-31) | Non-external/public variable names should begin with an underscore | 38 |
| [NC-32](#NC-32) | Return values of `approve()` not checked | 3 |
| [NC-33](#NC-33) | Setters should prevent re-setting of the same value | 9 |
| [NC-34](#NC-34) | Use the latest solidity version for deployment   | 6 |
| [NC-35](#NC-35) | Consider bounding input array length | 21 |
| [NC-36](#NC-36) | Constants should be defined rather than using magic numbers | 1 |
| [NC-37](#NC-37) | Strings should use double quotes rather than single quotes | 1 |
| [NC-38](#NC-38) | Variables need not be initialized to zero | 33 |
| [NC-39](#NC-39) | Consider moving msg.sender checks to modifiers | 71 |
| [NC-40](#NC-40) | Dont use _msgSender() if not supporting EIP-2771 | 1 |
| [NC-41](#NC-41) | Array indices should be referenced via enums rather than numeric literals | 16 |
| [NC-42](#NC-42) | <array>.length should not be looked up in every loop of a for-loop | 21 |
| [NC-43](#NC-43) | Use assembly to emit events, in order to save gas | 14 |
| [NC-44](#NC-44) | Don't initialize variables with default value | 34 |
| [NC-45](#NC-45) | Long revert strings | 11 |

## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Enable IR-based code generation | 8 |
| [GAS-2](#GAS-2) | Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18) | 12 |
| [GAS-3](#GAS-3) | Nesting if-statements is cheaper than using && | 11 |
| [GAS-4](#GAS-4) | Consider using = instead of += and -= for gas efficiency | 38 |
| [GAS-5](#GAS-5) | Use >= instead of > for gas efficiency | 23 |
| [GAS-6](#GAS-6) | Using bools for storage incurs overhead | 2 |
| [GAS-7](#GAS-7) | Cache array length outside of loop | 23 |
| [GAS-8](#GAS-8) | Consider using assembly for simple zero checks to save gas | 1 |
| [GAS-9](#GAS-9) | Expressions for constant values should use immutable rather than constant | 14 |
| [GAS-10](#GAS-10) | Constructors can be marked payable | 5 |
| [GAS-11](#GAS-11) | Use Custom Errors | 62 |
| [GAS-12](#GAS-12) | Reduce gas usage by moving to Solidity 0.8.19 or later | 4 |
| [GAS-13](#GAS-13) | Functions guaranteed to revert when called by normal users can be marked `payable` | 22 |
| [GAS-14](#GAS-14) | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 38 |
| [GAS-15](#GAS-15) | Using `private` rather than `public` for constants, saves gas | 10 |
| [GAS-16](#GAS-16) | require()/revert() strings longer than 32 bytes cost extra gas | 30 |
| [GAS-17](#GAS-17) | Use shift Right/Left instead of division/multiplication if possible | 1 |
| [GAS-18](#GAS-18) | Splitting require() statements that use && saves gas | 1 |
| [GAS-19](#GAS-19) | Structs can be packed into fewer storage slots | 4 |
| [GAS-20](#GAS-20) | Consider using uint256(1)/uint256(2) instead of true/false for gas efficiency | 20 |
| [GAS-21](#GAS-21) | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 19 |
| [GAS-22](#GAS-22) | Use != 0 instead of > for unsigned integer comparison | 23 |
| [GAS-23](#GAS-23) | Optimize names to save gas | 115 |

##################################################################### 
 
 DETAILED REPORT:: 


##################################################################### 


## Medium Issues


 ### <a name="M-1"></a>[M-1]
 ### Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (14)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

81:     ) Ownable(msg.sender) {

261:     ) external onlyOwner {

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

453:     ) public onlyOwner {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

629:     function drainTreasury() external onlyOwner {

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

```

```solidity
File: swapp.sol

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

```

</details> 
 


 ### <a name="M-2"></a>[M-2]
 ### No check for chainlink stale price
latestRoundData may return stale data, and there arent any checks to ensure that this doesnt happen. This can be caused by any issues with Chainlink, such as oracle consensus problems or chain congestion, which may result in this contract relying on outdated data.

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: PriceConvertor.sol

7:         (, int256 answer,,,) = priceFeed.latestRoundData(); //@audit issue

```

</details> 
 


 ### <a name="M-3"></a>[M-3]
 ### No check for minimum Or maximum answer circuit breaker
Chainlink aggregators have a built-in circuit breaker if the price of an asset goes outside of a predetermined price band. The result is that if an asset experiences a huge drop in value (i.e. LUNA crash) the price of the oracle will continue to return the minPrice instead of the actual price of the asset. This would allow users to continue borrowing with the asset but at the wrong price. This is exactly what happened to Venus on BSC when LUNA crashed.

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: PriceConvertor.sol

7:         (, int256 answer,,,) = priceFeed.latestRoundData(); //@audit issue

```

</details> 
 


 ### <a name="M-4"></a>[M-4]
 ### Unsafe use of ERC20 transferFrom()
Some tokens do not implement the ERC20 standard properly. For example Tether (USDT)'s transferFrom() function does not return a boolean as the ERC20 specification requires, and instead has no return value. When these sorts of tokens are cast to IERC20/ERC20, their function signatures do not match and therefore the calls made will revert. It is recommended to use the SafeERC20's safeTransferFrom() from OpenZeppelin instead.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

80:         return super.transferFrom(sender, recipient, transferAmount);

```

```solidity
File: Treasury.sol

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

148:         IERC721(founderNFT).transferFrom(msg.sender, address(this), tokenId);

168:         IERC721(genesisNFT).transferFrom(msg.sender, address(this), tokenId);

198:         IERC721(_nftContract).transferFrom(address(this), msg.sender, _tokenId); //@audit safeTransferFrom

272:         bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

651:             nft.transferFrom(address(this), companyWallet, tokenId);

```

</details> 
 


 ### <a name="M-5"></a>[M-5]
 ### Unsafe use of ERC20 transfer()
Some tokens do not implement the ERC20 standard properly. For example Tether (USDT)'s transfer() function does not return a boolean as the ERC20 specification requires, and instead has no return value. When these sorts of tokens are cast to IERC20/ERC20, their function signatures do not match and therefore the calls made will revert. It is recommended to use the SafeERC20's safeTransfer() from OpenZeppelin instead.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

72:         return super.transfer(recipient, transferAmount);

```

```solidity
File: Governance.sol

696:         payable(admin).transfer(address(this).balance);

```

```solidity
File: Treasury.sol

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

365:                 IERC20(assets[i]).transfer(companyWallet, userShare);

374:                 IERC20(assets[i]).transfer(companyWallet, usershareInDai);

635:                 token.transfer(companyWallet, balance);

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

```

</details> 
 


## Low Issues


 ### <a name="L-1"></a>[L-1]
 ### complex casting
Consider whether the number of casts is really necessary, or whether using a different type would be more appropriate. Alternatively, add comments to explain in detail why the casts are necessary, and any implicit reasons why the cast does not introduce an overflow.

*Instances (26)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

19:         _mint(_owner, INITIAL_SUPPLY);

50:         _mint(account, amount);

59:         _mint(account, amount);

```

```solidity
File: FounderNFT.sol

51:     function initiateMint(address _to, uint256 _amount) external {

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

82:             _safeMint(mintRequest.to, i);

91:         _removePendingMint(index);

141:     function _removePendingMint(uint256 index) internal {

214:                 _safeMint(holder, _tokenId);

```

```solidity
File: Governance.sol

450:                 voterVotes[_propID][msg.sender] = int256(votes);

453:                 voterVotes[_propID][msg.sender] = -int256(votes);

468:                 voterVotes[_propID][msg.sender] = int256(votes);

471:                 voterVotes[_propID][msg.sender] = -int256(votes);

490:                 voterVotes[_propID][msg.sender] = int256(votes);

493:                 voterVotes[_propID][msg.sender] = -int256(votes);

509:         int votes = int256(voterVotes[_propID][msg.sender]);

525:                 p.yVotes -= uint256(votes);

527:                 int256 result = int256(p.nVotes) + votes;

528:                 p.nVotes = uint256(result);

541:                 p.yVotes -= uint256(votes);

543:                 int256 result = int256(p.nVotes) + votes;

544:                 p.nVotes = uint256(result);

```

```solidity
File: PriceConvertor.sol

9:         return uint256(answer);

```

```solidity
File: Treasury.sol

10:     function mint(address to, uint256 amount) external;

621:         uint8 decimals = uint8(assetsdecimals[assetIndex]);

761:             uint8 decimal = uint8(assetsdecimals[assetIndex]);

```

</details> 
 


 ### <a name="L-2"></a>[L-2]
 ### Functions calling contracts/addresses with transfer hooks are missing reentrancy guards
Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to [read-only reentrancies](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect against it, except by block-listing the whole protocol.

*Instances (7)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

80:         return super.transferFrom(sender, recipient, transferAmount);

```

```solidity
File: Treasury.sol

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

148:         IERC721(founderNFT).transferFrom(msg.sender, address(this), tokenId);

168:         IERC721(genesisNFT).transferFrom(msg.sender, address(this), tokenId);

198:         IERC721(_nftContract).transferFrom(address(this), msg.sender, _tokenId); //@audit safeTransferFrom

272:         bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

651:             nft.transferFrom(address(this), companyWallet, tokenId);

```

</details> 
 


 ### <a name="L-3"></a>[L-3]
 ### Tokens may be minted to address(0x0)
Neither the  functions, nor _mint() prevent minting to address(0x0)  

*Instances (3)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

19:         _mint(_owner, INITIAL_SUPPLY);

50:         _mint(account, amount);

59:         _mint(account, amount);

```

</details> 
 


 ### <a name="L-4"></a>[L-4]
 ### Do not use deprecated library functions

*Instances (4)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

663:         TransferHelper.safeApprove(_token, SWAP_ROUTER_02, _amount);

668:         TransferHelper.safeApprove(assets[0], SWAP_ROUTER_02, daiAmount);

```

```solidity
File: swapp.sol

50:         TransferHelper.safeApprove(_token, SWAP_ROUTER_02, _amount);

55:         TransferHelper.safeApprove(DAI, SWAP_ROUTER_02, daiAmount);

```

</details> 
 


 ### <a name="L-5"></a>[L-5]
 ### Division by zero not prevented
The divisions below take an input parameter that has no zero-value checks, which can cause the functions reverting if zero is passed.  

*Instances (13)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

84:         return amount * BURN_PERCENTAGE / 100;

```

```solidity
File: Governance.sol

289:             percent = (p.yVotes * PERCENT) / totalVotes;

329:             uint initialReleaseAmount = (p.fundingAmount * 10) / 100;

```

```solidity
File: PriceConvertor.sol

40:             usdAmmountinasset = (1e18 * usdAmount) / assetPrice;

42:             usdAmmountinasset = (1e6 * usdAmount) / assetPrice;

58:         uint256 usdToDaiAmount = (usdAmount * 1e18) / daiUsdPrice;

71:     uint256 ethDaiPrice = 1e18 / daiEthPrice;

```

```solidity
File: Treasury.sol

279:         uint256 daiAmountToSwap = totalAmount / 2;

282:         uint256 DaiAllocatedFunds = (daiAmountToSwap * assetAllocationRatio[0]) / 100; //@audit what happened to DaiAllocatedFunds

346:             uint256 userShare = (treasuryBal * totalAmount) /

388:         uint256 feeAmount = (totalAmountInDai * feePercentage) / 100;

672:             uint256 amountIn = (daiAmount * assetAllocationRatio[i]) / 100;

```

```solidity
File: swapp.sol

59:             uint256 amountIn = (daiAmount * assetRatios[i]) / 100;

```

</details> 
 


 ### <a name="L-6"></a>[L-6]
 ### Use Ownable2Step instead of Ownable
Add a description of why Ownable2Step is recommended.

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

81:     ) Ownable(msg.sender) {

```

</details> 
 


 ### <a name="L-7"></a>[L-7]
 ### Owner can renounce Ownership  
Each of the following contracts implements or inherits the renounceOwnership() function. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

81:     ) Ownable(msg.sender) {

```

</details> 
 


 ### <a name="L-8"></a>[L-8]
 ### Loss of precision
Division by large numbers or variables may result in the result being zero, due to Solidity not supporting fractions.

*Instances (13)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

84:         return amount * BURN_PERCENTAGE / 100;

```

```solidity
File: Governance.sol

289:             percent = (p.yVotes * PERCENT) / totalVotes;

329:             uint initialReleaseAmount = (p.fundingAmount * 10) / 100;

```

```solidity
File: PriceConvertor.sol

40:             usdAmmountinasset = (1e18 * usdAmount) / assetPrice;

42:             usdAmmountinasset = (1e6 * usdAmount) / assetPrice;

58:         uint256 usdToDaiAmount = (usdAmount * 1e18) / daiUsdPrice;

71:     uint256 ethDaiPrice = 1e18 / daiEthPrice;

```

```solidity
File: Treasury.sol

279:         uint256 daiAmountToSwap = totalAmount / 2;

282:         uint256 DaiAllocatedFunds = (daiAmountToSwap * assetAllocationRatio[0]) / 100; //@audit what happened to DaiAllocatedFunds

346:             uint256 userShare = (treasuryBal * totalAmount) /

388:         uint256 feeAmount = (totalAmountInDai * feePercentage) / 100;

672:             uint256 amountIn = (daiAmount * assetAllocationRatio[i]) / 100;

```

```solidity
File: swapp.sol

59:             uint256 amountIn = (daiAmount * assetRatios[i]) / 100;

```

</details> 
 


 ### <a name="L-9"></a>[L-9]
 ### safeApprove() is deprecated
Deprecated in favor of safeIncreaseAllowance() and safeDecreaseAllowance(). If only setting the initial allowance to the value that means infinite, safeIncreaseAllowance() can be used instead. The function may currently work, but if a bug is found in this version of OpenZeppelin, and the version that you are forced to upgrade to no longer has this function, you will encounter unnecessary delays in porting and testing replacement contracts.

*Instances (4)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

663:         TransferHelper.safeApprove(_token, SWAP_ROUTER_02, _amount);

668:         TransferHelper.safeApprove(assets[0], SWAP_ROUTER_02, daiAmount);

```

```solidity
File: swapp.sol

50:         TransferHelper.safeApprove(_token, SWAP_ROUTER_02, _amount);

55:         TransferHelper.safeApprove(DAI, SWAP_ROUTER_02, daiAmount);

```

</details> 
 


 ### <a name="L-10"></a>[L-10]
 ### State variables not capped at reasonable values
Consider adding minimum/maximum value checks to ensure that the state variables below can never be used to excessively harm users, including via griefing

*Instances (72)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

12:     address public preSaleAddress;

13:      uint256 amountMintedTopreSale;

14:     address owner;

63:     function decimals() public view virtual override returns (uint8) {

```

```solidity
File: FounderNFT.sol

18:     struct MintRequest {

19:         address to;

20:         uint256 amount;

21:         uint256 requestId;

168:     function _baseURI() internal view override returns (string memory) {

181:     function totalrequestId() external view returns (uint256){

184:      function getPendingMints() external view returns (MintRequest[] memory) {

268:         }

279:     if (lastHolderIndex >= totalHolders) {

284:     remainingHolders = totalHolders - lastHolderIndex;

```

```solidity
File: Governance.sol

30:     struct Proposal {

31:         uint PID;

32:         address creator;

33:         string title;

34:         string description;

35:         uint vote_start;

36:         uint vote_end;

37:         ProposalType ptype;

38:         ProposalStatus pStatus;

39:         uint yVotes;

40:         uint nVotes;

41:         uint minVotes;

42:         uint totalVoted;

43:         uint minPercent;

47:  struct FundRelease {

48:     uint proposalId;

49:     uint totalAmount;

50:     uint monthlyAmount;

51:     uint releasedAmount;

52:     uint startTime;

53:     uint lastReleaseTime;

54:     uint releaseCount; 

135:         }

285:         (, uint totalVotes) = p.yVotes.tryAdd(p.nVotes);

392:             if (topTreasuryHolders[i] == sender) {

395:         }

420:         votes = votes ** (1 + genesisNFTCount + founderNFTCount);

498:         emit Voted(p.PID, _supports, msg.sender);

574:         }

611:         }

634:     function getAllProposals() public view returns (Proposal[] memory) {

652:         }

657:     function getComplianers() public view returns (address[] memory) {

```

```solidity
File: PriceConvertor.sol

18:         uint256 assetAmountInUsd;

27: 

37:         uint256 usdAmmountinasset;

46: 

61:         return usdToDaiAmount;

77:     return wethToDaiAmount;

90:     return wethToDaiAmount;

```

```solidity
File: Treasury.sol

31:     address companyWallet;

32:     address deedToken;

33:     address DaiToEthPriceFeed;

43:         uint256 treasuryDeposit;

44:         uint256 firstDepositTime;

45:         uint256 deedMint;

212:                 return true;

215:         return false;

322:         uint256 totalAmount;

343:         uint256 totalAmountInUsd;

348:             uint256 assetPriceInUsd;

447:         return _totalAmount;

610:         return topTreasuryHolders;

712:         uint256 totalamountinDai;

716:             uint256 usdInassetprice;

747:         uint256 assetAmount;

764: 

```

```solidity
File: swapp.sol

84:         uint256 daiamount;

```

</details> 
 


 ### <a name="L-11"></a>[L-11]
 ### Some tokens may revert when zero value transfers are made
Despite the fact that [EIP-20](https://github.com/ethereum/EIPs/blob/7500ac4fc1bbdfaf684e7ef851f798f6b667b2fe/EIPS/eip-20.md?plain=1#L116) states that zero-value transfers must be accepted, some tokens, such as LEND, will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

72:         return super.transfer(recipient, transferAmount);

```

```solidity
File: Governance.sol

696:         payable(admin).transfer(address(this).balance);

```

```solidity
File: Treasury.sol

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

365:                 IERC20(assets[i]).transfer(companyWallet, userShare);

374:                 IERC20(assets[i]).transfer(companyWallet, usershareInDai);

635:                 token.transfer(companyWallet, balance);

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

```

</details> 
 


 ### <a name="L-12"></a>[L-12]
 ###  Functions calling contracts/addresses with transfer hooks should be protected by reentrancy guard  
Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks opens the users of this protocol up to [read-only reentrancy](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) vulnerability with no way to protect them except by block-listing the entire protocol.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

72:         return super.transfer(recipient, transferAmount);

```

```solidity
File: Governance.sol

696:         payable(admin).transfer(address(this).balance);

```

```solidity
File: Treasury.sol

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

365:                 IERC20(assets[i]).transfer(companyWallet, userShare);

374:                 IERC20(assets[i]).transfer(companyWallet, usershareInDai);

635:                 token.transfer(companyWallet, balance);

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

```

</details> 
 


 ### <a name="L-13"></a>[L-13]
 ### Some tokens may revert when large transfers are made
Tokens such as COMP or UNI will revert when an address balance reaches type(uint96).max. Ensure that the calls below can be broken up into smaller batches if necessary.  

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

72:         return super.transfer(recipient, transferAmount);

```

```solidity
File: Governance.sol

696:         payable(admin).transfer(address(this).balance);

```

```solidity
File: Treasury.sol

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

365:                 IERC20(assets[i]).transfer(companyWallet, userShare);

374:                 IERC20(assets[i]).transfer(companyWallet, usershareInDai);

635:                 token.transfer(companyWallet, balance);

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

```

```solidity
File: swapp.sol

46:         TransferHelper.safeTransfer(_token, owner, amount);

```

</details> 
 


 ### <a name="L-14"></a>[L-14]
 ### Unsafe casting
Unsafe casting can cause alot of issues especially when a type is downcast to a smaller type, the higher order bits are truncated, effectively applying a modulo to the original value. Without any other checks, this wrapping will lead to unexpected behavior and bugs

*Instances (12)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

19:         _mint(_owner, INITIAL_SUPPLY);

50:         _mint(account, amount);

59:         _mint(account, amount);

```

```solidity
File: FounderNFT.sol

51:     function initiateMint(address _to, uint256 _amount) external {

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

82:             _safeMint(mintRequest.to, i);

91:         _removePendingMint(index);

141:     function _removePendingMint(uint256 index) internal {

214:                 _safeMint(holder, _tokenId);

```

```solidity
File: Treasury.sol

10:     function mint(address to, uint256 amount) external;

621:         uint8 decimals = uint8(assetsdecimals[assetIndex]);

761:             uint8 decimal = uint8(assetsdecimals[assetIndex]);

```

</details> 
 


 ### <a name="L-15"></a>[L-15]
 ### Unsafe ERC20 operation(s)

*Instances (16)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

72:         return super.transfer(recipient, transferAmount);

80:         return super.transferFrom(sender, recipient, transferAmount);

```

```solidity
File: Governance.sol

696:         payable(admin).transfer(address(this).balance);

```

```solidity
File: Treasury.sol

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

148:         IERC721(founderNFT).transferFrom(msg.sender, address(this), tokenId);

168:         IERC721(genesisNFT).transferFrom(msg.sender, address(this), tokenId);

198:         IERC721(_nftContract).transferFrom(address(this), msg.sender, _tokenId); //@audit safeTransferFrom

272:         bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

365:                 IERC20(assets[i]).transfer(companyWallet, userShare);

374:                 IERC20(assets[i]).transfer(companyWallet, usershareInDai);

471:             IERC20(assets[i]).approve(

481:             IERC20(assets[i]).approve(governance, 0);

635:                 token.transfer(companyWallet, balance);

651:             nft.transferFrom(address(this), companyWallet, tokenId);

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

```

</details> 
 


 ### <a name="L-16"></a>[L-16]
 ### Unspecific compiler version pragma

*Instances (3)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

```

</details> 
 


## Non Critical Issues


 ### <a name="NC-1"></a>[NC-1]
 ### Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

3: pragma abicoder v2;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

3: pragma abicoder v2;

```

</details> 
 


 ### <a name="NC-2"></a>[NC-2]
 ### Consider adding formal verification proofs
Consider using formal verification to mathematically prove that your code does what is intended, and does not have any edge cases with unexpected behavior. The solidity compiler itself has this functionality [built in](https://docs.soliditylang.org/en/latest/smtchecker.html#smtchecker-and-formal-verification)  

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

3: pragma abicoder v2;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

3: pragma abicoder v2;

```

</details> 
 


 ### <a name="NC-3"></a>[NC-3]
 ### Large or complicated code bases should implement invariant tests
Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement [invariant fuzzing tests](https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05). Invariant fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and invariant fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.  

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

3: pragma abicoder v2;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

3: pragma abicoder v2;

```

</details> 
 


 ### <a name="NC-4"></a>[NC-4]
 ### NatSpec: Interface declarations should have NatSpec descriptions
e.g. @dev or @notice, and it must appear above the contract definition braces in order to be identified by the compiler as NatSpec

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

8: 

12: 

```

</details> 
 


 ### <a name="NC-5"></a>[NC-5]
 ### Variables without visibility specifier

#### Impact:
Specifying visibility for variables can improve code readability and maintainability.

*Instances (44)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

13:      uint256 amountMintedTopreSale;

14:     address owner;

15:     uint256 amountMintedToTreasury;

```

```solidity
File: FounderNFT.sol

19:         address to;

20:         uint256 amount;

21:         uint256 requestId;

22:         bool completed;

```

```solidity
File: Governance.sol

31:         uint PID;

32:         address creator;

33:         string title;

34:         string description;

35:         uint vote_start;

36:         uint vote_end;

39:         uint yVotes;

40:         uint nVotes;

41:         uint minVotes;

42:         uint totalVoted;

43:         uint minPercent;

44:         uint fundingAmount;

48:     uint proposalId;

49:     uint totalAmount;

50:     uint monthlyAmount;

51:     uint releasedAmount;

52:     uint startTime;

53:     uint lastReleaseTime;

54:     uint releaseCount; 

55:     bool isStopped;   

287:         uint percent;

590:         require(msg.sender == treasury); //TODO:real address

```

```solidity
File: PriceConvertor.sol

18:         uint256 assetAmountInUsd;

37:         uint256 usdAmmountinasset;

```

```solidity
File: Treasury.sol

31:     address companyWallet;

32:     address deedToken;

33:     address DaiToEthPriceFeed;

43:         uint256 treasuryDeposit;

44:         uint256 firstDepositTime;

45:         uint256 deedMint;

322:         uint256 totalAmount;

343:         uint256 totalAmountInUsd;

348:             uint256 assetPriceInUsd;

712:         uint256 totalamountinDai;

716:             uint256 usdInassetprice;

747:         uint256 assetAmount;

```

```solidity
File: swapp.sol

84:         uint256 daiamount;

```

</details> 
 


 ### <a name="NC-6"></a>[NC-6]
 ### Array is push()ed but not pop()ed

#### Impact:
Array entries are added but are never removed. Consider whether this should be the case, or whether there should be a maximum, or whether old entries should be removed.

*Instances (12)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

207:         proposals.push(p);

208:         uProposals[msg.sender].push(p.PID);

315:                 complianceMembers.push(p.creator);

667:         complianceMembers.push(_complianer);

```

```solidity
File: Treasury.sol

127:         lockedNFTs[msg.sender][deedNFT].push(tokenId);

128:         contractlockedNFTs[address(this)][deedNFT].push(tokenId);

149:         lockedNFTs[msg.sender][founderNFT].push(tokenId);

150:         contractlockedNFTs[address(this)][founderNFT].push(tokenId);

169:         lockedNFTs[msg.sender][genesisNFT].push(tokenId);

170:         contractlockedNFTs[address(this)][genesisNFT].push(tokenId);

308:             userDeposits[users[i]].push(newUserDeposit);

508:                     topTreasuryHolders.push(user);

```

</details> 
 


 ### <a name="NC-7"></a>[NC-7]
 ### Constants in comparisons should appear on the left side

#### Impact:
Placing constants on the left side of comparisons can improve code readability and prevent accidental assignment. Doing so will prevent typo [bugs](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html)

*Instances (46)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

144:         if (index != lastIndex) {

203:         if (endIndex > totalHolders) {

224:         if (lastHolderIndex >= totalHolders) {

253:                 if (holders[j] == holder) {

279:     if (lastHolderIndex >= totalHolders) {

```

```solidity
File: Governance.sol

131:             if (s == complianceMembers[i]) {

201:         if (_ptype == ProposalType.COMPLIANCE) {

223:         if (agreed == 0) return false;

246:         if (p.vote_start < block.timestamp) {

288:         if (p.yVotes > 0) {

304:         if (p.ptype == ProposalType.COMPLIANCE) {

319:         if (p.fundingAmount > 0) {

356:      if (release.releasedAmount + amountToRelease > release.totalAmount) {

392:             if (topTreasuryHolders[i] == sender) {

415:         if (treasuryBal > 0) {

443:         if (p.ptype == ProposalType.OPEN) {

456:         } else if (p.ptype == ProposalType.TREASURY) {

474:         } else if (

485:         } else if (

519:         if (p.ptype == ProposalType.OPEN) {

531:         } else if (p.ptype == ProposalType.TREASURY) {

547:         } else if (

556:         } else if (

648:             if (proposals[i].pStatus == ProposalStatus.PENDING) {

676:             if (complianceMembers[i] == _complianer) {

682:         if (i < len) {

```

```solidity
File: PriceConvertor.sol

20:         if (decimals == 18) {

22:         } else if (decimals == 6) {

39:         if (decimals == 18) {

41:         } else if (decimals == 6) {

```

```solidity
File: Treasury.sol

190:             if (tokens[i] == _tokenId) {

211:             if (lockedTokens[i] == tokenId) {

226:             if (lockedTokens[i] == tokenId) {

350:             if (assets[i] == WETH) {

364:             if (assets[i] == assets[0]) {

424:             if (index != lastIndex) {

499:         if (treasuryBalances[user] > 0) {

507:                 if (topTreasuryHolders.length < 10) {

532:             if (treasuryBalances[holder] < smallestBalance) {

548:         if (index < lastIndex) {

577:         if (n <= 1) {

588:                 if (treasuryBalances[holder1] < treasuryBalances[holder2]) {

634:             if (balance > 0) {

721:             if (assets[i] == assets[0]) {

749:         if (assets[assetIndex] == WETH) {

```

```solidity
File: swapp.sol

87:         if(assets[i]==assets[0]){ 

```

</details> 
 


 ### <a name="NC-8"></a>[NC-8]
 ### constants should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

21:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

22:     address public SWAP_ROUTER_02 = 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45;

```

```solidity
File: swapp.sol

20:     address public SWAP_ROUTER_02 = 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45;

30:     address public constant DAI = 0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063;

31:     address public constant USDC = 0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359;

32:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

33:     address public constant WBTC = 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6;

34:     address public constant USDT = 0xc2132D05D31c914a87C6611C10748AEb04B58e8F;

```

</details> 
 


 ### <a name="NC-9"></a>[NC-9]
 ### Contract declarations should have NatSpec @author annotations

#### Impact:
Adding a NatSpec @author annotation to contract declarations can improve code documentation.

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-10"></a>[NC-10]
 ### Contract declarations should have NatSpec @Title annotations

#### Impact:
Adding a NatSpec @Title annotation to contract declarations can improve code documentation.

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-11"></a>[NC-11]
 ### NatSpec: Contract declarations should have @dev tags

#### Impact:
@dev is used to explain extra details to developers

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-12"></a>[NC-12]
 ### NatSpec: Contract declarations should have NatSpec descriptions
e.g. @dev or @notice, and it must appear above the contract definition braces in order to be identified by the compiler as NatSpec

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-13"></a>[NC-13]
 ### NatSpec: Contract declarations should have @notice tags
@notice is used to explain to end users what the contract does, and the compiler interprets /// or /** comments as this tag if one wasnt explicitly provided

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-14"></a>[NC-14]
 ### Consider using delete rather than assigning zero to clear value

#### Impact:
Consider using delete rather than assigning zero to clear values. The delete keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

*Instances (38)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

11:     uint256 private requestId = 0;

13:      uint256 public lastHolderIndex = 0;

213:             for (uint256 j = 0; j < holderBalance; j++) {

225:             lastHolderIndex = 0;

246:         uint256 holderCount = 0;

252:             for (uint256 j = 0; j < holderCount; j++) {

266:         for (uint256 k = 0; k < holderCount; k++) {

```

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

199:         p.totalVoted = 0;

217:         uint agreed = 0;

218:         for (uint i = 0; i < complianceMembers.length; i++) {

292:             percent = 0;

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

646:         uint ci = 0;

647:         for (uint i = 0; i < proposals.length; i++) {

671:         uint i = 0;

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

337:             userDeposit.treasuryDeposit = 0;

338:             userDeposit.firstDepositTime = 0; // Set deposit to 0 to prevent double withdrawal

344:         for (uint256 i = 0; i < assets.length; i++) {

440:         uint256 _totalAmount = 0;

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

461:         uint8 sum = 0;

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

528:         uint256 indexToReplace = 0;

582:         for (uint256 i = 0; i < n - 1; i++) {

583:             for (uint256 j = 0; j < n - i - 1; j++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

649:         for (uint256 i = 0; i < totalLocked; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="NC-15"></a>[NC-15]
 ### Consider using delete rather than assigning false to clear value

#### Impact:
Consider using delete rather than assigning alse to clear values. The delete keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

250:             bool isAlreadyCounted = false;

```

```solidity
File: Governance.sol

129:         bool _isComplianer = false;

```

</details> 
 


 ### <a name="NC-16"></a>[NC-16]
 ### Consider adding a block/deny-list"
Doing so will significantly increase centralization, but will help to prevent hackers from using stolen tokens  

*Instances (12)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

122:             "Contract is not approved to transfer this NFT"

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

145:             "Contract is not approved to transfer this NFT"

165:             "Contract is not approved to transfer this NFT"

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-17"></a>[NC-17]
 ### Use bytes.concat() on bytes instead of abi.encodePacked() for clearer semantic meaning
Starting with version 0.8.4, Solidity has the bytes.concat() function, which allows one to concatenate a list of bytes/strings, without extra padding. Using this function rather than abi.encodePacked() makes the intended operation more clear, leading to less reviewer confusion.  

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

174:         return string(abi.encodePacked(baseTokenURI, Strings.toString(tokenId), ".json"));

```

</details> 
 


 ### <a name="NC-18"></a>[NC-18]
 ### Consider adding emergency-stop functionality

#### Impact:
Adding a way to quickly halt protocol functionality in an emergency, rather than having to pause individual contracts one-by-one, will make in-progress hack mitigation faster and much less stressful.

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

6: contract DeedToken is ERC20{

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

5: contract FounderNFT is ERC721 {

```

```solidity
File: Governance.sol

10: contract Governance is ReentrancyGuard {

```

```solidity
File: PriceConvertor.sol

5: contract PriceConverter {

```

```solidity
File: Treasury.sol

17: contract Treasury is Ownable, ReentrancyGuard, PriceConverter {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

```solidity
File: swapp.sol

6: contract Swap {

```

</details> 
 


 ### <a name="NC-19"></a>[NC-19]
 ### Events are missing sender information

#### Impact:
Including msg.sender in events triggered by user actions can make events more useful for end users.

*Instances (14)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

69:         emit MintInitiated(_to, requestId, _amount);

89:         emit MintCompleted(mintRequest.to, mintRequest.requestId, amount);

226:             emit AllHoldersProcessed(totalHolders);

```

```solidity
File: Governance.sol

300:             emit ProposalTallied(p.PID, false, percent);

312:                 emit ProposalTallied(p.PID, false, percent);

345:         emit ProposalStatusChanged(p.PID, ProposalStatus.COMPLETED);

498:         emit Voted(p.PID, _supports, msg.sender);

```

```solidity
File: Treasury.sol

129:         emit NFTLocked(msg.sender, deedNFT, tokenId);

151:         emit NFTLocked(msg.sender, founderNFT, tokenId);

171:         emit NFTLocked(msg.sender, genesisNFT, tokenId);

199:         emit NFTUnlocked(msg.sender, _nftContract, _tokenId);

314:             emit Deposit(users[i], usdcAmounts[i]);

409:         emit Withdraw(msg.sender, totalAmount, daiAmountToMint);

494:         emit GovernanceUpdated(oldGovernance, _newGovernance);

```

</details> 
 


 ### <a name="NC-20"></a>[NC-20]
 ### NatSpec: Event declarations should have NatSpec descriptions

*Instances (12)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

31:     event MintInitiated(address indexed to, uint256 indexed requestId, uint256 indexed amount);

32:     event MintCompleted(address indexed to, uint256 indexed requestId, uint256 indexed amount);

33:    event AllHoldersProcessed(uint256 totalHolders);

```

```solidity
File: Governance.sol

83:     event ProposalAdded(

90:     event ProposalStatusChanged(uint indexed proposalID, ProposalStatus status);

91:     event Voted(uint indexed proposalID, bool position, address indexed voter);

92:     event ProposalTallied(

```

```solidity
File: Treasury.sol

58:     event Deposit(address user, uint256 treasuryPoints);

59:     event GovernanceUpdated(address oldGovernance, address newGovernance); // more events

60:     event NFTLocked(address indexed user, address nftContract, uint256 tokenId);

61:     event NFTUnlocked(

66:     event Withdraw(address user, uint256 deposits, uint256 deedMinted);

```

</details> 
 


 ### <a name="NC-21"></a>[NC-21]
 ### contracts should use fixed compiler versions
To prevent the contracts being deployed and behaving differently depending on the compiler version, it is recommended to use fixed solidity versions for contracts and libraries. Although we can configure a specific version through config (like hardhat, forge config files), it is recommended to set the fixed version in the solidity pragma directly before deploying to the mainnet.

*Instances (6)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

```

</details> 
 


 ### <a name="NC-22"></a>[NC-22]
 ### NatSpec: function declarations should have NatSpec descriptions

*Instances (101)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

48:     function mintForTreasury(address account, uint256 amount) external onlyTreasuryContract {

57:     function mintForPreSale(address account, uint256 amount) external onlypreSaleContract {

63:     function decimals() public view virtual override returns (uint8) {

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

83:     function _calculateBurnAmount(uint256 amount) internal pure returns (uint256) {

```

```solidity
File: FounderNFT.sol

51:     function initiateMint(address _to, uint256 _amount) external {

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

141:     function _removePendingMint(uint256 index) internal {

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

168:     function _baseURI() internal view override returns (string memory) {

172:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

177:     function totalSupply() external view returns (uint256) {

181:     function totalrequestId() external view returns (uint256){

184:      function getPendingMints() external view returns (MintRequest[] memory) {

188:     function getIndexFromRequestId(uint256 _requestId) internal view returns (uint256) {

194:     function mintBatchTokensForHolders(uint256 batchSize) external onlyWarehouseWallet {

231:     function hasAllHoldersBeenProcessed() external view returns (bool) {

243:     function _getHolders() internal view returns (address[] memory) {

274: function getRemainingHolders() external view returns (uint256 remainingHolders) {

```

```solidity
File: Governance.sol

128:     function isComplianceMember(address s) public view returns (bool) {

152:     function createProposal(

216:     function isProposalAgreed(uint _pId) internal view returns (bool) {

230:     function voteByComplianer(

260:     function makeItLive(uint _propID) public OnlyComplianceMember {

281:     function executeProposal(uint _pId) external nonReentrant {

348: function releaseFunds(uint _proposalId) external {

372: function stopFunding(uint _proposalId) external OnlyComplianceMember {

386:     function isTopTreasurer(address sender) internal view returns (bool) {

401:     function getVotingPower(address voter) public view returns (uint) {

428:     function voteByUser(uint _propID, bool _supports) public returns (bool) {

504:     function withdrawVote(uint _propID) public returns (bool) {

580:     function getTreasuryManagerQueue()

595:     function getProposal(uint pID) public view returns (Proposal memory) {

601:     function numOfProposals() external view returns (uint) {

606:     function getCreatorProposals() external view returns (Proposal[] memory) {

616:     function getProposalYayVotes(uint _pid) external view returns (uint) {

622:     function getProposalNayVotes(uint _pid) external view returns (uint) {

627:     function getProposalStatus(

634:     function getAllProposals() public view returns (Proposal[] memory) {

639:     function getPendingProposals()

657:     function getComplianers() public view returns (address[] memory) {

661:     function setTreasury(address _treasury) external onlyAdmin {

666:     function addComplianer(address _complianer) public onlyAdmin {

670:     function removeComplianer(address _complianer) public onlyAdmin {

690:     function revokeAdmin(address _newAdmin) public onlyAdmin {

694:     function withdrawCreateProposalFee() external onlyAdmin {

```

```solidity
File: PriceConvertor.sol

6:     function getPrice(AggregatorV3Interface priceFeed) public view returns (uint256) {

12:     function getAmountInUsd(

31:     function getusdAmountInassets(

53:     function getUsdToDaiAmount(

64:     function getWethToDaiAmount(

79:   function getDaitoWethAmount(

```

```solidity
File: Treasury.sol

10:     function mint(address to, uint256 amount) external;

14:     function mintForTreasury(address account, uint256 amount) external;

110:     function lockDeedNFT(uint256 tokenId) external {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

132:     function lockFounderNFT(uint256 tokenId) external {

154:     function lockGenesisNFT(uint256 tokenId) external {

175:     function unlockNFT(

203:     function _isLocked(

219:     function _removeLockedNFT(

235:     function getLockedNFTs(

242:     function getLockedNFTBalances( //@audit name should be getLockedNFTCount

258:     function deposit_admin2(

264:         uint256 totalAmount = calculateTotalDeposit(users, usdcAmounts); //@audit this function is just calculation the deposit

321:     function withdraw(uint256[] memory indices) external nonReentrant {

414:     function removeUserDeposits(

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

436:     function calculateTotalDeposit(

451:     function setAllocationRatios(

459:     function calculateSumOfRatios(uint8[] memory _ratios) public pure {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

498:     function updateTopTreasuryHolders(address user) internal {

525:     function _replaceSmallestHolder(address newUser) internal {

544:     function _removeTopHolder(address user, uint256 index) internal {

575:     function sortTopHolders() internal {

603:     function getTreasuryBalance(

609:     function getTopTreasuryHolders() public view returns (address[] memory) {

613:     function fetchAssetPriceInUsd(

625:     function getUserDeposits() external view returns (User[] memory) {

629:     function drainTreasury() external onlyOwner {

644:     function _transferAllNFTs(address nftContract) internal {

658:     function setSwapRouter(address _router) public { //@audit anyone can change the router address?

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

666:     function deposit(uint256 daiAmount) public {

684:     function swapExactInputSingle(

706:     function swapAndTransferToGovernance(

738:     function fetchUsdIntoAssetsPrice(

```

```solidity
File: swapp.sol

41:     function setSwapRouter(address _router) public {

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

53:     function deposit(uint256 daiAmount) external  {

66:     function swapExactInputSingle(address tokenIn, address tokenOut, uint256 amountIn, uint24 feeTier)

83:     function withdraw(uint amount) external {

```

</details> 
 


 ### <a name="NC-23"></a>[NC-23]
 ### NatSpec: function declarations should have @Notice tags
@notice is used to explain to end users what the function does, and the compiler interprets /// or /** comments as this tag if one wasn't explicitly provided.  

*Instances (101)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

48:     function mintForTreasury(address account, uint256 amount) external onlyTreasuryContract {

57:     function mintForPreSale(address account, uint256 amount) external onlypreSaleContract {

63:     function decimals() public view virtual override returns (uint8) {

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

83:     function _calculateBurnAmount(uint256 amount) internal pure returns (uint256) {

```

```solidity
File: FounderNFT.sol

51:     function initiateMint(address _to, uint256 _amount) external {

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

141:     function _removePendingMint(uint256 index) internal {

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

168:     function _baseURI() internal view override returns (string memory) {

172:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

177:     function totalSupply() external view returns (uint256) {

181:     function totalrequestId() external view returns (uint256){

184:      function getPendingMints() external view returns (MintRequest[] memory) {

188:     function getIndexFromRequestId(uint256 _requestId) internal view returns (uint256) {

194:     function mintBatchTokensForHolders(uint256 batchSize) external onlyWarehouseWallet {

231:     function hasAllHoldersBeenProcessed() external view returns (bool) {

243:     function _getHolders() internal view returns (address[] memory) {

274: function getRemainingHolders() external view returns (uint256 remainingHolders) {

```

```solidity
File: Governance.sol

128:     function isComplianceMember(address s) public view returns (bool) {

152:     function createProposal(

216:     function isProposalAgreed(uint _pId) internal view returns (bool) {

230:     function voteByComplianer(

260:     function makeItLive(uint _propID) public OnlyComplianceMember {

281:     function executeProposal(uint _pId) external nonReentrant {

348: function releaseFunds(uint _proposalId) external {

372: function stopFunding(uint _proposalId) external OnlyComplianceMember {

386:     function isTopTreasurer(address sender) internal view returns (bool) {

401:     function getVotingPower(address voter) public view returns (uint) {

428:     function voteByUser(uint _propID, bool _supports) public returns (bool) {

504:     function withdrawVote(uint _propID) public returns (bool) {

580:     function getTreasuryManagerQueue()

595:     function getProposal(uint pID) public view returns (Proposal memory) {

601:     function numOfProposals() external view returns (uint) {

606:     function getCreatorProposals() external view returns (Proposal[] memory) {

616:     function getProposalYayVotes(uint _pid) external view returns (uint) {

622:     function getProposalNayVotes(uint _pid) external view returns (uint) {

627:     function getProposalStatus(

634:     function getAllProposals() public view returns (Proposal[] memory) {

639:     function getPendingProposals()

657:     function getComplianers() public view returns (address[] memory) {

661:     function setTreasury(address _treasury) external onlyAdmin {

666:     function addComplianer(address _complianer) public onlyAdmin {

670:     function removeComplianer(address _complianer) public onlyAdmin {

690:     function revokeAdmin(address _newAdmin) public onlyAdmin {

694:     function withdrawCreateProposalFee() external onlyAdmin {

```

```solidity
File: PriceConvertor.sol

6:     function getPrice(AggregatorV3Interface priceFeed) public view returns (uint256) {

12:     function getAmountInUsd(

31:     function getusdAmountInassets(

53:     function getUsdToDaiAmount(

64:     function getWethToDaiAmount(

79:   function getDaitoWethAmount(

```

```solidity
File: Treasury.sol

10:     function mint(address to, uint256 amount) external;

14:     function mintForTreasury(address account, uint256 amount) external;

110:     function lockDeedNFT(uint256 tokenId) external {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

132:     function lockFounderNFT(uint256 tokenId) external {

154:     function lockGenesisNFT(uint256 tokenId) external {

175:     function unlockNFT(

203:     function _isLocked(

219:     function _removeLockedNFT(

235:     function getLockedNFTs(

242:     function getLockedNFTBalances( //@audit name should be getLockedNFTCount

258:     function deposit_admin2(

264:         uint256 totalAmount = calculateTotalDeposit(users, usdcAmounts); //@audit this function is just calculation the deposit

321:     function withdraw(uint256[] memory indices) external nonReentrant {

414:     function removeUserDeposits(

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

436:     function calculateTotalDeposit(

451:     function setAllocationRatios(

459:     function calculateSumOfRatios(uint8[] memory _ratios) public pure {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

498:     function updateTopTreasuryHolders(address user) internal {

525:     function _replaceSmallestHolder(address newUser) internal {

544:     function _removeTopHolder(address user, uint256 index) internal {

575:     function sortTopHolders() internal {

603:     function getTreasuryBalance(

609:     function getTopTreasuryHolders() public view returns (address[] memory) {

613:     function fetchAssetPriceInUsd(

625:     function getUserDeposits() external view returns (User[] memory) {

629:     function drainTreasury() external onlyOwner {

644:     function _transferAllNFTs(address nftContract) internal {

658:     function setSwapRouter(address _router) public { //@audit anyone can change the router address?

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

666:     function deposit(uint256 daiAmount) public {

684:     function swapExactInputSingle(

706:     function swapAndTransferToGovernance(

738:     function fetchUsdIntoAssetsPrice(

```

```solidity
File: swapp.sol

41:     function setSwapRouter(address _router) public {

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

53:     function deposit(uint256 daiAmount) external  {

66:     function swapExactInputSingle(address tokenIn, address tokenOut, uint256 amountIn, uint24 feeTier)

83:     function withdraw(uint amount) external {

```

</details> 
 


 ### <a name="NC-24"></a>[NC-24]
 ### NatSpec: function declarations should have NatSpec descriptions

*Instances (101)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

48:     function mintForTreasury(address account, uint256 amount) external onlyTreasuryContract {

57:     function mintForPreSale(address account, uint256 amount) external onlypreSaleContract {

63:     function decimals() public view virtual override returns (uint8) {

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

83:     function _calculateBurnAmount(uint256 amount) internal pure returns (uint256) {

```

```solidity
File: FounderNFT.sol

51:     function initiateMint(address _to, uint256 _amount) external {

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

141:     function _removePendingMint(uint256 index) internal {

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

168:     function _baseURI() internal view override returns (string memory) {

172:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

177:     function totalSupply() external view returns (uint256) {

181:     function totalrequestId() external view returns (uint256){

184:      function getPendingMints() external view returns (MintRequest[] memory) {

188:     function getIndexFromRequestId(uint256 _requestId) internal view returns (uint256) {

194:     function mintBatchTokensForHolders(uint256 batchSize) external onlyWarehouseWallet {

231:     function hasAllHoldersBeenProcessed() external view returns (bool) {

243:     function _getHolders() internal view returns (address[] memory) {

274: function getRemainingHolders() external view returns (uint256 remainingHolders) {

```

```solidity
File: Governance.sol

128:     function isComplianceMember(address s) public view returns (bool) {

152:     function createProposal(

216:     function isProposalAgreed(uint _pId) internal view returns (bool) {

230:     function voteByComplianer(

260:     function makeItLive(uint _propID) public OnlyComplianceMember {

281:     function executeProposal(uint _pId) external nonReentrant {

348: function releaseFunds(uint _proposalId) external {

372: function stopFunding(uint _proposalId) external OnlyComplianceMember {

386:     function isTopTreasurer(address sender) internal view returns (bool) {

401:     function getVotingPower(address voter) public view returns (uint) {

428:     function voteByUser(uint _propID, bool _supports) public returns (bool) {

504:     function withdrawVote(uint _propID) public returns (bool) {

580:     function getTreasuryManagerQueue()

595:     function getProposal(uint pID) public view returns (Proposal memory) {

601:     function numOfProposals() external view returns (uint) {

606:     function getCreatorProposals() external view returns (Proposal[] memory) {

616:     function getProposalYayVotes(uint _pid) external view returns (uint) {

622:     function getProposalNayVotes(uint _pid) external view returns (uint) {

627:     function getProposalStatus(

634:     function getAllProposals() public view returns (Proposal[] memory) {

639:     function getPendingProposals()

657:     function getComplianers() public view returns (address[] memory) {

661:     function setTreasury(address _treasury) external onlyAdmin {

666:     function addComplianer(address _complianer) public onlyAdmin {

670:     function removeComplianer(address _complianer) public onlyAdmin {

690:     function revokeAdmin(address _newAdmin) public onlyAdmin {

694:     function withdrawCreateProposalFee() external onlyAdmin {

```

```solidity
File: PriceConvertor.sol

6:     function getPrice(AggregatorV3Interface priceFeed) public view returns (uint256) {

12:     function getAmountInUsd(

31:     function getusdAmountInassets(

53:     function getUsdToDaiAmount(

64:     function getWethToDaiAmount(

79:   function getDaitoWethAmount(

```

```solidity
File: Treasury.sol

10:     function mint(address to, uint256 amount) external;

14:     function mintForTreasury(address account, uint256 amount) external;

110:     function lockDeedNFT(uint256 tokenId) external {

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

132:     function lockFounderNFT(uint256 tokenId) external {

154:     function lockGenesisNFT(uint256 tokenId) external {

175:     function unlockNFT(

203:     function _isLocked(

219:     function _removeLockedNFT(

235:     function getLockedNFTs(

242:     function getLockedNFTBalances( //@audit name should be getLockedNFTCount

258:     function deposit_admin2(

264:         uint256 totalAmount = calculateTotalDeposit(users, usdcAmounts); //@audit this function is just calculation the deposit

321:     function withdraw(uint256[] memory indices) external nonReentrant {

414:     function removeUserDeposits(

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

436:     function calculateTotalDeposit(

451:     function setAllocationRatios(

459:     function calculateSumOfRatios(uint8[] memory _ratios) public pure {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

498:     function updateTopTreasuryHolders(address user) internal {

525:     function _replaceSmallestHolder(address newUser) internal {

544:     function _removeTopHolder(address user, uint256 index) internal {

575:     function sortTopHolders() internal {

603:     function getTreasuryBalance(

609:     function getTopTreasuryHolders() public view returns (address[] memory) {

613:     function fetchAssetPriceInUsd(

625:     function getUserDeposits() external view returns (User[] memory) {

629:     function drainTreasury() external onlyOwner {

644:     function _transferAllNFTs(address nftContract) internal {

658:     function setSwapRouter(address _router) public { //@audit anyone can change the router address?

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

666:     function deposit(uint256 daiAmount) public {

684:     function swapExactInputSingle(

706:     function swapAndTransferToGovernance(

738:     function fetchUsdIntoAssetsPrice(

```

```solidity
File: swapp.sol

41:     function setSwapRouter(address _router) public {

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

53:     function deposit(uint256 daiAmount) external  {

66:     function swapExactInputSingle(address tokenIn, address tokenOut, uint256 amountIn, uint24 feeTier)

83:     function withdraw(uint amount) external {

```

</details> 
 


 ### <a name="NC-25"></a>[NC-25]
 ### If-statement can be converted to a ternary

#### Impact:
The code can be made more compact while also increasing readability by converting the following `if`-statements to ternaries (e.g. `foo += (x > y) ? a : b`)

*Instances (50)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

144:         if (index != lastIndex) {

203:         if (endIndex > totalHolders) {

224:         if (lastHolderIndex >= totalHolders) {

253:                 if (holders[j] == holder) {

259:             if (!isAlreadyCounted) {

279:     if (lastHolderIndex >= totalHolders) {

```

```solidity
File: Governance.sol

131:             if (s == complianceMembers[i]) {

201:         if (_ptype == ProposalType.COMPLIANCE) {

219:             if (complianerVotes[_pId][complianceMembers[i]]) {

246:         if (p.vote_start < block.timestamp) {

270:         if (isProposalAgreed(p.PID)) {

288:         if (p.yVotes > 0) {

298:         if (!criteria) {

304:         if (p.ptype == ProposalType.COMPLIANCE) {

319:         if (p.fundingAmount > 0) {

356:      if (release.releasedAmount + amountToRelease > release.totalAmount) {

392:             if (topTreasuryHolders[i] == sender) {

415:         if (treasuryBal > 0) {

443:         if (p.ptype == ProposalType.OPEN) {

456:         } else if (p.ptype == ProposalType.TREASURY) {

477:             if (_supports) {

488:             if (_supports) {

519:         if (p.ptype == ProposalType.OPEN) {

531:         } else if (p.ptype == ProposalType.TREASURY) {

550:             if (_supports) {

568:             if (_supports) {

648:             if (proposals[i].pStatus == ProposalStatus.PENDING) {

676:             if (complianceMembers[i] == _complianer) {

682:         if (i < len) {

```

```solidity
File: PriceConvertor.sol

20:         if (decimals == 18) {

22:         } else if (decimals == 6) {

39:         if (decimals == 18) {

41:         } else if (decimals == 6) {

```

```solidity
File: Treasury.sol

190:             if (tokens[i] == _tokenId) {

211:             if (lockedTokens[i] == tokenId) {

226:             if (lockedTokens[i] == tokenId) {

350:             if (assets[i] == WETH) {

364:             if (assets[i] == assets[0]) {

424:             if (index != lastIndex) {

499:         if (treasuryBalances[user] > 0) {

511:                 } else {

516:         } else {

532:             if (treasuryBalances[holder] < smallestBalance) {

548:         if (index < lastIndex) {

577:         if (n <= 1) {

588:                 if (treasuryBalances[holder1] < treasuryBalances[holder2]) {

634:             if (balance > 0) {

721:             if (assets[i] == assets[0]) {

749:         if (assets[assetIndex] == WETH) {

```

```solidity
File: swapp.sol

87:         if(assets[i]==assets[0]){ 

```

</details> 
 


 ### <a name="NC-26"></a>[NC-26]
 ### Large multiples of ten should use scientific notation
Use a scientific notation rather than decimal literals (e.g. 1e6 instead of 1000000), for better code readability.

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

12:     uint256 public constant MAX_SUPPLY = 222000;

```

</details> 
 


 ### <a name="NC-27"></a>[NC-27]
 ### Consider using named mappings

#### Impact:
Using named mappings can improve code readability and maintainability.

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

26:     mapping(uint256 => uint256) private requestIdToIndex;

29:      mapping(address => uint256) private holderTokenCount;

```

</details> 
 


 ### <a name="NC-28"></a>[NC-28]
 ### Consider combining multiple address/ID mappings into a single mapping of an address/ID to a struct

#### Impact:
Combining multiple mappings into a single mapping with a struct can improve readability and maintainability of the code.

*Instances (10)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

29:      mapping(address => uint256) private holderTokenCount;

```

```solidity
File: Governance.sol

74:     mapping(address => uint[]) public uProposals;

78:     mapping(uint => mapping(address => bool)) public complianerVotes;

81:     mapping(uint => mapping(address => int)) public voterVotes;

```

```solidity
File: Treasury.sol

39:     mapping(address => uint256) public treasuryHolderIndex;

40:     mapping(address => uint256) public treasuryBalances;

47:     mapping(address => User[]) public userDeposits; // in dollars User[]

54:     mapping(address => mapping(address => uint256[])) public lockedNFTs;

55:     mapping(address => mapping(address => uint256[])) public contractlockedNFTs;

56:     mapping(address => bool) public isTopTreasuryHolder;

```

</details> 
 


 ### <a name="NC-29"></a>[NC-29]
 ### Use of override is unnecessary

#### Impact:
Starting with Solidity version [0.8.8](https://docs.soliditylang.org/en/v0.8.20/contracts.html#function-overriding), using the override keyword when the function solely overrides an interface function, and the function doesnt exist in multiple base contracts, is unnecessary.

*Instances (5)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

63:     function decimals() public view virtual override returns (uint8) {

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

```

```solidity
File: FounderNFT.sol

168:     function _baseURI() internal view override returns (string memory) {

172:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

```

</details> 
 


 ### <a name="NC-30"></a>[NC-30]
 ### Consider using descriptive constants when using 0 in the code

#### Impact:
Passing zero as a function argument/Event emission can sometimes result in a security issue (e.g. passing zero as the slippage parameter). Consider using a constant variable with a descriptive name, so its clear that the 0 is intentionally being used, and for the right reasons.

*Instances (24)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

53:         require(_amount > 0, "Amount must be greater than zero");

199:         require(totalHolders > 0, "No holders to process");

```

```solidity
File: Governance.sol

350:     require(release.totalAmount > 0, "Invalid Proposal ID");

374:     require(release.totalAmount > 0, "Invalid Proposal ID");

431:         require(voterVotes[_propID][msg.sender] == 0, "user already voted");

444:             require(

462:             require(

505:         require(voterVotes[_propID][msg.sender] != 0, "user not voted");

510:         require(p.pStatus == ProposalStatus.LIVE && votes != 0);

520:             require(

539:             require(treasuryBal > 0);

```

```solidity
File: Treasury.sol

324:         require(userDepositCount > 0, "No deposits found for the user");

340:         require(totalAmount > 0, "No valid deposits to withdraw");

356:                 assetPriceInUsd = fetchAssetPriceInUsd(0, wethPriceInDai);

367:                 uint256 usershareInDai = swapExactInputSingle(

378:         uint256 totalDepositInUsd = fetchAssetPriceInUsd(0, totalAmount);

442:             require(usdcAmounts[i] > 0, "Zero Amount");

481:             IERC20(assets[i]).approve(governance, 0);

668:         TransferHelper.safeApprove(assets[0], SWAP_ROUTER_02, daiAmount);

675:             swapExactInputSingle(

710:         require(amount > 0, "amount should be greater than 0");

724:                 uint256 usershareInDai = swapExactInputSingle(

```

```solidity
File: swapp.sol

70:         IV3SwapRouter.ExactInputSingleParams memory params = IV3SwapRouter.ExactInputSingleParams({

91:             daiamount+= swapExactInputSingle(assets[i],assets[0],amount,poolFeeTier[i-1]);

```

</details> 
 


 ### <a name="NC-31"></a>[NC-31]
 ### Non-external/public variable names should begin with an underscore

#### Impact:
Using an underscore at the beginning of non-external/public variable names can improve code clarity and maintainability. According to the Solidity Style Guide, non-external/public variable names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*Instances (38)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

11:     address public treasuryAddress;

12:     address public preSaleAddress;

```

```solidity
File: FounderNFT.sol

14:     address public warehouseWallet;

15:     address public routingWallet;

16:     string private baseTokenURI;

25:     MintRequest[] public pendingMints;

26:     mapping(uint256 => uint256) private requestIdToIndex;

29:      mapping(address => uint256) private holderTokenCount;

```

```solidity
File: Governance.sol

59: mapping(uint => FundRelease) public fundReleases;

66:     Proposal[] public proposals;

69:     address public treasury;

70:     address public admin;

72:     uint public pendingProposals;

74:     mapping(address => uint[]) public uProposals;

78:     mapping(uint => mapping(address => bool)) public complianerVotes;

81:     mapping(uint => mapping(address => int)) public voterVotes;

98:     IERC721 public deedNFT;

99:     IERC721 public genesisNFT;

100:     IERC721 public founderNFT;

101:     address[] public complianceMembers;

125:     address[] private treasureMngQueue;

```

```solidity
File: Treasury.sol

26:     address public governance;

27:     address[] public assets;

28:     uint256[] public assetsdecimals;

29:     address[] public priceFeed;

30:     uint8[] public assetAllocationRatio;

36:     uint256 public totalTreasuryPoints;

38:     address[] public topTreasuryHolders;

39:     mapping(address => uint256) public treasuryHolderIndex;

40:     mapping(address => uint256) public treasuryBalances;

47:     mapping(address => User[]) public userDeposits; // in dollars User[]

50:     address public deedNFT;

51:     address public founderNFT;

52:     address public genesisNFT;

54:     mapping(address => mapping(address => uint256[])) public lockedNFTs;

55:     mapping(address => mapping(address => uint256[])) public contractlockedNFTs;

56:     mapping(address => bool) public isTopTreasuryHolder;

```

```solidity
File: swapp.sol

8:     address public owner;

```

</details> 
 


 ### <a name="NC-32"></a>[NC-32]
 ### Return values of `approve()` not checked
Not all IERC20 implementations `revert()` when there's a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

*Instances (3)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

197:         // IERC721(_nftContract).approve(msg.sender, _tokenId);

471:             IERC20(assets[i]).approve(

481:             IERC20(assets[i]).approve(governance, 0);

```

</details> 
 


 ### <a name="NC-33"></a>[NC-33]
 ### Setters should prevent re-setting of the same value
This especially problematic when the setter also emits the same value, which may be confusing to offline parsers  

*Instances (9)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

```

```solidity
File: FounderNFT.sol

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

```

```solidity
File: Governance.sol

661:     function setTreasury(address _treasury) external onlyAdmin {

```

```solidity
File: Treasury.sol

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

658:     function setSwapRouter(address _router) public { //@audit anyone can change the router address?

```

```solidity
File: swapp.sol

41:     function setSwapRouter(address _router) public {

```

</details> 
 


 ### <a name="NC-34"></a>[NC-34]
 ### Use the latest solidity version for deployment  
Upgrading to a newer Solidity release can optimize gas usage, take advantage of new features and improve overall contract efficiency. Where possible, based on compatibility requirements, it is recommended to use newer/latest solidity version to take advantage of the latest optimizations and features. You can see the latest version [here](https://soliditylang.org/blog/category/releases/)

*Instances (6)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

```

</details> 
 


 ### <a name="NC-35"></a>[NC-35]
 ### Consider bounding input array length
The functions below take in an unbounded array, and make function calls for entries in the array. While the function will revert if it eventually runs out of gas, it may be a nicer user experience to require() that the length of the array is below some reasonable maximum, so that the user doesnt have to use up a full transactions gas only to see that the transaction reverts

*Instances (21)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

218:         for (uint i = 0; i < complianceMembers.length; i++) {

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

647:         for (uint i = 0; i < proposals.length; i++) {

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

530:         for (uint256 i = 1; i < topTreasuryHolders.length; i++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

671:         for (uint256 i = 1; i < assets.length; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

58:         for (uint256 i = 1; i < assets.length; i++) {

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="NC-36"></a>[NC-36]
 ### Constants should be defined rather than using magic numbers

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

411:         uint votes = Math.min(11, total_votes);

```

</details> 
 


 ### <a name="NC-37"></a>[NC-37]
 ### Strings should use double quotes rather than single quotes

#### Impact:
Using consistent double quotes for strings improves code readability and maintainability. Also see it here https://docs.soliditylang.org/en/v0.8.20/style-guide.html#other-recommendations

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

```

</details> 
 


 ### <a name="NC-38"></a>[NC-38]
 ### Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.  

*Instances (33)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

11:     uint256 private requestId = 0;

13:      uint256 public lastHolderIndex = 0;

213:             for (uint256 j = 0; j < holderBalance; j++) {

246:         uint256 holderCount = 0;

252:             for (uint256 j = 0; j < holderCount; j++) {

266:         for (uint256 k = 0; k < holderCount; k++) {

```

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

217:         uint agreed = 0;

218:         for (uint i = 0; i < complianceMembers.length; i++) {

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

646:         uint ci = 0;

647:         for (uint i = 0; i < proposals.length; i++) {

671:         uint i = 0;

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

440:         uint256 _totalAmount = 0;

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

461:         uint8 sum = 0;

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

528:         uint256 indexToReplace = 0;

582:         for (uint256 i = 0; i < n - 1; i++) {

583:             for (uint256 j = 0; j < n - i - 1; j++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

649:         for (uint256 i = 0; i < totalLocked; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="NC-39"></a>[NC-39]
 ### Consider moving msg.sender checks to modifiers
If some functions are only allowed to be called by some specific users, consider using a modifier instead of checking with a require statement, especially if this check is done in multiple functions.  

*Instances (71)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

30:         require(msg.sender == owner, "caller is not the owner");

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

35:         require(msg.sender == warehouseWallet, "Caller is not the warehouse wallet");

54:         require(msg.sender == routingWallet, "Only the routing wallet can initiate minting");

```

```solidity
File: Governance.sol

141:         require(isComplianceMember(msg.sender));

147:         require(msg.sender == admin, "Callable by admin only");

172:             msg.sender

178:         ) = ITreasury(treasury).getLockedNFTBalances(msg.sender);

190:         p.creator = msg.sender;

208:         uProposals[msg.sender].push(p.PID);

243:         complianerVotes[p.PID][msg.sender] = _supports;

309:                 isComplianceMember(msg.sender);

431:         require(voterVotes[_propID][msg.sender] == 0, "user already voted");

438:         uint votes = getVotingPower(msg.sender);

440:             msg.sender

450:                 voterVotes[_propID][msg.sender] = int256(votes);

453:                 voterVotes[_propID][msg.sender] = -int256(votes);

459:                 msg.sender

468:                 voterVotes[_propID][msg.sender] = int256(votes);

471:                 voterVotes[_propID][msg.sender] = -int256(votes);

475:             p.ptype == ProposalType.COMPLIANCE && isComplianceMember(msg.sender)

479:                 voterVotes[_propID][msg.sender] = 1;

482:                 voterVotes[_propID][msg.sender] = -1;

486:             p.ptype == ProposalType.TREASURY_MAN && isTopTreasurer(msg.sender)

490:                 voterVotes[_propID][msg.sender] = int256(votes);

493:                 voterVotes[_propID][msg.sender] = -int256(votes);

498:         emit Voted(p.PID, _supports, msg.sender);

505:         require(voterVotes[_propID][msg.sender] != 0, "user not voted");

509:         int votes = int256(voterVotes[_propID][msg.sender]);

512:         voterVotes[_propID][msg.sender] = 0;

516:             msg.sender

535:                 msg.sender

548:             p.ptype == ProposalType.COMPLIANCE && isComplianceMember(msg.sender)

557:             p.ptype == ProposalType.TREASURY_MAN && isTopTreasurer(msg.sender)

562:                 msg.sender

590:         require(msg.sender == treasury); //TODO:real address

607:         uint[] memory upids = uProposals[msg.sender];

```

```solidity
File: Treasury.sol

81:     ) Ownable(msg.sender) {

105:         require(msg.sender == governance, "Only owner can call this");

112:             !_isLocked(msg.sender, deedNFT, tokenId),

116:             IERC721(deedNFT).ownerOf(tokenId) == msg.sender,

125:         IERC721(deedNFT).transferFrom(msg.sender, address(this), tokenId); //@audit address(this) is treasury contract but it don't have onERC721Received function

127:         lockedNFTs[msg.sender][deedNFT].push(tokenId);

129:         emit NFTLocked(msg.sender, deedNFT, tokenId);

134:             !_isLocked(msg.sender, founderNFT, tokenId),

138:             IERC721(founderNFT).ownerOf(tokenId) == msg.sender,

148:         IERC721(founderNFT).transferFrom(msg.sender, address(this), tokenId);

149:         lockedNFTs[msg.sender][founderNFT].push(tokenId);

151:         emit NFTLocked(msg.sender, founderNFT, tokenId);

156:             !_isLocked(msg.sender, genesisNFT, tokenId),

160:             IERC721(genesisNFT).ownerOf(tokenId) == msg.sender,

168:         IERC721(genesisNFT).transferFrom(msg.sender, address(this), tokenId);

169:         lockedNFTs[msg.sender][genesisNFT].push(tokenId);

171:         emit NFTLocked(msg.sender, genesisNFT, tokenId);

180:             _isLocked(msg.sender, _nftContract, _tokenId),

184:         _removeLockedNFT(msg.sender, _nftContract, _tokenId);

198:         IERC721(_nftContract).transferFrom(address(this), msg.sender, _tokenId); //@audit safeTransferFrom

199:         emit NFTUnlocked(msg.sender, _nftContract, _tokenId);

272:         bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

323:         uint256 userDepositCount = userDeposits[msg.sender].length;

329:             User storage userDeposit = userDeposits[msg.sender][indices[i]];

396:         IDeedToken(deedToken).mintForTreasury(msg.sender, daiAmountToMint);

401:         treasuryBalances[msg.sender] -= totalAmount;

404:         removeUserDeposits(msg.sender, indices);

407:         updateTopTreasuryHolders(msg.sender);

409:         emit Withdraw(msg.sender, totalAmount, daiAmountToMint);

626:         return userDeposits[msg.sender];

```

```solidity
File: swapp.sol

11:         owner = msg.sender;

15:         require(msg.sender == owner, "Only owner can call this");

74:             recipient: msg.sender, // You may want to send it to the treasury instead

```

</details> 
 


 ### <a name="NC-40"></a>[NC-40]
 ### Dont use _msgSender() if not supporting EIP-2771
Use msg.sender if the code does not implement EIP-2771 trusted forwarder support

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

71:         _burn(_msgSender(), burnAmount);

```

</details> 
 


 ### <a name="NC-41"></a>[NC-41]
 ### Array indices should be referenced via enums rather than numeric literals

#### Impact:
Referencing array indices via enums can improve code readability and maintainability.

*Instances (16)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Treasury.sol

272:         bool success = IERC20(assets[0]).transferFrom(msg.sender,address(this),totalAmount);

276:         console.log(IERC20(assets[0]).balanceOf(address(this)));

282:         uint256 DaiAllocatedFunds = (daiAmountToSwap * assetAllocationRatio[0]) / 100; //@audit what happened to DaiAllocatedFunds

288:         IERC20(assets[0]).transfer(companyWallet, daiAmountToTransfer);

364:             if (assets[i] == assets[0]) {

369:                     assets[0],

386:             AggregatorV3Interface(priceFeed[0])

526:         address smallestHolder = topTreasuryHolders[0];

668:         TransferHelper.safeApprove(assets[0], SWAP_ROUTER_02, daiAmount);

676:                 assets[0],

721:             if (assets[i] == assets[0]) {

726:                     assets[0],

734:             IERC20(assets[0]).transfer(payable(receiver), totalamountinDai);

753:                 AggregatorV3Interface(priceFeed[0])

```

```solidity
File: swapp.sol

87:         if(assets[i]==assets[0]){ 

91:             daiamount+= swapExactInputSingle(assets[i],assets[0],amount,poolFeeTier[i-1]);

```

</details> 
 


 ### <a name="NC-42"></a>[NC-42]
 ### <array>.length should not be looked up in every loop of a for-loop
The overheads outlined below are PER LOOP, excluding the first loop. Storage arrays incur a Gwarmaccess (100 gas), memory arrays use MLOAD (3 gas), calldata arrays use CALLDATALOAD (3 gas). Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset.

*Instances (21)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

218:         for (uint i = 0; i < complianceMembers.length; i++) {

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

647:         for (uint i = 0; i < proposals.length; i++) {

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

530:         for (uint256 i = 1; i < topTreasuryHolders.length; i++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

671:         for (uint256 i = 1; i < assets.length; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

58:         for (uint256 i = 1; i < assets.length; i++) {

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="NC-43"></a>[NC-43]
 ### Use assembly to emit events, in order to save gas
Using the [scratch space](https://github.com/Vectorized/solady/blob/30558f5402f02351b96eeb6eaf32bcea29773841/src/tokens/ERC1155.sol#L501-L504) for event arguments (two words or fewer) will save gas over needing Soliditys full abi memory expansion used for emitting normally.

*Instances (14)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

69:         emit MintInitiated(_to, requestId, _amount);

89:         emit MintCompleted(mintRequest.to, mintRequest.requestId, amount);

226:             emit AllHoldersProcessed(totalHolders);

```

```solidity
File: Governance.sol

300:             emit ProposalTallied(p.PID, false, percent);

312:                 emit ProposalTallied(p.PID, false, percent);

345:         emit ProposalStatusChanged(p.PID, ProposalStatus.COMPLETED);

498:         emit Voted(p.PID, _supports, msg.sender);

```

```solidity
File: Treasury.sol

129:         emit NFTLocked(msg.sender, deedNFT, tokenId);

151:         emit NFTLocked(msg.sender, founderNFT, tokenId);

171:         emit NFTLocked(msg.sender, genesisNFT, tokenId);

199:         emit NFTUnlocked(msg.sender, _nftContract, _tokenId);

314:             emit Deposit(users[i], usdcAmounts[i]);

409:         emit Withdraw(msg.sender, totalAmount, daiAmountToMint);

494:         emit GovernanceUpdated(oldGovernance, _newGovernance);

```

</details> 
 


 ### <a name="NC-44"></a>[NC-44]
 ### Don't initialize variables with default value

*Instances (34)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

11:     uint256 private requestId = 0;

13:      uint256 public lastHolderIndex = 0;

213:             for (uint256 j = 0; j < holderBalance; j++) {

246:         uint256 holderCount = 0;

250:             bool isAlreadyCounted = false;

252:             for (uint256 j = 0; j < holderCount; j++) {

266:         for (uint256 k = 0; k < holderCount; k++) {

```

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

217:         uint agreed = 0;

218:         for (uint i = 0; i < complianceMembers.length; i++) {

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

646:         uint ci = 0;

647:         for (uint i = 0; i < proposals.length; i++) {

671:         uint i = 0;

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

440:         uint256 _totalAmount = 0;

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

461:         uint8 sum = 0;

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

528:         uint256 indexToReplace = 0;

582:         for (uint256 i = 0; i < n - 1; i++) {

583:             for (uint256 j = 0; j < n - i - 1; j++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

649:         for (uint256 i = 0; i < totalLocked; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="NC-45"></a>[NC-45]
 ### Long revert strings

*Instances (11)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

35:         require(msg.sender == warehouseWallet, "Caller is not the warehouse wallet");

42:         require(_warehouseWallet != address(0), "Warehouse wallet address cannot be zero");

43:         require(_routingWallet != address(0), "Routing wallet address cannot be zero");

54:         require(msg.sender == routingWallet, "Only the routing wallet can initiate minting");

155:         require(_warehouseWallet != address(0), "Warehouse wallet address cannot be zero");

160:         require(_routingWallet != address(0), "Routing wallet address cannot be zero");

173:         require(tokenId < _tokenId, "ERC721Metadata: URI query for nonexistent token");

```

```solidity
File: Governance.sol

163:         require(_minPercent > 40, "Min percentage to vote is too low");

351:     require(!release.isStopped, "Funding stopped by compliance team");

```

</details> 
 


## Gas Optimizations


 ### <a name="GAS-1"></a>[GAS-1]
 ### Enable IR-based code generation
By using `--via-ir` or `{"viaIR": true}`, the compiler is able to use more advanced [multi-function optimizations](https://docs.soliditylang.org/en/v0.8.17/ir-breaking-changes.html#solidity-ir-based-codegen-changes), for extra gas savings.

*Instances (8)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: Governance.sol

3: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: Treasury.sol

2: pragma solidity >=0.7.0 <0.9.0;

3: pragma abicoder v2;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

3: pragma abicoder v2;

```

</details> 
 


 ### <a name="GAS-2"></a>[GAS-2]
 ### Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)
While the compiler knows to optimize away the exponentiation, its still better coding practice to use idioms that do not require compiler optimization, if they exist.

*Instances (12)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

8:      uint256 private constant INITIAL_SUPPLY = 88,888,888,888 * 10**DECIMALS; // Supply with 8 decimals

9:      uint256 public constant TOTAL_DEED_ALLOCATION = 500 * 10**6 * 10**DECIMALS;

9:      uint256 public constant TOTAL_DEED_ALLOCATION = 500 * 10**6 * 10**DECIMALS;

16:     uint256 public constant MAX_TREASURY_MINTS = 100,000,000 * 10**DECIMALS;

```

```solidity
File: Governance.sol

63:     uint CREATE_PROPOSAL_COST = 1 * 10 ** 13; // 0.0001 ETH ~ $0.5

414:         uint256 treasuryBal = treasuryDeposit/10**18;

420:         votes = votes ** (1 + genesisNFTCount + founderNFTCount);

```

```solidity
File: Treasury.sol

267:         uint256 amountToMint = (totalAmount * DEED_EMISSION_RATE * 10 ** 8) / (10 ** 18); //@audit-ok yeh puchna ha

267:         uint256 amountToMint = (totalAmount * DEED_EMISSION_RATE * 10 ** 8) / (10 ** 18); //@audit-ok yeh puchna ha

395:             10 ** 8) / (10 ** 18);

395:             10 ** 8) / (10 ** 18);

717:             uint256 splitAmount = ((amount *  10 ** 8 * assetAllocationRatio[i]) / 100);

```

</details> 
 


 ### <a name="GAS-3"></a>[GAS-3]
 ### Nesting if-statements is cheaper than using &&
Nesting if-statements avoids the stack operations of setting up and using an extra jumpdest, and saves 6 [gas](https://gist.github.com/IllIllI000/7f3b818abecfadbef93b894481ae7d19)

*Instances (11)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

234:         return lastHolderIndex == 0 && totalHolders > 0;

```

```solidity
File: Governance.sol

111:             _deedNFT != address(0) &&

112:                 _genesisNFT != address(0) &&

295:         bool criteria = (percent >= p.minPercent) &&

308:             bool compliant = p.yVotes >= complianceVotesNeed &&

434:             (block.timestamp >= p.vote_start) &&

475:             p.ptype == ProposalType.COMPLIANCE && isComplianceMember(msg.sender)

486:             p.ptype == ProposalType.TREASURY_MAN && isTopTreasurer(msg.sender)

510:         require(p.pStatus == ProposalStatus.LIVE && votes != 0);

548:             p.ptype == ProposalType.COMPLIANCE && isComplianceMember(msg.sender)

557:             p.ptype == ProposalType.TREASURY_MAN && isTopTreasurer(msg.sender)

```

</details> 
 


 ### <a name="GAS-4"></a>[GAS-4]
 ### Consider using = instead of += and -= for gas efficiency
Using = instead of += and -= can save gas in certain scenarios. Consider using = when appropriate.

*Instances (38)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

51:         amountMintedToTreasury += amount; 

60:         amountMintedTopreSale += amount;

```

```solidity
File: FounderNFT.sol

57:         requestId += 1;

85:         _tokenId += amount;

```

```solidity
File: Governance.sol

220:                 agreed += 1;

362:     release.releasedAmount += amountToRelease;

449:                 p.yVotes += votes;

452:                 p.nVotes += votes;

455:             p.totalVoted += 1;

467:                 p.yVotes += votes;

470:                 p.nVotes += votes;

473:             p.totalVoted += 1;

478:                 p.yVotes += 1;

481:                 p.nVotes += 1;

484:             p.totalVoted += 1;

489:                 p.yVotes += votes;

492:                 p.nVotes += votes;

495:             p.totalVoted += 1;

525:                 p.yVotes -= uint256(votes);

530:             p.totalVoted -= 1;

541:                 p.yVotes -= uint256(votes);

546:             p.totalVoted -= 1;

551:                 p.yVotes -= 1;

553:                 p.nVotes -= 1;

555:             p.totalVoted -= 1;

569:                 p.yVotes -= r;

571:                 p.nVotes -= r;

573:             p.totalVoted -= 1;

```

```solidity
File: Treasury.sol

318:         totalTreasuryPoints += totalAmount; //@audit You should ensure that treasury points are tracked on a per-user basis and that totalTreasuryPoints reflects a fair distribution

336:             totalAmount += userDeposit.treasuryDeposit; //in 18 decimals

361:             totalAmountInUsd += assetPriceInUsd;

399:         totalTreasuryPoints -= totalAmount;

445:             _totalAmount += usdcAmounts[i];

463:             sum += _ratios[i];

722:                 totalamountinDai += usdInassetprice;

730:                 totalamountinDai += usershareInDai;

```

```solidity
File: swapp.sol

89:           daiamount+=amount;

91:             daiamount+= swapExactInputSingle(assets[i],assets[0],amount,poolFeeTier[i-1]);

```

</details> 
 


 ### <a name="GAS-5"></a>[GAS-5]
 ### Use >= instead of > for gas efficiency
Using >= costs less gas than >. Consider using >= when appropriate.

*Instances (23)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

53:         require(_amount > 0, "Amount must be greater than zero");

199:         require(totalHolders > 0, "No holders to process");

203:         if (endIndex > totalHolders) {

234:         return lastHolderIndex == 0 && totalHolders > 0;

```

```solidity
File: Governance.sol

163:         require(_minPercent > 40, "Min percentage to vote is too low");

226:         return p > 50;

288:         if (p.yVotes > 0) {

319:         if (p.fundingAmount > 0) {

350:     require(release.totalAmount > 0, "Invalid Proposal ID");

356:      if (release.releasedAmount + amountToRelease > release.totalAmount) {

374:     require(release.totalAmount > 0, "Invalid Proposal ID");

415:         if (treasuryBal > 0) {

445:                 deedNFTCount > 0,

463:                 treasuryBal > 0,

514:         bool _supports = votes > 0;

521:                 deedNFTCount > 0,

539:             require(treasuryBal > 0);

695:         require(address(this).balance > 0, "balance should not be 0");

```

```solidity
File: Treasury.sol

324:         require(userDepositCount > 0, "No deposits found for the user");

340:         require(totalAmount > 0, "No valid deposits to withdraw");

420:         for (uint256 i = indices.length; i > 0; i--) {

634:             if (balance > 0) {

710:         require(amount > 0, "amount should be greater than 0");

```

</details> 
 


 ### <a name="GAS-6"></a>[GAS-6]
 ### Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (2)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

78:     mapping(uint => mapping(address => bool)) public complianerVotes;

```

```solidity
File: Treasury.sol

56:     mapping(address => bool) public isTopTreasuryHolder;

```

</details> 
 


 ### <a name="GAS-7"></a>[GAS-7]
 ### Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (23)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

218:         for (uint i = 0; i < complianceMembers.length; i++) {

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

647:         for (uint i = 0; i < proposals.length; i++) {

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

254:         founderNFTCount = lockedNFTs[_user][founderNFT].length;

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

420:         for (uint256 i = indices.length; i > 0; i--) {

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

530:         for (uint256 i = 1; i < topTreasuryHolders.length; i++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

671:         for (uint256 i = 1; i < assets.length; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

58:         for (uint256 i = 1; i < assets.length; i++) {

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="GAS-8"></a>[GAS-8]
 ### Consider using assembly for simple zero checks to save gas
Using assembly for simple zero checks can save gas. Consider using assembly when appropriate.

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

223:         if (agreed == 0) return false;

```

</details> 
 


 ### <a name="GAS-9"></a>[GAS-9]
 ### Expressions for constant values should use immutable rather than constant

#### Impact:
While it doesnt save any gas because the compiler knows that developers often make this mistake, its still best to use the right tool for the task at hand. There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts. constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.  

*Instances (14)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

7:     uint8 private constant DECIMALS = 8;

8:      uint256 private constant INITIAL_SUPPLY = 88,888,888,888 * 10**DECIMALS; // Supply with 8 decimals

9:      uint256 public constant TOTAL_DEED_ALLOCATION = 500 * 10**6 * 10**DECIMALS;

10:     uint256 private constant BURN_PERCENTAGE = 1;

16:     uint256 public constant MAX_TREASURY_MINTS = 100,000,000 * 10**DECIMALS;

```

```solidity
File: FounderNFT.sol

12:     uint256 public constant MAX_SUPPLY = 222000;

```

```solidity
File: Governance.sol

76:     uint private constant PERCENT = 100;

```

```solidity
File: Treasury.sol

20:     uint256 public constant FULL_TERM_LOCK_PERIOD = 2 * 6 * 30 * 24 * 60 * 60; //@audit-info 360 days

21:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

```

```solidity
File: swapp.sol

30:     address public constant DAI = 0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063;

31:     address public constant USDC = 0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359;

32:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

33:     address public constant WBTC = 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6;

34:     address public constant USDT = 0xc2132D05D31c914a87C6611C10748AEb04B58e8F;

```

</details> 
 


 ### <a name="GAS-10"></a>[GAS-10]
 ### Constructors can be marked payable
Payable functions cost less gas to execute, since the compiler does not have to add extra checks to ensure that a payment wasn  t provided. A constructor can safely be marked as payable, since only the deployer would be able to pass funds, and the project itself would not pass any funds.

*Instances (5)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

18:     constructor(address _owner) ERC20("Deed Token test", "DEED") {

```

```solidity
File: FounderNFT.sol

39:     constructor(address _warehouseWallet, address _routingWallet, string memory _baseTokenURI)

```

```solidity
File: Governance.sol

103:     constructor(

```

```solidity
File: Treasury.sol

68:     constructor(

```

```solidity
File: swapp.sol

10:     constructor() {

```

</details> 
 


 ### <a name="GAS-11"></a>[GAS-11]
 ### Use Custom Errors
[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

*Instances (62)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

30:         require(msg.sender == owner, "caller is not the owner");

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

49:         require(amountMintedToTreasury + amount <= MAX_TREASURY_MINTS,"you cannot mint it");

58:         require(amountMintedTopreSale + amount <= TOTAL_DEED_ALLOCATION ,"you cannot mint it");

```

```solidity
File: FounderNFT.sol

35:         require(msg.sender == warehouseWallet, "Caller is not the warehouse wallet");

42:         require(_warehouseWallet != address(0), "Warehouse wallet address cannot be zero");

43:         require(_routingWallet != address(0), "Routing wallet address cannot be zero");

52:         require(_to != address(0), "Invalid address");

53:         require(_amount > 0, "Amount must be greater than zero");

54:         require(msg.sender == routingWallet, "Only the routing wallet can initiate minting");

55:         require(_tokenId < MAX_SUPPLY, "Max supply reached");

75:         require(index <= pendingMints.length, "Invalid mint request index");

77:         require(!mintRequest.completed, "Mint request already completed");

155:         require(_warehouseWallet != address(0), "Warehouse wallet address cannot be zero");

160:         require(_routingWallet != address(0), "Routing wallet address cannot be zero");

173:         require(tokenId < _tokenId, "ERC721Metadata: URI query for nonexistent token");

189:         require(_requestId <= requestId,"invalid requestId");

199:         require(totalHolders > 0, "No holders to process");

211:             require(_tokenId + holderBalance <= MAX_SUPPLY, "Max supply will be exceeded");

```

```solidity
File: Governance.sol

116:         require(_admin != address(0), "Zero Address");

147:         require(msg.sender == admin, "Callable by admin only");

163:         require(_minPercent > 40, "Min percentage to vote is too low");

184:         require(amount <= maxFundingAmount, "Funding limit exceeded");

234:         require(_propID < proposals.length, "Invalid ID");

261:         require(_propID < proposals.length, "Invalid ID");

282:         require(_pId < proposals.length, "Invalid ID");

350:     require(release.totalAmount > 0, "Invalid Proposal ID");

351:     require(!release.isStopped, "Funding stopped by compliance team");

352:    require(block.timestamp >= release.lastReleaseTime + 30 days, "Monthly release not due yet");

353:     require(release.releaseCount < 10, "All funds have been released");

374:     require(release.totalAmount > 0, "Invalid Proposal ID");

429:         require(_propID < proposals.length, "Invalid ID");

431:         require(voterVotes[_propID][msg.sender] == 0, "user already voted");

432:         require(p.pStatus == ProposalStatus.LIVE, "status not live");

505:         require(voterVotes[_propID][msg.sender] != 0, "user not voted");

506:         require(_propID < proposals.length, "Invalid ID");

596:         require(pID < proposals.length, "Invalid ID");

617:         require(_pid < proposals.length, "Invalid ID");

623:         require(_pid < proposals.length, "Invalid ID");

630:         require(_pid < proposals.length, "Invalid ID");

662:         require(_treasury != address(0), "Zero Address");

695:         require(address(this).balance > 0, "balance should not be 0");

```

```solidity
File: PriceConvertor.sol

25:             revert("Unsupported decimals");

44:             revert("Unsupported decimals");

```

```solidity
File: Treasury.sol

105:         require(msg.sender == governance, "Only owner can call this");

262:         require(users.length == usdcAmounts.length, "Length mismatch");

275:         require(success, "Transfer to treasury failed");

324:         require(userDepositCount > 0, "No deposits found for the user");

328:             require(indices[i] < userDepositCount, "Invalid index");

340:         require(totalAmount > 0, "No valid deposits to withdraw");

422:             require(index <= lastIndex, "Index out of bounds");

442:             require(usdcAmounts[i] > 0, "Zero Amount");

443:             require(users[i] != address(0), "Invalid User");

465:         require(sum == 100, "Ratios should equal 100");

487:         require(_newGovernance != address(0), "Zero Address");

545:         require(isTopTreasuryHolder[user], "User not in top holders");

617:         require(assetIndex < assets.length, "Invalid asset index");

710:         require(amount > 0, "amount should be greater than 0");

711:         require(receiver != address(0), "should be valid address");

742:         require(assetIndex < assets.length, "Invalid asset index");

```

```solidity
File: swapp.sol

15:         require(msg.sender == owner, "Only owner can call this");

```

</details> 
 


 ### <a name="GAS-12"></a>[GAS-12]
 ### Reduce gas usage by moving to Solidity 0.8.19 or later
Solidity version 0.8.19 introduced a number of gas optimizations, refer to the [Solidity 0.8.19 Release Announcement](https://soliditylang.org/blog/2023/02/22/solidity-0.8.19-release-announcement) for details.

*Instances (4)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: FounderNFT.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: PriceConvertor.sol

2: pragma solidity ^0.8.19;

```

```solidity
File: swapp.sol

2: pragma solidity >=0.7.5;

```

</details> 
 


 ### <a name="GAS-13"></a>[GAS-13]
 ### Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (22)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

48:     function mintForTreasury(address account, uint256 amount) external onlyTreasuryContract {

57:     function mintForPreSale(address account, uint256 amount) external onlypreSaleContract {

```

```solidity
File: FounderNFT.sol

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

194:     function mintBatchTokensForHolders(uint256 batchSize) external onlyWarehouseWallet {

```

```solidity
File: Governance.sol

661:     function setTreasury(address _treasury) external onlyAdmin {

666:     function addComplianer(address _complianer) public onlyAdmin {

670:     function removeComplianer(address _complianer) public onlyAdmin {

690:     function revokeAdmin(address _newAdmin) public onlyAdmin {

694:     function withdrawCreateProposalFee() external onlyAdmin {

```

```solidity
File: Treasury.sol

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

629:     function drainTreasury() external onlyOwner {

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

```

```solidity
File: swapp.sol

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

```

</details> 
 


 ### <a name="GAS-14"></a>[GAS-14]
 ### `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
*Saves 5 gas per loop*

*Instances (38)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

81:         for (uint256 i = _tokenId; i < (_tokenId + amount); i++) {

208:         for (uint256 i = lastHolderIndex; i < endIndex; i++) {

213:             for (uint256 j = 0; j < holderBalance; j++) {

215:                 holderTokenCount[holder]++;

216:                 _tokenId++;

248:         for (uint256 i = 1; i <= totalTokens; i++) {

252:             for (uint256 j = 0; j < holderCount; j++) {

261:                 holderCount++;

266:         for (uint256 k = 0; k < holderCount; k++) {

```

```solidity
File: Governance.sol

130:         for (uint i = 0; i < complianceMembers.length; i++) {

204:             pendingProposals++;

218:         for (uint i = 0; i < complianceMembers.length; i++) {

364:     release.releaseCount++;

391:         for (uint i = 0; i < topTreasuryHolders.length; i++) {

609:         for (uint i = 0; i < upids.length; i++) {

647:         for (uint i = 0; i < proposals.length; i++) {

650:                 ci++;

675:         for (; i < len; i++) {

683:             for (uint j = i; j < len - 1; j++) {

```

```solidity
File: Treasury.sol

189:         for (uint256 i = 0; i < tokens.length; i++) {

210:         for (uint256 i = 0; i < lockedTokens.length; i++) {

225:         for (uint256 i = 0; i < lockedTokens.length; i++) {

297:         for (uint256 i = 0; i < users.length; i++) {

327:         for (uint256 i = 0; i < indices.length; i++) {

344:         for (uint256 i = 0; i < assets.length; i++) {

441:         for (uint256 i = 0; i < usdcAmounts.length; i++) {

462:         for (uint8 i = 0; i < _ratios.length; i++) {

470:         for (uint256 i = 0; i < assets.length; i++) {

480:         for (uint256 i = 0; i < assets.length; i++) {

530:         for (uint256 i = 1; i < topTreasuryHolders.length; i++) {

582:         for (uint256 i = 0; i < n - 1; i++) {

583:             for (uint256 j = 0; j < n - i - 1; j++) {

631:         for (uint256 i = 0; i < assets.length; i++) {

649:         for (uint256 i = 0; i < totalLocked; i++) {

671:         for (uint256 i = 1; i < assets.length; i++) {

714:         for (uint256 i = 0; i < assets.length; i++) {

```

```solidity
File: swapp.sol

58:         for (uint256 i = 1; i < assets.length; i++) {

85:         for(uint i = 0; i<assets.length;i++){

```

</details> 
 


 ### <a name="GAS-15"></a>[GAS-15]
 ### Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (10)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

9:      uint256 public constant TOTAL_DEED_ALLOCATION = 500 * 10**6 * 10**DECIMALS;

16:     uint256 public constant MAX_TREASURY_MINTS = 100,000,000 * 10**DECIMALS;

```

```solidity
File: FounderNFT.sol

12:     uint256 public constant MAX_SUPPLY = 222000;

```

```solidity
File: Treasury.sol

20:     uint256 public constant FULL_TERM_LOCK_PERIOD = 2 * 6 * 30 * 24 * 60 * 60; //@audit-info 360 days

21:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

```

```solidity
File: swapp.sol

30:     address public constant DAI = 0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063;

31:     address public constant USDC = 0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359;

32:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

33:     address public constant WBTC = 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6;

34:     address public constant USDT = 0xc2132D05D31c914a87C6611C10748AEb04B58e8F;

```

</details> 
 


 ### <a name="GAS-16"></a>[GAS-16]
 ### require()/revert() strings longer than 32 bytes cost extra gas
Each extra memory word of bytes past the original 32 [incurs an MSTORE](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas.

*Instances (30)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

25:         require(msg.sender == treasuryAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

35:         require(msg.sender == preSaleAddress, "Caller is not the treasury contract"); //@audit where do we set the treasury address?

```

```solidity
File: FounderNFT.sol

35:         require(msg.sender == warehouseWallet, "Caller is not the warehouse wallet");

53:         require(_amount > 0, "Amount must be greater than zero");

55:         require(_tokenId < MAX_SUPPLY, "Max supply reached");

77:         require(!mintRequest.completed, "Mint request already completed");

173:         require(tokenId < _tokenId, "ERC721Metadata: URI query for nonexistent token");

189:         require(_requestId <= requestId,"invalid requestId");

```

```solidity
File: Governance.sol

147:         require(msg.sender == admin, "Callable by admin only");

165:         require(

238:         require(

265:         require(

351:     require(!release.isStopped, "Funding stopped by compliance team");

353:     require(release.releaseCount < 10, "All funds have been released");

429:         require(_propID < proposals.length, "Invalid ID");

432:         require(p.pStatus == ProposalStatus.LIVE, "status not live");

444:             require(

505:         require(voterVotes[_propID][msg.sender] != 0, "user not voted");

520:             require(

617:         require(_pid < proposals.length, "Invalid ID");

630:         require(_pid < proposals.length, "Invalid ID");

```

```solidity
File: PriceConvertor.sol

25:             revert("Unsupported decimals");

```

```solidity
File: Treasury.sol

82:         require(

105:         require(msg.sender == governance, "Only owner can call this");

262:         require(users.length == usdcAmounts.length, "Length mismatch");

324:         require(userDepositCount > 0, "No deposits found for the user");

330:             require(

422:             require(index <= lastIndex, "Index out of bounds");

545:         require(isTopTreasuryHolder[user], "User not in top holders");

710:         require(amount > 0, "amount should be greater than 0");

```

</details> 
 


 ### <a name="GAS-17"></a>[GAS-17]
 ### Use shift Right/Left instead of division/multiplication if possible

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

306:             uint256 complianceVotesNeed = (totalComplianceMembers / 2) + 1;

```

</details> 
 


 ### <a name="GAS-18"></a>[GAS-18]
 ### Splitting require() statements that use && saves gas

*Instances (1)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Governance.sol

510:         require(p.pStatus == ProposalStatus.LIVE && votes != 0);

```

</details> 
 


 ### <a name="GAS-19"></a>[GAS-19]
 ### Structs can be packed into fewer storage slots
Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

*Instances (4)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

18:     struct MintRequest {

```

```solidity
File: Governance.sol

30:     struct Proposal {

47:  struct FundRelease {

```

```solidity
File: Treasury.sol

42:     struct User {

```

</details> 
 


 ### <a name="GAS-20"></a>[GAS-20]
 ### Consider using uint256(1)/uint256(2) instead of true/false for gas efficiency
Using uint256(1) and uint256(2) instead of true and false can save gas for certain changes. Consider using uint256(1)/uint256(2) when appropriate.

*Instances (20)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

63:             completed: false

87:         mintRequest.completed = true;

250:             bool isAlreadyCounted = false;

254:                     isAlreadyCounted = true;

```

```solidity
File: Governance.sol

129:         bool _isComplianer = false;

132:                 _isComplianer = true;

223:         if (agreed == 0) return false;

257:         _success = true;

300:             emit ProposalTallied(p.PID, false, percent);

312:                 emit ProposalTallied(p.PID, false, percent);

340:              isStopped: false

375:     release.isStopped = true;

393:                 return true;

397:         return false;

500:         return true;

576:         return true;

```

```solidity
File: Treasury.sol

212:                 return true;

215:         return false;

509:                     isTopTreasuryHolder[user] = true;

555:         isTopTreasuryHolder[user] = false;

```

</details> 
 


 ### <a name="GAS-21"></a>[GAS-21]
 ### Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
Using uints or ints smaller than 32 bytes (256 bits) can incur overhead. Consider using uint256 or int256 to avoid potential issues.

*Instances (19)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

7:     uint8 private constant DECIMALS = 8;

63:     function decimals() public view virtual override returns (uint8) {

```

```solidity
File: PriceConvertor.sol

15:         uint8 decimals

34:         uint8 decimals

```

```solidity
File: Treasury.sol

30:     uint8[] public assetAllocationRatio;

34:     uint24[] poolFeeTier = [100, 100, 3000, 100];

71:         uint8[] memory _assetAllocations,

452:         uint8[] memory _allocationRatios    

459:     function calculateSumOfRatios(uint8[] memory _ratios) public pure {

461:         uint8 sum = 0;

462:         for (uint8 i = 0; i < _ratios.length; i++) {

621:         uint8 decimals = uint8(assetsdecimals[assetIndex]);

621:         uint8 decimals = uint8(assetsdecimals[assetIndex]);

688:         uint24 feeTier

761:             uint8 decimal = uint8(assetsdecimals[assetIndex]);

761:             uint8 decimal = uint8(assetsdecimals[assetIndex]);

```

```solidity
File: swapp.sol

38:     uint24[] assetRatios = [20, 20, 20, 20, 20];

39:     uint24[] poolFeeTier = [100, 100, 3000, 100];

66:     function swapExactInputSingle(address tokenIn, address tokenOut, uint256 amountIn, uint24 feeTier)

```

</details> 
 


 ### <a name="GAS-22"></a>[GAS-22]
 ### Use != 0 instead of > for unsigned integer comparison

*Instances (23)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: FounderNFT.sol

53:         require(_amount > 0, "Amount must be greater than zero");

199:         require(totalHolders > 0, "No holders to process");

203:         if (endIndex > totalHolders) {

234:         return lastHolderIndex == 0 && totalHolders > 0;

```

```solidity
File: Governance.sol

163:         require(_minPercent > 40, "Min percentage to vote is too low");

226:         return p > 50;

288:         if (p.yVotes > 0) {

319:         if (p.fundingAmount > 0) {

350:     require(release.totalAmount > 0, "Invalid Proposal ID");

356:      if (release.releasedAmount + amountToRelease > release.totalAmount) {

374:     require(release.totalAmount > 0, "Invalid Proposal ID");

415:         if (treasuryBal > 0) {

445:                 deedNFTCount > 0,

463:                 treasuryBal > 0,

514:         bool _supports = votes > 0;

521:                 deedNFTCount > 0,

539:             require(treasuryBal > 0);

695:         require(address(this).balance > 0, "balance should not be 0");

```

```solidity
File: Treasury.sol

324:         require(userDepositCount > 0, "No deposits found for the user");

340:         require(totalAmount > 0, "No valid deposits to withdraw");

420:         for (uint256 i = indices.length; i > 0; i--) {

634:             if (balance > 0) {

710:         require(amount > 0, "amount should be greater than 0");

```

</details> 
 


 ### <a name="GAS-23"></a>[GAS-23]
 ### Optimize names to save gas
public/external function names and public member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

*Instances (115)*:
 
 <details>
 <summary>Click to expand!</summary>

```solidity
File: Deed.sol

9:      uint256 public constant TOTAL_DEED_ALLOCATION = 500 * 10**6 * 10**DECIMALS;

11:     address public treasuryAddress;

12:     address public preSaleAddress;

16:     uint256 public constant MAX_TREASURY_MINTS = 100,000,000 * 10**DECIMALS;

39:     function setTreasuryAddress(address _treasuryAddress) external onlyOwner {

43:     function setpreSaleAddress(address _preSaleAddress) external onlyOwner {

48:     function mintForTreasury(address account, uint256 amount) external onlyTreasuryContract {

57:     function mintForPreSale(address account, uint256 amount) external onlypreSaleContract {

63:     function decimals() public view virtual override returns (uint8) {

67:     function transfer(address recipient, uint256 amount) public virtual override returns (bool) {

75:     function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {

```

```solidity
File: FounderNFT.sol

12:     uint256 public constant MAX_SUPPLY = 222000;

13:      uint256 public lastHolderIndex = 0;

14:     address public warehouseWallet;

15:     address public routingWallet;

25:     MintRequest[] public pendingMints;

73:     function completeMint(uint256 _requestId) external onlyWarehouseWallet {

154:     function setWarehouseWallet(address _warehouseWallet) external onlyWarehouseWallet {

159:     function setRoutingWallet(address _routingWallet) external onlyWarehouseWallet {

164:     function setBaseTokenURI(string memory _baseTokenURI) external onlyWarehouseWallet {

172:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

177:     function totalSupply() external view returns (uint256) {

181:     function totalrequestId() external view returns (uint256){

184:      function getPendingMints() external view returns (MintRequest[] memory) {

194:     function mintBatchTokensForHolders(uint256 batchSize) external onlyWarehouseWallet {

231:     function hasAllHoldersBeenProcessed() external view returns (bool) {

274: function getRemainingHolders() external view returns (uint256 remainingHolders) {

```

```solidity
File: Governance.sol

59: mapping(uint => FundRelease) public fundReleases;

66:     Proposal[] public proposals;

69:     address public treasury;

70:     address public admin;

72:     uint public pendingProposals;

74:     mapping(address => uint[]) public uProposals;

78:     mapping(uint => mapping(address => bool)) public complianerVotes;

81:     mapping(uint => mapping(address => int)) public voterVotes;

98:     IERC721 public deedNFT;

99:     IERC721 public genesisNFT;

100:     IERC721 public founderNFT;

101:     address[] public complianceMembers;

128:     function isComplianceMember(address s) public view returns (bool) {

162:     ) external payable {

233:     ) public OnlyComplianceMember returns (bool _success) {

260:     function makeItLive(uint _propID) public OnlyComplianceMember {

281:     function executeProposal(uint _pId) external nonReentrant {

372: function stopFunding(uint _proposalId) external OnlyComplianceMember {

401:     function getVotingPower(address voter) public view returns (uint) {

581:         public

595:     function getProposal(uint pID) public view returns (Proposal memory) {

601:     function numOfProposals() external view returns (uint) {

606:     function getCreatorProposals() external view returns (Proposal[] memory) {

616:     function getProposalYayVotes(uint _pid) external view returns (uint) {

622:     function getProposalNayVotes(uint _pid) external view returns (uint) {

629:     ) external view returns (ProposalStatus) {

634:     function getAllProposals() public view returns (Proposal[] memory) {

640:         public

657:     function getComplianers() public view returns (address[] memory) {

661:     function setTreasury(address _treasury) external onlyAdmin {

666:     function addComplianer(address _complianer) public onlyAdmin {

670:     function removeComplianer(address _complianer) public onlyAdmin {

690:     function revokeAdmin(address _newAdmin) public onlyAdmin {

694:     function withdrawCreateProposalFee() external onlyAdmin {

```

```solidity
File: PriceConvertor.sol

6:     function getPrice(AggregatorV3Interface priceFeed) public view returns (uint256) {

16:     ) public view returns (uint256) {

35:     ) public view returns (uint256) {

56:     ) public view returns (uint256) {

67: ) public view returns (uint256) {

82: ) public view returns (uint256) {

```

```solidity
File: Treasury.sol

20:     uint256 public constant FULL_TERM_LOCK_PERIOD = 2 * 6 * 30 * 24 * 60 * 60; //@audit-info 360 days

21:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

22:     address public SWAP_ROUTER_02 = 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45;

24:     uint256 public DEED_EMISSION_RATE = 50;

26:     address public governance;

27:     address[] public assets;

28:     uint256[] public assetsdecimals;

29:     address[] public priceFeed;

30:     uint8[] public assetAllocationRatio;

36:     uint256 public totalTreasuryPoints;

38:     address[] public topTreasuryHolders;

39:     mapping(address => uint256) public treasuryHolderIndex;

40:     mapping(address => uint256) public treasuryBalances;

47:     mapping(address => User[]) public userDeposits; // in dollars User[]

50:     address public deedNFT;

51:     address public founderNFT;

52:     address public genesisNFT;

54:     mapping(address => mapping(address => uint256[])) public lockedNFTs;

55:     mapping(address => mapping(address => uint256[])) public contractlockedNFTs;

56:     mapping(address => bool) public isTopTreasuryHolder;

178:     ) external nonReentrant {

238:     ) external view returns (uint256[] memory) {

245:         external

261:     ) external onlyOwner {

264:         uint256 totalAmount = calculateTotalDeposit(users, usdcAmounts); //@audit this function is just calculation the deposit

321:     function withdraw(uint256[] memory indices) external nonReentrant {

432:     function setDeedEmissionRate(uint256 _deedEmissionRate) external onlyOwner {

453:     ) public onlyOwner {

459:     function calculateSumOfRatios(uint8[] memory _ratios) public pure {

469:     function approveGovernance() public onlyOwner {

479:     function revokeGovernance() public onlyOwner {

486:     function updateGovernance(address _newGovernance) external onlyOwner {

605:     ) public view returns (uint256 treasuryBal) {

609:     function getTopTreasuryHolders() public view returns (address[] memory) {

616:     ) public view returns (uint256) {

625:     function getUserDeposits() external view returns (User[] memory) {

629:     function drainTreasury() external onlyOwner {

662:     function approveAddress(address _token, uint _amount) public onlyOwner {

709:     ) external onlyGovernanace {

```

```solidity
File: swapp.sol

8:     address public owner;

20:     address public SWAP_ROUTER_02 = 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45;

30:     address public constant DAI = 0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063;

31:     address public constant USDC = 0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359;

32:     address public constant WETH = 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619;

33:     address public constant WBTC = 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6;

34:     address public constant USDT = 0xc2132D05D31c914a87C6611C10748AEb04B58e8F;

45:     function withdraw(address _token, uint amount) public onlyOwner {

49:     function approveAddress(address _token, uint _amount) public onlyOwner {

```

</details> 
 

