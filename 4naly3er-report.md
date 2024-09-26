# Report

- [Report](#report)
  - [Gas Optimizations](#gas-optimizations)
    - [\[GAS-1\] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)](#gas-1-a--a--b-is-more-gas-effective-than-a--b-for-state-variables-excluding-arrays-and-mappings)
    - [\[GAS-2\] Use assembly to check for `address(0)`](#gas-2-use-assembly-to-check-for-address0)
    - [\[GAS-3\] Using bools for storage incurs overhead](#gas-3-using-bools-for-storage-incurs-overhead)
    - [\[GAS-4\] Cache array length outside of loop](#gas-4-cache-array-length-outside-of-loop)
    - [\[GAS-5\] State variables should be cached in stack variables rather than re-reading them from storage](#gas-5-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage)
    - [\[GAS-6\] Use calldata instead of memory for function arguments that do not get mutated](#gas-6-use-calldata-instead-of-memory-for-function-arguments-that-do-not-get-mutated)
    - [\[GAS-7\] For Operations that will not overflow, you could use unchecked](#gas-7-for-operations-that-will-not-overflow-you-could-use-unchecked)
    - [\[GAS-8\] Avoid contract existence checks by using low level calls](#gas-8-avoid-contract-existence-checks-by-using-low-level-calls)
    - [\[GAS-9\] State variables only set in the constructor should be declared `immutable`](#gas-9-state-variables-only-set-in-the-constructor-should-be-declared-immutable)
    - [\[GAS-10\] Functions guaranteed to revert when called by normal users can be marked `payable`](#gas-10-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable)
    - [\[GAS-11\] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)](#gas-11-i-costs-less-gas-compared-to-i-or-i--1-same-for---i-vs-i---or-i---1)
    - [\[GAS-12\] Use shift right/left instead of division/multiplication if possible](#gas-12-use-shift-rightleft-instead-of-divisionmultiplication-if-possible)
    - [\[GAS-13\] Increments/decrements can be unchecked in for-loops](#gas-13-incrementsdecrements-can-be-unchecked-in-for-loops)
    - [\[GAS-14\] Use != 0 instead of \> 0 for unsigned integer comparison](#gas-14-use--0-instead-of--0-for-unsigned-integer-comparison)
    - [\[GAS-15\] `internal` functions not called by the contract should be removed](#gas-15-internal-functions-not-called-by-the-contract-should-be-removed)
    - [\[GAS-16\] WETH address definition can be use directly](#gas-16-weth-address-definition-can-be-use-directly)
  - [Non Critical Issues](#non-critical-issues)
    - [\[NC-1\] Missing checks for `address(0)` when assigning values to address state variables](#nc-1-missing-checks-for-address0-when-assigning-values-to-address-state-variables)
    - [\[NC-2\] Array indices should be referenced via `enum`s rather than via numeric literals](#nc-2-array-indices-should-be-referenced-via-enums-rather-than-via-numeric-literals)
    - [\[NC-3\] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`](#nc-3-use-stringconcat-or-bytesconcat-instead-of-abiencodepacked)
    - [\[NC-4\] `constant`s should be defined rather than using magic numbers](#nc-4-constants-should-be-defined-rather-than-using-magic-numbers)
    - [\[NC-5\] Control structures do not follow the Solidity Style Guide](#nc-5-control-structures-do-not-follow-the-solidity-style-guide)
    - [\[NC-6\] Unused `error` definition](#nc-6-unused-error-definition)
    - [\[NC-7\] Events that mark critical parameter changes should contain both the old and the new value](#nc-7-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value)
    - [\[NC-8\] Function ordering does not follow the Solidity style guide](#nc-8-function-ordering-does-not-follow-the-solidity-style-guide)
    - [\[NC-9\] Functions should not be longer than 50 lines](#nc-9-functions-should-not-be-longer-than-50-lines)
    - [\[NC-10\] Change int to int256](#nc-10-change-int-to-int256)
    - [\[NC-11\] Lack of checks in setters](#nc-11-lack-of-checks-in-setters)
    - [\[NC-12\] Lines are too long](#nc-12-lines-are-too-long)
    - [\[NC-13\] `type(uint256).max` should be used instead of `2 ** 256 - 1`](#nc-13-typeuint256max-should-be-used-instead-of-2--256---1)
    - [\[NC-14\] Missing Event for critical parameters change](#nc-14-missing-event-for-critical-parameters-change)
    - [\[NC-15\] Incomplete NatSpec: `@param` is missing on actually documented functions](#nc-15-incomplete-natspec-param-is-missing-on-actually-documented-functions)
    - [\[NC-16\] Incomplete NatSpec: `@return` is missing on actually documented functions](#nc-16-incomplete-natspec-return-is-missing-on-actually-documented-functions)
    - [\[NC-17\] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor](#nc-17-use-a-modifier-instead-of-a-requireif-statement-for-a-special-msgsender-actor)
    - [\[NC-18\] Constant state variables defined more than once](#nc-18-constant-state-variables-defined-more-than-once)
    - [\[NC-19\] Adding a `return` statement when the function defines a named return variable, is redundant](#nc-19-adding-a-return-statement-when-the-function-defines-a-named-return-variable-is-redundant)
    - [\[NC-20\] `require()` / `revert()` statements should have descriptive reason strings](#nc-20-requirerevertstatements-should-have-descriptive-reason-strings)
    - [\[NC-21\] Take advantage of Custom Error's return value property](#nc-21-take-advantage-of-custom-errors-return-value-property)
    - [\[NC-22\] Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)](#nc-22-use-scientific-notation-eg-1e18-rather-than-exponentiation-eg-1018)
    - [\[NC-23\] Strings should use double quotes rather than single quotes](#nc-23-strings-should-use-double-quotes-rather-than-single-quotes)
    - [\[NC-24\] Contract does not follow the Solidity style guide's suggested layout ordering](#nc-24-contract-does-not-follow-the-solidity-style-guides-suggested-layout-ordering)
    - [\[NC-25\] Use Underscores for Number Literals (add an underscore every 3 digits)](#nc-25-use-underscores-for-number-literals-add-an-underscore-every-3-digits)
    - [\[NC-26\] Internal and private variables and functions names should begin with an underscore](#nc-26-internal-and-private-variables-and-functions-names-should-begin-with-an-underscore)
    - [\[NC-27\] Event is missing `indexed` fields](#nc-27-event-is-missing-indexed-fields)
    - [\[NC-28\] Constants should be defined rather than using magic numbers](#nc-28-constants-should-be-defined-rather-than-using-magic-numbers)
    - [\[NC-29\] `public` functions not called by the contract should be declared `external` instead](#nc-29-public-functions-not-called-by-the-contract-should-be-declared-external-instead)
    - [\[NC-30\] Variables need not be initialized to zero](#nc-30-variables-need-not-be-initialized-to-zero)
  - [Low Issues](#low-issues)
    - [\[L-1\] `approve()`/`safeApprove()` may revert if the current approval is not zero](#l-1-approvesafeapprove-may-revert-if-the-current-approval-is-not-zero)
    - [\[L-2\] Some tokens may revert when zero value transfers are made](#l-2-some-tokens-may-revert-when-zero-value-transfers-are-made)
    - [\[L-3\] Missing checks for `address(0)` when assigning values to address state variables](#l-3-missing-checks-for-address0-when-assigning-values-to-address-state-variables)
    - [\[L-4\] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`](#l-4-abiencodepacked-should-not-be-used-with-dynamic-types-when-passing-the-result-to-a-hash-function-such-as-keccak256)
    - [\[L-5\] `decimals()` is not a part of the ERC-20 standard](#l-5-decimals-is-not-a-part-of-the-erc-20-standard)
    - [\[L-6\] Deprecated approve() function](#l-6-deprecated-approve-function)
    - [\[L-7\] Division by zero not prevented](#l-7-division-by-zero-not-prevented)
    - [\[L-8\] External calls in an un-bounded `for-`loop may result in a DOS](#l-8-external-calls-in-an-un-bounded-for-loop-may-result-in-a-dos)
    - [\[L-9\] Prevent accidentally burning tokens](#l-9-prevent-accidentally-burning-tokens)
    - [\[L-10\] NFT ownership doesn't support hard forks](#l-10-nft-ownership-doesnt-support-hard-forks)
    - [\[L-11\] Possible rounding issue](#l-11-possible-rounding-issue)
    - [\[L-12\] Loss of precision](#l-12-loss-of-precision)
    - [\[L-13\] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`](#l-13-solidity-version-0820-may-not-work-on-other-chains-due-to-push0)
    - [\[L-14\] `symbol()` is not a part of the ERC-20 standard](#l-14-symbol-is-not-a-part-of-the-erc-20-standard)
    - [\[L-15\] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting](#l-15-consider-using-openzeppelins-safecast-library-to-prevent-unexpected-overflows-when-downcasting)
    - [\[L-16\] Unsafe ERC20 operation(s)](#l-16-unsafe-erc20-operations)
    - [\[L-17\] Upgradeable contract not initialized](#l-17-upgradeable-contract-not-initialized)
  - [Medium Issues](#medium-issues)
    - [\[M-1\] `_safeMint()` should be used rather than `_mint()` wherever possible](#m-1-_safemint-should-be-used-rather-than-_mint-wherever-possible)
    - [\[M-2\] Library function isn't `internal` or `private`](#m-2-library-function-isnt-internal-or-private)
    - [\[M-3\] Return values of `transfer()`/`transferFrom()` not checked](#m-3-return-values-of-transfertransferfrom-not-checked)
    - [\[M-4\] Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20`](#m-4-unsafe-use-of-transfertransferfromapprove-with-ierc20)
  - [High Issues](#high-issues)
    - [\[H-1\] IERC20.approve() will revert for USDT](#h-1-ierc20approve-will-revert-for-usdt)

## Gas Optimizations

| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 40 |
| [GAS-2](#GAS-2) | Use assembly to check for `address(0)` | 6 |
| [GAS-3](#GAS-3) | Using bools for storage incurs overhead | 14 |
| [GAS-4](#GAS-4) | Cache array length outside of loop | 10 |
| [GAS-5](#GAS-5) | State variables should be cached in stack variables rather than re-reading them from storage | 3 |
| [GAS-6](#GAS-6) | Use calldata instead of memory for function arguments that do not get mutated | 7 |
| [GAS-7](#GAS-7) | For Operations that will not overflow, you could use unchecked | 763 |
| [GAS-8](#GAS-8) | Avoid contract existence checks by using low level calls | 5 |
| [GAS-9](#GAS-9) | State variables only set in the constructor should be declared `immutable` | 14 |
| [GAS-10](#GAS-10) | Functions guaranteed to revert when called by normal users can be marked `payable` | 3 |
| [GAS-11](#GAS-11) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 35 |
| [GAS-12](#GAS-12) | Use shift right/left instead of division/multiplication if possible | 30 |
| [GAS-13](#GAS-13) | Increments/decrements can be unchecked in for-loops | 19 |
| [GAS-14](#GAS-14) | Use != 0 instead of > 0 for unsigned integer comparison | 29 |
| [GAS-15](#GAS-15) | `internal` functions not called by the contract should be removed | 107 |
| [GAS-16](#GAS-16) | WETH address definition can be use directly | 2 |

### <a name="GAS-1"></a>[GAS-1] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)

This saves **16 gas per instance.**

*Instances (40)*:

```solidity
File: contracts/CollateralTracker.sol

420:         s_poolAssets += uint128(assets);

476:         s_poolAssets += uint128(assets);

875:         balanceOf[delegatee] += type(uint248).max;

907:             s_poolAssets += uint128(bonusAbs);

914:                     totalSupply += type(uint248).max - liquidateeBalance;

926:                     totalSupply += type(uint248).max - liquidateeBalance;

1118:             exchangedAmount += int256(

1199:                 tokenRequired += _tokenRequired;

1229:                 tokenRequired += _getRequiredCollateralSingleLeg(

1364:                         required += Math.mulDiv96RoundingUp(amountMoved, c2);

1380:                         required += c3;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

302:                 salt += 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

1187:                 solvent += (

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

220:               s_accountPremiumOwed += feeGrowthX128 * R * (1 + ν*R/N) / R

221:                                    += feeGrowthX128 * (T - R + ν*R)/N

222:                                    += feeGrowthX128 * T/N * (1 - R/T + ν*R/T)

236:              s_accountPremiumOwed += feesCollected * T/N^2 * (1 - R/T + ν*R/T)          (Eqn 3)     

256:             s_accountPremiumGross += feesCollected * T/N^2 * (1 - R/T + ν*R^2/T^2)       (Eqn 4) 

745:                 amount0 += Math.getAmount0ForLiquidity(liquidityChunk);

747:                 amount1 += Math.getAmount1ForLiquidity(liquidityChunk);

879:                     removedLiquidity += chunkLiquidity;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

225:             offset += charOffset;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Math.sol

95:                 r += 32;

99:                 r += 16;

103:                 r += 8;

107:                 r += 4;

111:                 r += 2;

114:                 r += 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

53:             poolId += uint64(uint24(tickSpacing)) << 48;

314:                         shift += 1;

829:                     bonus1 += Math.min(

847:                     bonus0 += Math.min(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

107:             balanceOf[to][id] += amount;

151:                 balanceOf[to][id] += amount;

217:             balanceOf[to][id] += amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

67:             balanceOf[to] += amount;

91:             balanceOf[to] += amount;

109:             balanceOf[to] += amount;

126:             balanceOf[to] += amount;

128:         totalSupply += amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

### <a name="GAS-2"></a>[GAS-2] Use assembly to check for `address(0)`

*Saves 6 gas per instance*

*Instances (6)*:

```solidity
File: contracts/PanopticFactory.sol

183:         if (address(v3Pool) == address(0)) revert Errors.UniswapPoolNotInitialized();

185:         if (address(s_getPanopticPool[v3Pool]) != address(0))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

272:         if (address(s_univ3pool) != address(0)) revert Errors.PoolAlreadyInitialized();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

323:         // There are 281,474,976,710,655 possible pool patterns.

346:     /// @dev Pays the pool tokens owed for the minted liquidity from the payer (always the caller).

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

78:             return addr == address(0) ? 40 : 39 - Math.mostSignificantNibble(uint160(addr));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-3"></a>[GAS-3] Using bools for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (14)*:

```solidity
File: contracts/CollateralTracker.sol

93:     bool internal s_initialized;

102:     bool internal s_underlyingIsToken0;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

102:     bool internal constant COMPUTE_ALL_PREMIA = true;

104:     bool internal constant COMPUTE_LONG_PREMIA = false;

107:     bool internal constant ONLY_AVAILABLE_PREMIUM = false;

110:     bool internal constant COMMIT_LONG_SETTLED = true;

112:     bool internal constant DONOT_COMMIT_LONG_SETTLED = false;

115:     bool internal constant ASSERT_SOLVENCY = true;

118:     bool internal constant ASSERT_INSOLVENCY = false;

121:     bool internal constant ADD = true;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

116:     bool internal constant MINT = false;

119:     bool internal constant BURN = true;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/Constants.sol

28:     bool internal constant SLOW_ORACLE_UNISWAP_MODE = false;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Constants.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

71:     mapping(address owner => mapping(address operator => bool approvedForAll))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

### <a name="GAS-4"></a>[GAS-4] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (10)*:

```solidity
File: contracts/PanopticPool.sol

722:         for (uint256 i = 0; i < positionIdList.length; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

512:         for (uint256 i = 0; i < ids.length; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

220:         for (uint256 i = 0; i < bytes(chars).length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

28:         for (uint256 i = 0; i < properties.length; i++) {

29:             for (uint256 j = 0; j < indices[i].length; j++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/base/Multicall.sol

14:         for (uint256 i = 0; i < data.length; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

893:             for (uint256 i = 0; i < positionIdList.length; ++i) {

973:             for (uint256 i = 0; i < positionIdList.length; i++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

143:         for (uint256 i = 0; i < ids.length; ) {

187:             for (uint256 i = 0; i < owners.length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

### <a name="GAS-5"></a>[GAS-5] State variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (3)*:

```solidity
File: contracts/PanopticFactory.sol

209:             Clones.clone(COLLATERAL_REFERENCE)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

596:         ) = PanopticMath.getOracleTicks(s_univ3pool, s_miniMedian);

1366:         medianData = s_miniMedian;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

### <a name="GAS-6"></a>[GAS-6] Use calldata instead of memory for function arguments that do not get mutated

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly bypasses this loop.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gas-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one.

 *Saves 60 gas per instance*

*Instances (7)*:

```solidity
File: contracts/CollateralTracker.sol

1144:         uint256[2][] memory positionBalanceArray,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/base/FactoryNFT.sol

60:         string memory symbol0,

61:         string memory symbol1,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

53:         string memory prefix

80:         string memory prefix

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

884:         LeftRightSigned collateralRemaining,

885:         CollateralTracker collateral0,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-7"></a>[GAS-7] For Operations that will not overflow, you could use unchecked

*Instances (763)*:

```solidity
File: contracts/CollateralTracker.sol

5: import {PanopticPool} from "./PanopticPool.sol";

7: import {ERC20Minimal} from "@tokens/ERC20Minimal.sol";

8: import {Multicall} from "@base/Multicall.sol";

10: import {Constants} from "@libraries/Constants.sol";

11: import {Errors} from "@libraries/Errors.sol";

12: import {InteractionHelper} from "@libraries/InteractionHelper.sol";

13: import {Math} from "@libraries/Math.sol";

14: import {PanopticMath} from "@libraries/PanopticMath.sol";

15: import {SafeTransferLib} from "@libraries/SafeTransferLib.sol";

17: import {LeftRightUnsigned, LeftRightSigned} from "@types/LeftRight.sol";

18: import {LiquidityChunk} from "@types/LiquidityChunk.sol";

19: import {PositionBalance} from "@types/PositionBalance.sol";

20: import {TokenId} from "@types/TokenId.sol";

73:     string internal constant NAME_PREFIX = "POPT-V1";

224:         totalSupply = 10 ** 6;

248:             s_ITMSpreadFee = uint128((ITM_SPREAD_MULTIPLIER * fee) / DECIMALS);

357:             return uint256(s_poolAssets) + s_inAMM;

388:                 assets * (DECIMALS - COMMISSION_FEE),

390:                 totalAssets() * DECIMALS

420:         s_poolAssets += uint128(assets);

429:             return (convertToShares(type(uint104).max) * (DECIMALS - COMMISSION_FEE)) / DECIMALS;

445:             shares * DECIMALS,

447:             totalSupply * (DECIMALS - COMMISSION_FEE)

476:         s_poolAssets += uint128(assets);

489:         uint256 available = s_poolAssets - 1;

498:         uint256 supply = totalSupply; // Saves an extra SLOAD if totalSupply is non-zero.

521:             uint256 allowed = allowance[owner][msg.sender]; // Saves gas for limited approvals.

523:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

531:             s_poolAssets -= uint128(assets);

563:             uint256 allowed = allowance[owner][msg.sender]; // Saves gas for limited approvals.

565:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

572:         s_poolAssets -= uint128(assets);

593:         uint256 available = convertToShares(s_poolAssets - 1);

620:             uint256 allowed = allowance[owner][msg.sender]; // Saves gas for limited approvals.

622:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

632:             s_poolAssets -= uint128(assets);

677:         uint256 maxNumRangesFromStrike = 1; // technically "maxNum(Half)RangesFromStrike" but the name is long

680:             for (uint256 leg = 0; leg < positionId.countLegs(); ++leg) {

688:                                 uint24(positionId.width(leg) * positionId.tickSpacing()),

695:                         uint256(Math.abs(currentTick - positionId.strike(leg)) / range)

731:                         .toRightSlot(int128(uint128(currentValue0)) - int128(uint128(oracleValue0)))

732:                         .toLeftSlot(int128(uint128(currentValue1)) - int128(uint128(oracleValue1)))

740:             int256 fee = (FORCE_EXERCISE_COST >> (maxNumRangesFromStrike - 1)); // exponential decay of fee based on number of half ranges away from the price

744:                 .toRightSlot(int128((longAmounts.rightSlot() * fee) / DECIMALS_128))

745:                 .toLeftSlot(int128((longAmounts.leftSlot() * fee) / DECIMALS_128));

755:             return (s_inAMM * DECIMALS) / totalAssets();

776:                    100% - |                _------

777:                           |             _-¯

778:                           |          _-¯

779:                     20% - |---------¯

781:                           +---------+-------+-+--->   POOL_

790:                 min_sell_ratio /= 2;

791:                 utilization = -utilization;

808:                 min_sell_ratio +

809:                 ((DECIMALS - min_sell_ratio) * (uint256(utilization) - TARGET_POOL_UTIL)) /

810:                 (SATURATED_POOL_UTIL - TARGET_POOL_UTIL);

839:            10% - |----------__       min_ratio = 5%

840:            5%  - | . . . . .  ¯¯¯--______

842:                  +---------+-------+-+--->   POOL_

855:                 return BUYER_COLLATERAL_RATIO / 2;

861:                 (BUYER_COLLATERAL_RATIO +

862:                     (BUYER_COLLATERAL_RATIO * (SATURATED_POOL_UTIL - utilization)) /

863:                     (SATURATED_POOL_UTIL - TARGET_POOL_UTIL)) / 2; // do the division by 2 at the end after all addition and multiplication; b/c y1 = buyCollateralRatio / 2

868:           LIFECYCLE OF A COLLATERAL TOKEN AND DELEGATE/REVOKE LOGIC

875:         balanceOf[delegatee] += type(uint248).max;

883:         balanceOf[delegatee] -= type(uint248).max;

900:                 bonusAbs = uint256(-bonus);

907:             s_poolAssets += uint128(bonusAbs);

914:                     totalSupply += type(uint248).max - liquidateeBalance;

918:                     balanceOf[liquidatee] = liquidateeBalance - type(uint248).max;

926:                     totalSupply += type(uint248).max - liquidateeBalance;

931:                     liquidateeBalance -= type(uint248).max;

963:                                 _totalSupply - liquidateeBalance,

964:                                 uint256(Math.max(1, int256(totalAssets()) - bonus))

965:                             ) - liquidateeBalance,

966:                             _totalSupply * DECIMALS

987:                 _transferFrom(refundee, refunder, convertToShares(uint256(-assets)));

1010:             int256 updatedAssets = int256(uint256(s_poolAssets)) - swappedAmount;

1027:                 uint256 sharesToMint = convertToShares(uint256(-tokenToPay));

1036:             s_inAMM = uint256(int256(uint256(s_inAMM)) + (shortAmount - longAmount)).toUint128();

1059:             int256 updatedAssets = int256(uint256(s_poolAssets)) - swappedAmount;

1062:             int256 tokenToPay = int256(swappedAmount) -

1063:                 (longAmount - shortAmount) -

1076:                 uint256 sharesToMint = convertToShares(uint256(-tokenToPay));

1083:             s_poolAssets = uint256(updatedAssets + realizedPremium).toUint128();

1084:             s_inAMM = uint256(int256(uint256(s_inAMM)) - (shortAmount - longAmount)).toUint128();

1104:             int256 intrinsicValue = int256(swappedAmount) - (shortAmount - longAmount);

1109:                     s_ITMSpreadFee * uint256(Math.abs(intrinsicValue)),

1110:                     DECIMALS * 100

1114:                 exchangedAmount = intrinsicValue + int256(swapCommission);

1118:             exchangedAmount += int256(

1120:                     uint256(uint128(shortAmount + longAmount)) * COMMISSION_FEE,

1151:                     .wrap((convertToAssets(balanceOf[user]) + shortPremium).toUint128())

1154:                             ? (_getTotalRequiredCollateral(atTick, positionBalanceArray) +

1199:                 tokenRequired += _tokenRequired;

1202:                 ++i;

1225:             for (uint256 index = 0; index < numLegs; ++index) {

1229:                 tokenRequired += _getRequiredCollateralSingleLeg(

1255:             tokenId.riskPartner(index) == index // does this leg have a risk partner? Affects required collateral

1309:                     ((atTick >= tickUpper) && (tokenType == 1)) || // strike OTM when price >= upperTick for tokenType=1

1310:                     ((atTick < tickLower) && (tokenType == 0)) // strike OTM when price < lowerTick for tokenType=0

1325:                     uint160 ratio = tokenType == 1 // tokenType

1327:                             Math.max24(2 * (atTick - strike), Constants.MIN_V3POOL_TICK)

1328:                         ) // puts ->  price/strike

1330:                             Math.max24(2 * (strike - atTick), Constants.MIN_V3POOL_TICK)

1331:                         ); // calls -> strike/price

1337:                         ((atTick < tickLower) && (tokenType == 1)) || // strike ITM but out of range price < lowerTick for tokenType=1

1338:                         ((atTick >= tickUpper) && (tokenType == 0)) // strike ITM but out of range when price >= upperTick for tokenType=0

1341:                                     Short put BPR = 100% - (price/strike) + SCR

1348:                                          |        <- ITM . <-ATM-> . OTM ->

1349:                            100% + SCR% - |--__           .    .    .

1350:                                   100% - | . .¯¯--__     .    .    .

1351:                                          |    .     ¯¯--__    .    .

1352:                                    SCR - |    .          .¯¯--__________

1354:                                          +----+----------+----+----+--->   current

1355:                                          0   Liqui-     Pa  strike Pb       price

1357:                                              price = SCR*strike                                         

1360:                         uint256 c2 = Constants.FP96 - ratio;

1364:                         required += Math.mulDiv96RoundingUp(amountMoved, c2);

1372:                             (tickUpper - strike) + (strike - tickLower)

1376:                             scaleFactor - ratio,

1377:                             scaleFactor + Constants.FP96

1380:                         required += c3;

1448:                 required = Math.unsafeDivRoundingUp(amount * sellCollateral, DECIMALS);

1458:                 required = Math.unsafeDivRoundingUp(amount * buyCollateral, DECIMALS);

1508:                         ? movedPartnerRight - movedRight

1509:                         : movedRight - movedPartnerRight;

1512:                         ? movedPartnerLeft - movedLeft

1513:                         : movedLeft - movedPartnerLeft;

1533:                     ? Math.unsafeDivRoundingUp((notionalP - notional) * contracts, notional)

1534:                     : Math.unsafeDivRoundingUp((notional - notionalP) * contracts, notionalP);

1574:                     Put side of a short strangle, BPR = 100% - (100% - SCR/2)*(price/strike)

1579:                          |           <- ITM   .  OTM ->

1580:                   100% - |--__                .

1581:                          |    ¯¯--__          .

1582:                          |          ¯¯--__    .

1583:                  SCR/2 - |                ¯¯--______ <------ base collateral is half that of a single-leg

1584:                          +--------------------+--->   current

1592:             poolUtilization = -(poolUtilization == 0 ? int16(1) : poolUtilization);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

5: import {CollateralTracker} from "@contracts/CollateralTracker.sol";

6: import {PanopticPool} from "@contracts/PanopticPool.sol";

7: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

8: import {IUniswapV3Factory} from "univ3-core/interfaces/IUniswapV3Factory.sol";

9: import {IUniswapV3Pool} from "univ3-core/interfaces/IUniswapV3Pool.sol";

11: import {Multicall} from "@base/Multicall.sol";

12: import {FactoryNFT} from "@base/FactoryNFT.sol";

14: import {Clones} from "@openzeppelin/contracts/proxy/Clones.sol";

16: import {CallbackLib} from "@libraries/CallbackLib.sol";

17: import {Constants} from "@libraries/Constants.sol";

18: import {Errors} from "@libraries/Errors.sol";

19: import {Math} from "@libraries/Math.sol";

20: import {PanopticMath} from "@libraries/PanopticMath.sol";

21: import {SafeTransferLib} from "@libraries/SafeTransferLib.sol";

23: import {Pointer} from "@types/Pointer.sol";

271:             maxSalt = uint256(salt) + loops;

302:                 salt += 1;

371:             tickLower = (Constants.MIN_V3POOL_TICK / tickSpacing) * tickSpacing;

372:             tickUpper = -tickLower;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

5: import {CollateralTracker} from "@contracts/CollateralTracker.sol";

6: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

7: import {IUniswapV3Pool} from "univ3-core/interfaces/IUniswapV3Pool.sol";

9: import {ERC1155Holder} from "@openzeppelin/contracts/token/ERC1155/utils/ERC1155Holder.sol";

10: import {Multicall} from "@base/Multicall.sol";

12: import {Constants} from "@libraries/Constants.sol";

13: import {Errors} from "@libraries/Errors.sol";

14: import {InteractionHelper} from "@libraries/InteractionHelper.sol";

15: import {Math} from "@libraries/Math.sol";

16: import {PanopticMath} from "@libraries/PanopticMath.sol";

18: import {LeftRightUnsigned, LeftRightSigned} from "@types/LeftRight.sol";

19: import {LiquidityChunk} from "@types/LiquidityChunk.sol";

20: import {PositionBalance, PositionBalanceLibrary} from "@types/PositionBalance.sol";

21: import {TokenId} from "@types/TokenId.sol";

96:     int24 internal constant MIN_SWAP_TICK = Constants.MIN_V3POOL_TICK - 1;

99:     int24 internal constant MAX_SWAP_TICK = Constants.MAX_V3POOL_TICK + 1;

137:     uint64 internal constant MAX_SPREAD = 9 * (2 ** 32);

282:                 (uint256(block.timestamp) << 216) +

285:                 (uint256(0xF590A6F276170D89E9F276170D89E9F276170D89E9000000000000)) +

286:                 (uint256(uint24(currentTick)) << 24) + // add to slot 4

287:                 (uint256(uint24(currentTick))); // add to slot 3

445:                     ++leg;

450:                 ++k;

475:                           MINT/BURN INTERFACE

566:             tokenId = positionIdList[positionIdList.length - 1];

661:             return uint32(10_000 + (10_000 << 16));

698:             return utilization0 + (utilization1 << 16);

733:                 ++i;

830:             int256(fastOracleTick - slowOracleTick) ** 2 +

831:                 int256(lastObservedTick - slowOracleTick) ** 2 +

832:                 int256(currentTick - slowOracleTick) ** 2 >

833:             MAX_TICKS_DELTA ** 2

947:                 if (Math.abs(currentTick - twapTick) > MAX_TWAP_DELTA_LIQUIDATION)

1187:                 solvent += (

1200:                 ++i;

1258:                 uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1262:             return Math.abs(currentTick - medianTick) > MAX_TICKS_DELTA;

1283:             pLength = positionIdList.length - offset;

1295:                 ++i;

1423:             effectiveLiquidityFactorX32 = (uint256(removedLiquidity) * 2 ** 32) / netLiquidity;

1485:                                     ((premiumAccumulatorsByLeg[leg][0] -

1486:                                         premiumAccumulatorLast.rightSlot()) *

1487:                                         (liquidityChunk.liquidity())) / 2 ** 64

1494:                                     ((premiumAccumulatorsByLeg[leg][1] -

1495:                                         premiumAccumulatorLast.leftSlot()) *

1496:                                         (liquidityChunk.liquidity())) / 2 ** 64

1507:                 ++leg;

1529:         TokenId tokenId = positionIdList[positionIdList.length - 1];

1571:                 .toRightSlot(int128(int256((accumulatedPremium.rightSlot() * liquidity) / 2 ** 64)))

1572:                 .toLeftSlot(int128(int256((accumulatedPremium.leftSlot() * liquidity) / 2 ** 64)));

1575:             s_collateralToken0.exercise(owner, 0, 0, 0, -realizedPremia.rightSlot());

1576:             s_collateralToken1.exercise(owner, 0, 0, 0, -realizedPremia.leftSlot());

1614:         for (uint256 leg = 0; leg < numLegs; ++leg) {

1666:                     uint256 totalLiquidityBefore = totalLiquidity - positionLiquidity;

1688:                                 (grossCurrent0 *

1689:                                     positionLiquidity +

1690:                                     grossPremiumLast.rightSlot() *

1691:                                     totalLiquidityBefore) / totalLiquidity

1696:                                 (grossCurrent1 *

1697:                                     positionLiquidity +

1698:                                     grossPremiumLast.leftSlot() *

1699:                                     totalLiquidityBefore) / totalLiquidity

1727:             uint256 accumulated0 = ((premiumAccumulators[0] - grossPremiumLast.rightSlot()) *

1728:                 totalLiquidity) / 2 ** 64;

1729:             uint256 accumulated1 = ((premiumAccumulators[1] - grossPremiumLast.leftSlot()) *

1730:                 totalLiquidity) / 2 ** 64;

1737:                                 (uint256(premiumOwed.rightSlot()) * settledTokens.rightSlot()) /

1746:                                 (uint256(premiumOwed.leftSlot()) * settledTokens.leftSlot()) /

1785:             totalLiquidity = netLiquidity + removedLiquidity;

1855:                 uint256 totalLiquidityBefore = totalLiquidity + positionLiquidity;

1860:                     totalLiquidity + positionLiquidity,

1906:                                                 grossPremiumLast.rightSlot() * totalLiquidityBefore

1907:                                             ) -

1909:                                                     _premiumAccumulatorsByLeg[_leg][0] *

1911:                                                 )) + int256(legPremia.rightSlot() * 2 ** 64),

1914:                                     ) / totalLiquidity

1922:                                                 grossPremiumLast.leftSlot() * totalLiquidityBefore

1923:                                             ) -

1925:                                                     _premiumAccumulatorsByLeg[_leg][1] *

1927:                                                 )) + int256(legPremia.leftSlot()) * 2 ** 64,

1930:                                     ) / totalLiquidity

1945:                 ++leg;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

5: import {IUniswapV3Factory} from "univ3-core/interfaces/IUniswapV3Factory.sol";

6: import {IUniswapV3Pool} from "univ3-core/interfaces/IUniswapV3Pool.sol";

8: import {ERC1155} from "@tokens/ERC1155Minimal.sol";

9: import {Multicall} from "@base/Multicall.sol";

10: import {TransientReentrancyGuard} from "solmate/utils/TransientReentrancyGuard.sol";

12: import {CallbackLib} from "@libraries/CallbackLib.sol";

13: import {Constants} from "@libraries/Constants.sol";

14: import {Errors} from "@libraries/Errors.sol";

15: import {FeesCalc} from "@libraries/FeesCalc.sol";

16: import {Math} from "@libraries/Math.sol";

17: import {PanopticMath} from "@libraries/PanopticMath.sol";

18: import {SafeTransferLib} from "@libraries/SafeTransferLib.sol";

20: import {LeftRightUnsigned, LeftRightSigned, LeftRightLibrary} from "@types/LeftRight.sol";

21: import {LiquidityChunk} from "@types/LiquidityChunk.sol";

22: import {TokenId} from "@types/TokenId.sol";

151:           │  ┌────┐-T      due isLong=1   in the UniswapV3Pool 

153:           │  │    │                        ┌────┐-(T-R)  

154:           │  │    │         ┌────┐-R       │    │          

157:              total=T       removed=R      net=(T-R)

175:         keep track of the amount of fees that *would have been collected*, we call this the owed

183:         same tick using a tokenId with a isLong=1 parameter. Because the netLiquidity is only (T-R),

186:               net_feesCollectedX128 = feeGrowthX128 * (T - R)

187:                                     = feeGrowthX128 * N                                     

189:         where N = netLiquidity = T-R. Had that liquidity never been removed, we want the gross

192:               gross_feesCollectedX128 = feeGrowthX128 * T

200:               gross_feesCollectedX128 = net_feesCollectedX128 + owed_feesCollectedX128

204:               owed_feesCollectedX128 = feeGrowthX128 * R * (1 + spread)                      (Eqn 1)

208:               spread = ν*(liquidity removed from that strike)/(netLiquidity remaining at that strike)

209:                      = ν*R/N

211:         For an arbitrary parameter 0 <= ν <= 1 (ν = 1/2^VEGOID). This way, the gross_feesCollectedX128 will be given by: 

213:               gross_feesCollectedX128 = feeGrowthX128 * N + feeGrowthX128*R*(1 + ν*R/N) 

214:                                       = feeGrowthX128 * T + feesGrowthX128*ν*R^2/N         

215:                                       = feeGrowthX128 * T * (1 + ν*R^2/(N*T))                (Eqn 2)

217:         The s_accountPremiumOwed accumulator tracks the feeGrowthX128 * R * (1 + spread) term

220:               s_accountPremiumOwed += feeGrowthX128 * R * (1 + ν*R/N) / R

221:                                    += feeGrowthX128 * (T - R + ν*R)/N

222:                                    += feeGrowthX128 * T/N * (1 - R/T + ν*R/T)

228:              feesCollected = feesGrowthX128 * (T-R)

232:              feesGrowthX128 = feesCollected/N

236:              s_accountPremiumOwed += feesCollected * T/N^2 * (1 - R/T + ν*R/T)          (Eqn 3)     

241:              owedPremia(t1, t2) = (s_accountPremiumOwed_t2-s_accountPremiumOwed_t1) * r

242:                                 = ∆feesGrowthX128 * r * T/N * (1 - R/T + ν*R/T)

243:                                 = ∆feesGrowthX128 * r * (T - R + ν*R)/N

244:                                 = ∆feesGrowthX128 * r * (N + ν*R)/N

245:                                 = ∆feesGrowthX128 * r * (1 + ν*R/N)             (same as Eqn 1)

252:         However, since we require that Eqn 2 holds up-- ie. the gross fees collected should be equal

256:             s_accountPremiumGross += feesCollected * T/N^2 * (1 - R/T + ν*R^2/T^2)       (Eqn 4) 

261:             grossPremia(t1, t2) = ∆(s_accountPremiumGross) * t

262:                                 = ∆feeGrowthX128 * t * T/N * (1 - R/T + ν*R^2/T^2) 

263:                                 = ∆feeGrowthX128 * t * (T - R + ν*R^2/T) / N 

264:                                 = ∆feeGrowthX128 * t * (N + ν*R^2/T) / N

265:                                 = ∆feeGrowthX128 * t * (1  + ν*R^2/(N*T))   (same as Eqn 2)

270:         long+short liquidity to guarantee that liquidity deposited always receives the correct

335:             s_AddrToPoolIdData[univ3pool] = uint256(poolId) + 2 ** 255;

408:                        PUBLIC MINT/BURN FUNCTIONS

515:                 ++i;

575:                 ++leg;

614:         bool zeroForOne; // The direction of the swap, true for token0 to token1, false for token1 to token0

615:         int256 swapAmount; // The amount of token0 or token1 to swap

670:                 int256 net0 = itm0 - PanopticMath.convert1to0(itm1, sqrtPriceX96);

675:                 swapAmount = -net0;

678:                 swapAmount = -itm0;

681:                 swapAmount = -itm1;

695:                     ? Constants.MIN_V3POOL_SQRT_RATIO + 1

696:                     : Constants.MAX_V3POOL_SQRT_RATIO - 1,

745:                 amount0 += Math.getAmount0ForLiquidity(liquidityChunk);

747:                 amount1 += Math.getAmount1ForLiquidity(liquidityChunk);

771:                 ++leg;

778:         if (amount0 > uint128(type(int128).max - 4) || amount1 > uint128(type(int128).max - 4))

833:         LeftRightUnsigned currentLiquidity = s_accountLiquidity[positionKey]; //cache

851:                 updatedLiquidity = startingLiquidity + chunkLiquidity;

856:                     removedLiquidity -= chunkLiquidity;

872:                         updatedLiquidity = startingLiquidity - chunkLiquidity;

879:                     removedLiquidity += chunkLiquidity;

913:             : _burnLiquidity(liquidityChunk, univ3pool); // from msg.sender to Uniswap

1019:             CallbackLib.CallbackData({ // compute by reading values from univ3pool every time

1069:             movedAmounts = LeftRightSigned.wrap(0).toRightSlot(-int128(int256(amount0))).toLeftSlot(

1070:                 -int128(int256(amount1))

1125:                     ? receivedAmount0 - uint128(-movedInLeg.rightSlot())

1128:                     ? receivedAmount1 - uint128(-movedInLeg.leftSlot())

1166:             uint256 totalLiquidity = netLiquidity + removedLiquidity;

1178:                     totalLiquidity * 2 ** 64,

1179:                     netLiquidity ** 2

1183:                     totalLiquidity * 2 ** 64,

1184:                     netLiquidity ** 2

1193:                     uint256 numerator = netLiquidity + (removedLiquidity / 2 ** VEGOID);

1213:                     uint256 numerator = totalLiquidity ** 2 -

1214:                         totalLiquidity *

1215:                         removedLiquidity +

1216:                         ((removedLiquidity ** 2) / 2 ** (VEGOID));

1219:                         .mulDiv(premium0X64_base, numerator, totalLiquidity ** 2)

1222:                         .mulDiv(premium1X64_base, numerator, totalLiquidity ** 2)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

5: import {PanopticMath} from "@libraries/PanopticMath.sol";

6: import {PanopticPool} from "@contracts/PanopticPool.sol";

8: import {ERC721} from "solmate/tokens/ERC721.sol";

9: import {MetadataStore} from "@base/MetadataStore.sol";

11: import {Pointer} from "@types/Pointer.sol";

13: import {LibString} from "solady/utils/LibString.sol";

14: import {Base64} from "solady/utils/Base64.sol";

32:         ERC721("Panoptic Factory Deployer NFTs", "PANOPTIC-NFT")

73:                     "data:application/json;base64,",

80:                                     "-",

83:                                         "-",

91:                                     "-",

93:                                     "-",

100:                                     " - ",

107:                                 '"}], "image": "data:image/svg+xml;base64,',

126:             rarity < 18 ? rarity / 3 : rarity < 23 ? 23 - rarity : 0

129:             "<!-- LABEL -->",

138:                 "<!-- TEXT -->",

139:                 metadata[bytes32("descriptions")][lastCharVal + 16 * (rarity / 8)]

142:             .replace("<!-- ART -->", metadata[bytes32("art")][lastCharVal].decompressedDataStr())

143:             .replace("<!-- FILTER -->", metadata[bytes32("filters")][rarity].decompressedDataStr());

161:             .replace("<!-- POOLADDRESS -->", LibString.toHexString(uint160(panopticPool), 20))

162:             .replace("<!-- CHAINID -->", getChainName());

165:             "<!-- RARITY_NAME -->",

171:                 .replace("<!-- RARITY -->", write(LibString.toString(rarity)))

172:                 .replace("<!-- SYMBOL0 -->", write(symbol0, maxSymbolWidth(rarity)))

173:                 .replace("<!-- SYMBOL1 -->", write(symbol1, maxSymbolWidth(rarity)));

188:             return "Avalanche C-Chain";

220:         for (uint256 i = 0; i < bytes(chars).length; ++i) {

225:             offset += charOffset;

228:                 '<g transform="translate(-',

233:                 "</g>"

240:             uint256 _scale = (3400 * maxWidth) / offset;

254:             LibString.toString(offset / 2),

257:             "</g>"

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

5: import {Pointer} from "@types/Pointer.sol";

28:         for (uint256 i = 0; i < properties.length; i++) {

29:             for (uint256 j = 0; j < indices[i].length; j++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/base/Multicall.sol

25:                 assembly ("memory-safe") {

33:                 ++i;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

5: import {IUniswapV3Factory} from "univ3-core/interfaces/IUniswapV3Factory.sol";

7: import {Errors} from "@libraries/Errors.sol";

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Constants.sol

12:     int24 internal constant MIN_V3POOL_TICK = -887272;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Constants.sol)

```solidity
File: contracts/libraries/FeesCalc.sol

5: import {IUniswapV3Pool} from "univ3-core/interfaces/IUniswapV3Pool.sol";

7: import {Math} from "@libraries/Math.sol";

9: import {LeftRightUnsigned, LeftRightSigned} from "@types/LeftRight.sol";

114:                 feeGrowthInside0X128 = lowerOut0 - upperOut0; // fee growth inside the chunk

115:                 feeGrowthInside1X128 = lowerOut1 - upperOut1;

131:                 feeGrowthInside0X128 = upperOut0 - lowerOut0;

132:                 feeGrowthInside1X128 = upperOut1 - lowerOut1;

152:                 feeGrowthInside0X128 = univ3pool.feeGrowthGlobal0X128() - lowerOut0 - upperOut0;

153:                 feeGrowthInside1X128 = univ3pool.feeGrowthGlobal1X128() - lowerOut1 - upperOut1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/FeesCalc.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

5: import {CollateralTracker} from "@contracts/CollateralTracker.sol";

6: import {IERC20Metadata} from "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";

7: import {IERC20Partial} from "@tokens/interfaces/IERC20Partial.sol";

8: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

10: import {PanopticMath} from "@libraries/PanopticMath.sol";

66:                     "/",

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/Math.sol

5: import {Errors} from "@libraries/Errors.sol";

6: import {Constants} from "@libraries/Constants.sol";

8: import {LiquidityChunk, LiquidityChunkLibrary} from "@types/LiquidityChunk.sol";

15:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

74:         return x > 0 ? x : -x;

83:             return x > 0 ? uint256(x) : uint256(-x);

95:                 r += 32;

99:                 r += 16;

103:                 r += 8;

107:                 r += 4;

111:                 r += 2;

114:                 r += 1;

130:             uint256 absTick = tick < 0 ? uint256(-int256(tick)) : uint256(int256(tick));

142:             if (absTick & 0x2 != 0) sqrtR = (sqrtR * 0xfff97272373d413259a46990580e213a) >> 128;

144:             if (absTick & 0x4 != 0) sqrtR = (sqrtR * 0xfff2e50f5f656932ef12357cf3c7fdcc) >> 128;

146:             if (absTick & 0x8 != 0) sqrtR = (sqrtR * 0xffe5caca7e10e4e61c3624eaa0941cd0) >> 128;

148:             if (absTick & 0x10 != 0) sqrtR = (sqrtR * 0xffcb9843d60f6159c9db58835c926644) >> 128;

150:             if (absTick & 0x20 != 0) sqrtR = (sqrtR * 0xff973b41fa98c081472e6896dfb254c0) >> 128;

152:             if (absTick & 0x40 != 0) sqrtR = (sqrtR * 0xff2ea16466c96a3843ec78b326b52861) >> 128;

154:             if (absTick & 0x80 != 0) sqrtR = (sqrtR * 0xfe5dee046a99a2a811c461f1969c3053) >> 128;

156:             if (absTick & 0x100 != 0) sqrtR = (sqrtR * 0xfcbe86c7900a88aedcffc83b479aa3a4) >> 128;

158:             if (absTick & 0x200 != 0) sqrtR = (sqrtR * 0xf987a7253ac413176f2b074cf7815e54) >> 128;

160:             if (absTick & 0x400 != 0) sqrtR = (sqrtR * 0xf3392b0822b70005940c7a398e4b70f3) >> 128;

162:             if (absTick & 0x800 != 0) sqrtR = (sqrtR * 0xe7159475a2c29b7443b29c7fa6e889d9) >> 128;

164:             if (absTick & 0x1000 != 0) sqrtR = (sqrtR * 0xd097f3bdfd2022b8845ad8f792aa5825) >> 128;

166:             if (absTick & 0x2000 != 0) sqrtR = (sqrtR * 0xa9f746462d870fdf8a65dc1f90e061e5) >> 128;

168:             if (absTick & 0x4000 != 0) sqrtR = (sqrtR * 0x70d869a156d2a1b890bb3df62baf32f7) >> 128;

170:             if (absTick & 0x8000 != 0) sqrtR = (sqrtR * 0x31be135f97d08fd981231505542fcfa6) >> 128;

172:             if (absTick & 0x10000 != 0) sqrtR = (sqrtR * 0x9aa508b5b7a84e1c677de54f3e99bc9) >> 128;

174:             if (absTick & 0x20000 != 0) sqrtR = (sqrtR * 0x5d6af8dedb81196699c329225ee604) >> 128;

176:             if (absTick & 0x40000 != 0) sqrtR = (sqrtR * 0x2216e584f5fa1ea926041bedfe98) >> 128;

178:             if (absTick & 0x80000 != 0) sqrtR = (sqrtR * 0x48a170391f7dc42444e8fa2) >> 128;

181:             if (tick > 0) sqrtR = type(uint256).max / sqrtR;

184:             return uint160((sqrtR >> 32) + (sqrtR % (1 << 32) == 0 ? 0 : 1));

189:                     LIQUIDITY AMOUNTS (STRIKE+WIDTH)

203:                     highPriceX96 - lowPriceX96,

205:                 ) / lowPriceX96;

217:             return mulDiv96(liquidityChunk.liquidity(), highPriceX96 - lowPriceX96);

263:                             highPriceX96 - lowPriceX96

289:                     toUint128(mulDiv(amount1, Constants.FP96, highPriceX96 - lowPriceX96))

356:             uint256 prod0; // Least significant 256 bits of the product

357:             uint256 prod1; // Most significant 256 bits of the product

358:             assembly ("memory-safe") {

367:                 assembly ("memory-safe") {

384:             assembly ("memory-safe") {

388:             assembly ("memory-safe") {

396:             uint256 twos = (0 - denominator) & denominator;

398:             assembly ("memory-safe") {

403:             assembly ("memory-safe") {

409:             assembly ("memory-safe") {

412:             prod0 |= prod1 * twos;

419:             uint256 inv = (3 * denominator) ^ 2;

423:             inv *= 2 - denominator * inv; // inverse mod 2**8

424:             inv *= 2 - denominator * inv; // inverse mod 2**16

425:             inv *= 2 - denominator * inv; // inverse mod 2**32

426:             inv *= 2 - denominator * inv; // inverse mod 2**64

427:             inv *= 2 - denominator * inv; // inverse mod 2**128

428:             inv *= 2 - denominator * inv; // inverse mod 2**256

436:             result = prod0 * inv;

456:             uint256 prod0; // Least significant 256 bits of the product

457:             uint256 prod1; // Most significant 256 bits of the product

458:             assembly ("memory-safe") {

467:                 assembly ("memory-safe") {

482:             assembly ("memory-safe") {

486:             assembly ("memory-safe") {

494:             uint256 twos = (0 - denominator) & denominator;

496:             assembly ("memory-safe") {

501:             assembly ("memory-safe") {

507:             assembly ("memory-safe") {

510:             prod0 |= prod1 * twos;

517:             uint256 inv = (3 * denominator) ^ 2;

521:             inv *= 2 - denominator * inv; // inverse mod 2**8

522:             inv *= 2 - denominator * inv; // inverse mod 2**16

523:             inv *= 2 - denominator * inv; // inverse mod 2**32

524:             inv *= 2 - denominator * inv; // inverse mod 2**64

525:             inv *= 2 - denominator * inv; // inverse mod 2**128

526:             inv *= 2 - denominator * inv; // inverse mod 2**256

534:             result = prod0 * inv;

552:                 result++;

568:             uint256 prod0; // Least significant 256 bits of the product

569:             uint256 prod1; // Most significant 256 bits of the product

570:             assembly ("memory-safe") {

579:                 assembly ("memory-safe") {

587:             require(2 ** 64 > prod1);

596:             assembly ("memory-safe") {

600:             assembly ("memory-safe") {

606:             assembly ("memory-safe") {

614:             prod0 |= prod1 * 2 ** 192;

631:             uint256 prod0; // Least significant 256 bits of the product

632:             uint256 prod1; // Most significant 256 bits of the product

633:             assembly ("memory-safe") {

642:                 assembly ("memory-safe") {

650:             require(2 ** 96 > prod1);

659:             assembly ("memory-safe") {

663:             assembly ("memory-safe") {

669:             assembly ("memory-safe") {

677:             prod0 |= prod1 * 2 ** 160;

690:             if (mulmod(a, b, 2 ** 96) > 0) {

692:                 result++;

708:             uint256 prod0; // Least significant 256 bits of the product

709:             uint256 prod1; // Most significant 256 bits of the product

710:             assembly ("memory-safe") {

719:                 assembly ("memory-safe") {

727:             require(2 ** 128 > prod1);

736:             assembly ("memory-safe") {

740:             assembly ("memory-safe") {

746:             assembly ("memory-safe") {

754:             prod0 |= prod1 * 2 ** 128;

767:             if (mulmod(a, b, 2 ** 128) > 0) {

769:                 result++;

785:             uint256 prod0; // Least significant 256 bits of the product

786:             uint256 prod1; // Most significant 256 bits of the product

787:             assembly ("memory-safe") {

796:                 assembly ("memory-safe") {

804:             require(2 ** 192 > prod1);

813:             assembly ("memory-safe") {

817:             assembly ("memory-safe") {

823:             assembly ("memory-safe") {

831:             prod0 |= prod1 * 2 ** 64;

844:             if (mulmod(a, b, 2 ** 192) > 0) {

846:                 result++;

856:         assembly ("memory-safe") {

875:             int256 pivot = arr[uint256(left + (right - left) / 2)];

877:                 while (arr[uint256(i)] < pivot) i++;

878:                 while (pivot < arr[uint256(j)]) j--;

881:                     i++;

882:                     j--;

895:             quickSort(data, int256(0), int256(data.length - 1));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

5: import {CollateralTracker} from "@contracts/CollateralTracker.sol";

6: import {IERC20Metadata} from "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";

7: import {IUniswapV3Pool} from "univ3-core/interfaces/IUniswapV3Pool.sol";

9: import {Constants} from "@libraries/Constants.sol";

10: import {Errors} from "@libraries/Errors.sol";

11: import {Math} from "@libraries/Math.sol";

13: import {Strings} from "@openzeppelin/contracts/utils/Strings.sol";

15: import {LeftRightUnsigned, LeftRightSigned} from "@types/LeftRight.sol";

16: import {LiquidityChunk} from "@types/LiquidityChunk.sol";

17: import {TokenId} from "@types/TokenId.sol";

25:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

53:             poolId += uint64(uint24(tickSpacing)) << 48;

63:             return (poolId & TICKSPACING_MASK) + (uint48(poolId) + 1);

78:             return addr == address(0) ? 40 : 39 - Math.mostSignificantNibble(uint160(addr));

102:                 Strings.toString(fee / 100),

107:                         Strings.toString((fee / 10) % 10),

139:             ? uint8(existingHash >> 248) + 1

140:             : uint8(existingHash >> 248) - 1;

143:             return uint256(updatedHash) + (newPositionCount << 248);

228:             int256[] memory tickCumulatives = new int256[](cardinality + 1);

230:             uint256[] memory timestamps = new uint256[](cardinality + 1);

232:             for (uint256 i = 0; i < cardinality + 1; ++i) {

235:                         (int256(observationIndex) - int256(i * period)) +

243:             for (uint256 i = 0; i < cardinality; ++i) {

245:                     (tickCumulatives[i] - tickCumulatives[i + 1]) /

246:                     int256(timestamps[i] - timestamps[i + 1]);

250:             return (int24(Math.sort(ticks)[cardinality / 2]), int24(ticks[0]));

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

278:             if (block.timestamp >= uint256(uint40(medianData >> 216)) + period) {

283:                             int256(observationIndex) - int256(1) + int256(observationCardinality)

290:                         (tickCumulative_last - tickCumulative_old) /

291:                             int256(timestamp_last - timestamp_old)

302:                 for (uint8 i; i < 8; ++i) {

304:                     rank = (orderMap >> (3 * i)) % 8;

307:                         shift -= 1;

312:                     entry = int24(uint24(medianData >> (rank * 24)));

314:                         shift += 1;

318:                     newOrderMap = newOrderMap + ((rank + 1) << (3 * (i + shift - 1)));

322:                     (block.timestamp << 216) +

323:                     (uint256(newOrderMap) << 192) +

324:                     uint256(uint192(medianData << 24)) +

343:             for (uint256 i = 0; i < 20; ++i) {

344:                 secondsAgos[i] = uint32(((i + 1) * twapWindow) / 20);

351:             for (uint256 i = 0; i < 19; ++i) {

353:                     (tickCumulatives[i] - tickCumulatives[i + 1]) / int56(uint56(twapWindow / 20))

419:         uint256 amount = uint256(positionSize) * tokenId.optionRatio(legIndex);

441:             int24 minTick = (Constants.MIN_V3POOL_TICK / tickSpacing) * tickSpacing;

442:             int24 maxTick = (Constants.MAX_V3POOL_TICK / tickSpacing) * tickSpacing;

446:             (tickLower, tickUpper) = (strike - rangeDown, strike + rangeUp);

471:             (width * tickSpacing) / 2,

472:             int24(int256(Math.unsafeDivRoundingUp(uint24(width) * uint24(tickSpacing), 2)))

502:                 ++leg;

517:                 return Math.mulDiv192(amount, uint256(sqrtPriceX96) ** 2);

537:                 return Math.mulDiv192RoundingUp(amount, uint256(sqrtPriceX96) ** 2);

554:                 return Math.mulDiv(amount, 2 ** 192, uint256(sqrtPriceX96) ** 2);

556:                 return Math.mulDiv(amount, 2 ** 128, Math.mulDiv64(sqrtPriceX96, sqrtPriceX96));

574:                 return Math.mulDivRoundingUp(amount, 2 ** 192, uint256(sqrtPriceX96) ** 2);

579:                         2 ** 128,

597:                     .mulDiv192(Math.absUint(amount), uint256(sqrtPriceX96) ** 2)

599:                 return amount < 0 ? -absResult : absResult;

604:                 return amount < 0 ? -absResult : absResult;

620:                     .mulDiv(Math.absUint(amount), 2 ** 192, uint256(sqrtPriceX96) ** 2)

622:                 return amount < 0 ? -absResult : absResult;

627:                         2 ** 128,

631:                 return amount < 0 ? -absResult : absResult;

650:                 tokenData0.rightSlot() +

652:                 tokenData0.leftSlot() +

658:             PanopticMath.convert0to1(tokenData0.rightSlot(), sqrtPriceX96) + tokenData1.rightSlot(),

659:             PanopticMath.convert0to1RoundingUp(tokenData0.leftSlot(), sqrtPriceX96) +

687:             amount0 = positionSize * uint128(tokenId.optionRatio(legIndex));

691:             amount1 = positionSize * uint128(tokenId.optionRatio(legIndex));

693:             amount0 = Math.mulDivRoundingUp(amount1, 2 ** 96, geometricMeanPriceX96).toUint128();

736:                 LIQUIDATION/FORCE EXERCISE CALCULATIONS

767:                 uint256 bonusCross = Math.min(balanceCross / 2, thresholdCross - balanceCross);

774:                         2 ** 128,

782:                             Math.mulDiv128(bonusCross, 2 ** 128 - requiredRatioX128),

790:                         2 ** 128,

798:                             Math.mulDiv128(bonusCross, 2 ** 128 - requiredRatioX128),

807:             int256 balance0 = int256(uint256(tokenData0.rightSlot())) -

809:             int256 balance1 = int256(uint256(tokenData1.rightSlot())) -

812:             int256 paid0 = bonus0 + int256(netPaid.rightSlot());

813:             int256 paid1 = bonus1 + int256(netPaid.leftSlot());

829:                     bonus1 += Math.min(

830:                         balance1 - paid1,

831:                         PanopticMath.convert0to1(paid0 - balance0, atSqrtPriceX96)

833:                     bonus0 -= Math.min(

834:                         PanopticMath.convert1to0(balance1 - paid1, atSqrtPriceX96),

835:                         paid0 - balance0

847:                     bonus0 += Math.min(

848:                         balance0 - paid0,

849:                         PanopticMath.convert1to0(paid1 - balance1, atSqrtPriceX96)

851:                     bonus1 -= Math.min(

852:                         PanopticMath.convert0to1(balance0 - paid0, atSqrtPriceX96),

853:                         paid1 - balance1

858:             paid0 = bonus0 + int256(netPaid.rightSlot());

859:             paid1 = bonus1 + int256(netPaid.leftSlot());

862:                 LeftRightSigned.wrap(0).toRightSlot(int128(balance0 - paid0)).toLeftSlot(

863:                     int128(balance1 - paid1)

893:             for (uint256 i = 0; i < positionIdList.length; ++i) {

896:                 for (uint256 leg = 0; leg < numLegs; ++leg) {

903:             int256 collateralDelta0 = -Math.min(collateralRemaining.rightSlot(), 0);

904:             int256 collateralDelta1 = -Math.min(collateralRemaining.leftSlot(), 0);

916:                     -Math.min(

917:                         collateralDelta0 - longPremium.rightSlot(),

919:                             longPremium.leftSlot() - collateralDelta1,

924:                         longPremium.leftSlot() - collateralDelta1,

926:                             collateralDelta0 - longPremium.rightSlot(),

933:                 haircut1 = protocolLoss1 + collateralDelta1;

941:                         longPremium.rightSlot() - collateralDelta0,

943:                             collateralDelta1 - longPremium.leftSlot(),

947:                     -Math.min(

948:                         collateralDelta1 - longPremium.leftSlot(),

950:                             longPremium.rightSlot() - collateralDelta0,

956:                 haircut0 = collateralDelta0 + protocolLoss0;

973:             for (uint256 i = 0; i < positionIdList.length; i++) {

976:                 for (uint256 leg = 0; leg < tokenId.countLegs(); ++leg) {

983:                             uint128(-_premiasByLeg[i][leg].rightSlot()) * uint256(haircut0),

987:                             uint128(-_premiasByLeg[i][leg].leftSlot()) * uint256(haircut1),

1003:                             uint128(-_premiasByLeg[i][leg].rightSlot()) - settled0

1007:                             uint128(-_premiasByLeg[i][leg].leftSlot()) - settled1

1042:             int256 balanceShortage = int256(uint256(type(uint248).max)) -

1043:                 int256(ct0.balanceOf(exercisee)) -

1044:                 int256(ct0.convertToShares(uint128(-exerciseFees.rightSlot())));

1052:                                 exerciseFees.rightSlot() -

1069:                                 ) + exerciseFees.leftSlot()

1075:                 int256(uint256(type(uint248).max)) -

1076:                 int256(ct1.balanceOf(exercisee)) -

1077:                 int256(ct1.convertToShares(uint128(-exerciseFees.leftSlot())));

1089:                                 ) + exerciseFees.rightSlot()

1094:                                 exerciseFees.leftSlot() -

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

5: import {Errors} from "@libraries/Errors.sol";

24:         assembly ("memory-safe") {

30:             mstore(add(4, p), from) // Append the "from" argument.

31:             mstore(add(36, p), to) // Append the "to" argument.

32:             mstore(add(68, p), amount) // Append the "amount" argument.

55:         assembly ("memory-safe") {

61:             mstore(add(4, p), to) // Append the "to" argument.

62:             mstore(add(36, p), amount) // Append the "amount" argument.

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

5: import {ERC1155Holder} from "@openzeppelin/contracts/token/ERC1155/utils/ERC1155Holder.sol";

103:         balanceOf[from][id] -= amount;

107:             balanceOf[to][id] += amount;

147:             balanceOf[from][id] -= amount;

151:                 balanceOf[to][id] += amount;

157:                 ++i;

187:             for (uint256 i = 0; i < owners.length; ++i) {

202:             interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165

203:             interfaceId == 0xd9b67a26; // ERC165 Interface ID for ERC1155

207:                         INTERNAL MINT/BURN LOGIC

217:             balanceOf[to][id] += amount;

237:         balanceOf[from][id] -= amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

62:         balanceOf[msg.sender] -= amount;

67:             balanceOf[to] += amount;

82:         uint256 allowed = allowance[from][msg.sender]; // Saves gas for limited approvals.

84:         if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed - amount;

86:         balanceOf[from] -= amount;

91:             balanceOf[to] += amount;

104:         balanceOf[from] -= amount;

109:             balanceOf[to] += amount;

116:                         INTERNAL MINT/BURN LOGIC

126:             balanceOf[to] += amount;

128:         totalSupply += amount;

137:         balanceOf[from] -= amount;

142:             totalSupply -= amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

```solidity
File: contracts/types/LeftRight.sol

5: import {Errors} from "@libraries/Errors.sol";

6: import {Math} from "@libraries/Math.sol";

67:                     (LeftRightUnsigned.unwrap(self) & LEFT_HALF_BIT_MASK) +

68:                         uint256(uint128(LeftRightUnsigned.unwrap(self)) + right)

87:                     (LeftRightSigned.unwrap(self) & LEFT_HALF_BIT_MASK_INT) +

88:                         (int256(int128(LeftRightSigned.unwrap(self)) + right) & RIGHT_HALF_BIT_MASK)

125:             return LeftRightUnsigned.wrap(LeftRightUnsigned.unwrap(self) + (uint256(left) << 128));

135:             return LeftRightSigned.wrap(LeftRightSigned.unwrap(self) + (int256(left) << 128));

154:             z = LeftRightUnsigned.wrap(LeftRightUnsigned.unwrap(x) + LeftRightUnsigned.unwrap(y));

177:             z = LeftRightUnsigned.wrap(LeftRightUnsigned.unwrap(x) - LeftRightUnsigned.unwrap(y));

195:             int256 left = int256(uint256(x.leftSlot())) + y.leftSlot();

200:             int256 right = int256(uint256(x.rightSlot())) + y.rightSlot();

215:             int256 left256 = int256(x.leftSlot()) + y.leftSlot();

218:             int256 right256 = int256(x.rightSlot()) + y.rightSlot();

233:             int256 left256 = int256(x.leftSlot()) - y.leftSlot();

236:             int256 right256 = int256(x.rightSlot()) - y.rightSlot();

255:             int256 left256 = int256(x.leftSlot()) - y.leftSlot();

258:             int256 right256 = int256(x.rightSlot()) - y.rightSlot();

284:         uint128 z_xR = (uint256(x.rightSlot()) + dx.rightSlot()).toUint128Capped();

285:         uint128 z_xL = (uint256(x.leftSlot()) + dx.leftSlot()).toUint128Capped();

286:         uint128 z_yR = (uint256(y.rightSlot()) + dy.rightSlot()).toUint128Capped();

287:         uint128 z_yL = (uint256(y.leftSlot()) + dy.leftSlot()).toUint128Capped();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

5: import {TokenId} from "@types/TokenId.sol";

78:                     (uint256(uint24(_tickLower)) << 232) +

79:                         (uint256(uint24(_tickUpper)) << 208) +

94:             return LiquidityChunk.wrap(LiquidityChunk.unwrap(self) + amount);

109:                     LiquidityChunk.unwrap(self) + (uint256(uint24(_tickLower)) << 232)

126:                     LiquidityChunk.unwrap(self) + ((uint256(uint24(_tickUpper))) << 208)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

5: import {LibZip} from "solady/utils/LibZip.sol";

23:             Pointer.wrap((uint256(_size) << 208) + (uint256(_start) << 160) + uint160(_location));

57:         assembly ("memory-safe") {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

50:                     (uint256(_tickData) << 160) +

51:                         (uint256(_utilizations) << 128) +

71:                 uint96(uint24(_currentTick)) +

72:                 (uint96(uint24(_fastOracleTick)) << 24) +

73:                 (uint96(uint24(_slowOracleTick)) << 48) +

148:             return int256((PositionBalance.unwrap(self) >> 128) % 2 ** 16);

157:             return int256((PositionBalance.unwrap(self) >> 144) % 2 ** 16);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

5: import {Constants} from "@libraries/Constants.sol";

6: import {Errors} from "@libraries/Errors.sol";

7: import {PanopticMath} from "@libraries/PanopticMath.sol";

98:             return int24(uint24((TokenId.unwrap(self) >> 48) % 2 ** 16));

110:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48)) % 2);

120:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 1)) % 128);

130:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 8)) % 2);

140:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 9)) % 2);

150:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 10)) % 4);

160:             return int24(int256(TokenId.unwrap(self) >> (64 + legIndex * 48 + 12)));

171:             return int24(int256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 36)) % 4096));

172:         } // "% 4096" = take last (2 ** 12 = 4096) 12 bits

185:             return TokenId.wrap(TokenId.unwrap(self) + _poolId);

195:             return TokenId.wrap(TokenId.unwrap(self) + (uint256(uint24(_tickSpacing)) << 48));

212:                 TokenId.wrap(TokenId.unwrap(self) + (uint256(_asset % 2) << (64 + legIndex * 48)));

229:                     TokenId.unwrap(self) + (uint256(_optionRatio % 128) << (64 + legIndex * 48 + 1))

246:             return TokenId.wrap(TokenId.unwrap(self) + ((_isLong % 2) << (64 + legIndex * 48 + 8)));

263:                     TokenId.unwrap(self) + (uint256(_tokenType % 2) << (64 + legIndex * 48 + 9))

281:                     TokenId.unwrap(self) + (uint256(_riskPartner % 4) << (64 + legIndex * 48 + 10))

299:                     TokenId.unwrap(self) +

300:                         uint256((int256(_strike) & BITMASK_INT24) << (64 + legIndex * 48 + 12))

319:                     TokenId.unwrap(self) +

320:                         (uint256(uint24(_width) % 4096) << (64 + legIndex * 48 + 36))

376:             if (optionRatios < 2 ** 64) {

378:             } else if (optionRatios < 2 ** 112) {

380:             } else if (optionRatios < 2 ** 160) {

382:             } else if (optionRatios < 2 ** 208) {

395:                         ((LONG_MASK >> (48 * (4 - optionRatios))) & CLEAR_POOLID_MASK)

406:             return self.isLong(0) + self.isLong(1) + self.isLong(2) + self.isLong(3);

439:         if (optionRatios < 2 ** 64) {

441:         } else if (optionRatios < 2 ** 112) {

443:         } else if (optionRatios < 2 ** 160) {

445:         } else if (optionRatios < 2 ** 208) {

507:             for (uint256 i = 0; i < 4; ++i) {

512:                     if ((TokenId.unwrap(self) >> (64 + 48 * i)) != 0)

515:                     break; // we are done iterating over potential legs

520:                 for (uint256 j = i + 1; j < numLegs; ++j) {

521:                     if (uint48(chunkData >> (48 * i)) == uint48(chunkData >> (48 * j))) {

569:             } // end for loop over legs

581:             for (uint256 i = 0; i < numLegs; ++i) {

589:                 if ((currentTick >= _strike + rangeUp) || (currentTick < _strike - rangeDown)) {

592:                     if (self.isLong(i) == 1) return; // validated

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="GAS-8"></a>[GAS-8] Avoid contract existence checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (5)*:

```solidity
File: contracts/PanopticPool.sol

314:             ct0.convertToAssets(ct0.balanceOf(msg.sender)) < minValue0 ||

315:             ct1.convertToAssets(ct1.balanceOf(msg.sender)) < minValue1

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/base/Multicall.sol

15:             (bool success, bytes memory result) = address(this).delegatecall(data[i]);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

1043:                 int256(ct0.balanceOf(exercisee)) -

1076:                 int256(ct1.balanceOf(exercisee)) -

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-9"></a>[GAS-9] State variables only set in the constructor should be declared `immutable`

Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around **20 000 gas** per variable) and replace the expensive storage-reading operations (around **2100 gas** per reading) to a less expensive value reading (**3 gas**)

*Instances (14)*:

```solidity
File: contracts/CollateralTracker.sol

194:         COMMISSION_FEE = _commissionFee;

195:         SELLER_COLLATERAL_RATIO = _sellerCollateralRatio;

196:         BUYER_COLLATERAL_RATIO = _buyerCollateralRatio;

197:         FORCE_EXERCISE_COST = _forceExerciseCost;

198:         TARGET_POOL_UTIL = _targetPoolUtilization;

199:         SATURATED_POOL_UTIL = _saturatedPoolUtilization;

200:         ITM_SPREAD_MULTIPLIER = _ITMSpreadMultiplier;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

115:         WETH = _WETH9;

116:         SFPM = _SFPM;

117:         UNIV3_FACTORY = _univ3Factory;

118:         POOL_REFERENCE = _poolReference;

119:         COLLATERAL_REFERENCE = _collateralReference;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

254:         SFPM = _sfpm;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

312:         // return if the pool has already been initialized in SFPM

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

### <a name="GAS-10"></a>[GAS-10] Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (3)*:

```solidity
File: contracts/CollateralTracker.sol

874:     function delegate(address delegatee) external onlyPanopticPool {

882:     function revoke(address delegatee) external onlyPanopticPool {

982:     function refund(address refunder, address refundee, int256 assets) external onlyPanopticPool {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="GAS-11"></a>[GAS-11] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (35)*:

```solidity
File: contracts/CollateralTracker.sol

776:                    100% - |                _------

779:                     20% - |---------¯

781:                           +---------+-------+-+--->   POOL_

839:            10% - |----------__       min_ratio = 5%

840:            5%  - | . . . . .  ¯¯¯--______

842:                  +---------+-------+-+--->   POOL_

1349:                            100% + SCR% - |--__           .    .    .

1350:                                   100% - | . .¯¯--__     .    .    .

1351:                                          |    .     ¯¯--__    .    .

1352:                                    SCR - |    .          .¯¯--__________

1354:                                          +----+----------+----+----+--->   current

1580:                   100% - |--__                .

1581:                          |    ¯¯--__          .

1582:                          |          ¯¯--__    .

1583:                  SCR/2 - |                ¯¯--______ <------ base collateral is half that of a single-leg

1584:                          +--------------------+--->   current

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

252:         However, since we require that Eqn 2 holds up-- ie. the gross fees collected should be equal

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

129:             "<!-- LABEL -->",

138:                 "<!-- TEXT -->",

142:             .replace("<!-- ART -->", metadata[bytes32("art")][lastCharVal].decompressedDataStr())

143:             .replace("<!-- FILTER -->", metadata[bytes32("filters")][rarity].decompressedDataStr());

161:             .replace("<!-- POOLADDRESS -->", LibString.toHexString(uint160(panopticPool), 20))

162:             .replace("<!-- CHAINID -->", getChainName());

165:             "<!-- RARITY_NAME -->",

171:                 .replace("<!-- RARITY -->", write(LibString.toString(rarity)))

172:                 .replace("<!-- SYMBOL0 -->", write(symbol0, maxSymbolWidth(rarity)))

173:                 .replace("<!-- SYMBOL1 -->", write(symbol1, maxSymbolWidth(rarity)));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

28:         for (uint256 i = 0; i < properties.length; i++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/libraries/Math.sol

552:                 result++;

692:                 result++;

769:                 result++;

846:                 result++;

881:                     i++;

882:                     j--;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

973:             for (uint256 i = 0; i < positionIdList.length; i++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-12"></a>[GAS-12] Use shift right/left instead of division/multiplication if possible

While the `DIV` / `MUL` opcode uses 5 gas, the `SHR` / `SHL` opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version `0.8+`

- Use `>> 1` instead of `/ 2`
- Use `>> 2` instead of `/ 4`
- Use `<< 3` instead of `* 8`
- ...
- Use `>> 5` instead of `/ 2^5 == / 32`
- Use `<< 6` instead of `* 2^6 == * 64`

TL;DR:

- Shifting left by N is like multiplying by 2^N (Each bits to the left is an increased power of 2)
- Shifting right by N is like dividing by 2^N (Each bits to the right is a decreased power of 2)

*Saves around 2 gas + 20 for unchecked per instance*

*Instances (30)*:

```solidity
File: contracts/CollateralTracker.sol

855:                 return BUYER_COLLATERAL_RATIO / 2;

863:                     (SATURATED_POOL_UTIL - TARGET_POOL_UTIL)) / 2; // do the division by 2 at the end after all addition and multiplication; b/c y1 = buyCollateralRatio / 2

1574:                     Put side of a short strangle, BPR = 100% - (100% - SCR/2)*(price/strike)

1583:                  SCR/2 - |                ¯¯--______ <------ base collateral is half that of a single-leg

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1423:             effectiveLiquidityFactorX32 = (uint256(removedLiquidity) * 2 ** 32) / netLiquidity;

1487:                                         (liquidityChunk.liquidity())) / 2 ** 64

1496:                                         (liquidityChunk.liquidity())) / 2 ** 64

1571:                 .toRightSlot(int128(int256((accumulatedPremium.rightSlot() * liquidity) / 2 ** 64)))

1572:                 .toLeftSlot(int128(int256((accumulatedPremium.leftSlot() * liquidity) / 2 ** 64)));

1728:                 totalLiquidity) / 2 ** 64;

1730:                 totalLiquidity) / 2 ** 64;

1911:                                                 )) + int256(legPremia.rightSlot() * 2 ** 64),

1927:                                                 )) + int256(legPremia.leftSlot()) * 2 ** 64,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

211:         For an arbitrary parameter 0 <= ν <= 1 (ν = 1/2^VEGOID). This way, the gross_feesCollectedX128 will be given by: 

1178:                     totalLiquidity * 2 ** 64,

1183:                     totalLiquidity * 2 ** 64,

1193:                     uint256 numerator = netLiquidity + (removedLiquidity / 2 ** VEGOID);

1216:                         ((removedLiquidity ** 2) / 2 ** (VEGOID));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

139:                 metadata[bytes32("descriptions")][lastCharVal + 16 * (rarity / 8)]

254:             LibString.toString(offset / 2),

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Math.sol

614:             prod0 |= prod1 * 2 ** 192;

677:             prod0 |= prod1 * 2 ** 160;

754:             prod0 |= prod1 * 2 ** 128;

831:             prod0 |= prod1 * 2 ** 64;

875:             int256 pivot = arr[uint256(left + (right - left) / 2)];

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

250:             return (int24(Math.sort(ticks)[cardinality / 2]), int24(ticks[0]));

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

471:             (width * tickSpacing) / 2,

767:                 uint256 bonusCross = Math.min(balanceCross / 2, thresholdCross - balanceCross);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-13"></a>[GAS-13] Increments/decrements can be unchecked in for-loops

In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (19)*:

```solidity
File: contracts/CollateralTracker.sol

680:             for (uint256 leg = 0; leg < positionId.countLegs(); ++leg) {

1225:             for (uint256 index = 0; index < numLegs; ++index) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

1614:         for (uint256 leg = 0; leg < numLegs; ++leg) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/base/FactoryNFT.sol

220:         for (uint256 i = 0; i < bytes(chars).length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

28:         for (uint256 i = 0; i < properties.length; i++) {

29:             for (uint256 j = 0; j < indices[i].length; j++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

232:             for (uint256 i = 0; i < cardinality + 1; ++i) {

243:             for (uint256 i = 0; i < cardinality; ++i) {

302:                 for (uint8 i; i < 8; ++i) {

343:             for (uint256 i = 0; i < 20; ++i) {

351:             for (uint256 i = 0; i < 19; ++i) {

893:             for (uint256 i = 0; i < positionIdList.length; ++i) {

896:                 for (uint256 leg = 0; leg < numLegs; ++leg) {

973:             for (uint256 i = 0; i < positionIdList.length; i++) {

976:                 for (uint256 leg = 0; leg < tokenId.countLegs(); ++leg) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

187:             for (uint256 i = 0; i < owners.length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/types/TokenId.sol

507:             for (uint256 i = 0; i < 4; ++i) {

520:                 for (uint256 j = i + 1; j < numLegs; ++j) {

581:             for (uint256 i = 0; i < numLegs; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="GAS-14"></a>[GAS-14] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (29)*:

```solidity
File: contracts/CollateralTracker.sol

983:         if (assets > 0) {

1017:             if (tokenToPay > 0) {

1066:             if (tokenToPay > 0) {

1153:                         positionBalanceArray.length > 0

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

140:         if (amount0Owed > 0)

147:         if (amount1Owed > 0)

180:         (token0, token1) = token0 < token1 ? (token0, token1) : (token1, token0);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

661:             return uint32(10_000 + (10_000 << 16));

1143:         if (positionIdListExercisor.length > 0)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

211:         For an arbitrary parameter 0 <= ν <= 1 (ν = 1/2^VEGOID). This way, the gross_feesCollectedX128 will be given by: 

360:         if (amount0Owed > 0)

367:         if (amount1Owed > 0)

394:         address token = amount0Delta > 0

401:         uint256 amountToPay = amount0Delta > 0 ? uint256(amount0Delta) : uint256(amount1Delta);

672:                 zeroForOne = net0 < 0;

677:                 zeroForOne = itm0 < 0;

680:                 zeroForOne = itm1 > 0;

916:         if (currentLiquidity.rightSlot() > 0) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/Math.sol

74:         return x > 0 ? x : -x;

83:             return x > 0 ? uint256(x) : uint256(-x);

181:             if (tick > 0) sqrtR = type(uint256).max / sqrtR;

366:                 require(denominator > 0);

466:                 require(denominator > 0);

550:             if (mulmod(a, b, denominator) > 0) {

690:             if (mulmod(a, b, 2 ** 96) > 0) {

767:             if (mulmod(a, b, 2 ** 128) > 0) {

844:             if (mulmod(a, b, 2 ** 192) > 0) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

1046:             if (balanceShortage > 0) {

1078:             if (balanceShortage > 0) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="GAS-15"></a>[GAS-15] `internal` functions not called by the contract should be removed

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (107)*:

```solidity
File: contracts/libraries/CallbackLib.sol

30:     function validateCallback(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Math.sol

25:     function min24(int24 a, int24 b) internal pure returns (int24) {

33:     function max24(int24 a, int24 b) internal pure returns (int24) {

41:     function min(uint256 a, uint256 b) internal pure returns (uint256) {

49:     function min(int256 a, int256 b) internal pure returns (int256) {

57:     function max(uint256 a, uint256 b) internal pure returns (uint256) {

65:     function max(int256 a, int256 b) internal pure returns (int256) {

73:     function abs(int256 x) internal pure returns (int256) {

81:     function absUint(int256 x) internal pure returns (uint256) {

91:     function mostSignificantNibble(uint160 x) internal pure returns (uint256 r) {

226:     function getAmountsForLiquidity(

246:     function getLiquidityForAmount0(

276:     function getLiquidityForAmount1(

307:     function toUint128Capped(uint256 toDowncast) internal pure returns (uint128 downcastedInt) {

316:     function toInt128(uint128 toCast) internal pure returns (int128 downcastedInt) {

323:     function toInt128(int256 toCast) internal pure returns (int128 downcastedInt) {

330:     function toInt256(uint256 toCast) internal pure returns (int256) {

445:     function mulDivCapped(

543:     function mulDivRoundingUp(

561:     function mulDiv64(uint256 a, uint256 b) internal pure returns (uint256) {

687:     function mulDiv96RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

764:     function mulDiv128RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

841:     function mulDiv192RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

855:     function unsafeDivRoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

893:     function sort(int256[] memory data) internal pure returns (int256[] memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

49:     function getPoolId(address univ3pool) internal view returns (uint64) {

61:     function incrementPoolPattern(uint64 poolId) internal pure returns (uint64) {

99:     function uniswapFeeToString(uint24 fee) internal pure returns (string memory) {

125:     function updatePositionsHash(

387:         uint128 positionSize

437:     ) internal pure returns (int24 tickLower, int24 tickUpper) {

469:     ) internal pure returns (int24, int24) {

488:     ) internal pure returns (LeftRightSigned longAmounts, LeftRightSigned shortAmounts) {

512:     function convert0to1(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

532:     ) internal pure returns (uint256) {

549:     function convert1to0(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

569:     ) internal pure returns (uint256) {

591:     function convert0to1(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

614:     function convert1to0(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

644:         LeftRightUnsigned tokenData1,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

21:     function safeTransferFrom(address token, address from, address to, uint256 amount) internal {

52:     function safeTransfer(address token, address to, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

214:     function _mint(address to, uint256 id, uint256 amount) internal {

236:     function _burn(address from, uint256 id, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

103:     function _transferFrom(address from, address to, uint256 amount) internal {

122:     function _mint(address to, uint256 amount) internal {

136:     function _burn(address from, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

```solidity
File: contracts/types/LeftRight.sol

38:     function rightSlot(LeftRightUnsigned self) internal pure returns (uint128) {

45:     function rightSlot(LeftRightSigned self) internal pure returns (int128) {

58:     function toRightSlot(

77:     function toRightSlot(

100:     function leftSlot(LeftRightUnsigned self) internal pure returns (uint128) {

107:     function leftSlot(LeftRightSigned self) internal pure returns (int128) {

120:     function toLeftSlot(

133:     function toLeftSlot(LeftRightSigned self, int128 left) internal pure returns (LeftRightSigned) {

147:     function add(

170:     function sub(

193:     function add(LeftRightUnsigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

213:     function add(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

231:     function sub(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

250:     function subRect(

278:     function addCapped(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

75:         unchecked {

94:             return LiquidityChunk.wrap(LiquidityChunk.unwrap(self) + amount);

107:             return

123:             // convert tick upper to uint24 as explicit conversion from int24 to uint256 is not allowed

139:         unchecked {

155:         unchecked {

173:             return int24(int256(LiquidityChunk.unwrap(self) >> 232));

182:             return int24(int256(LiquidityChunk.unwrap(self) >> 208));

191:             return uint128(LiquidityChunk.unwrap(self));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

17:     function createPointer(

29:     function location(Pointer self) internal pure returns (address) {

36:     function start(Pointer self) internal pure returns (uint48) {

43:     function size(Pointer self) internal pure returns (uint48) {

67:     function dataStr(Pointer self) internal view returns (string memory) {

74:     function decompressedDataStr(Pointer self) internal view returns (string memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

42:     function storeBalanceData(

63:     function packTickData(

85:     function lastObservedTick(PositionBalance self) internal pure returns (int24) {

94:     function slowOracleTick(PositionBalance self) internal pure returns (int24) {

103:     function fastOracleTick(PositionBalance self) internal pure returns (int24) {

112:     function currentTick(PositionBalance self) internal pure returns (int24) {

121:     function tickData(PositionBalance self) internal pure returns (uint96) {

146:     function utilization0(PositionBalance self) internal pure returns (int256) {

155:     function utilization1(PositionBalance self) internal pure returns (int256) {

164:     function utilizations(PositionBalance self) internal pure returns (uint32) {

173:     function positionSize(PositionBalance self) internal pure returns (uint128) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

87:     function poolId(TokenId self) internal pure returns (uint64) {

96:     function tickSpacing(TokenId self) internal pure returns (int24) {

108:     function asset(TokenId self, uint256 legIndex) internal pure returns (uint256) {

118:     function optionRatio(TokenId self, uint256 legIndex) internal pure returns (uint256) {

128:     function isLong(TokenId self, uint256 legIndex) internal pure returns (uint256) {

138:     function tokenType(TokenId self, uint256 legIndex) internal pure returns (uint256) {

148:     function riskPartner(TokenId self, uint256 legIndex) internal pure returns (uint256) {

158:     function strike(TokenId self, uint256 legIndex) internal pure returns (int24) {

169:     function width(TokenId self, uint256 legIndex) internal pure returns (int24) {

183:     function addPoolId(TokenId self, uint64 _poolId) internal pure returns (TokenId) {

193:     function addTickSpacing(TokenId self, int24 _tickSpacing) internal pure returns (TokenId) {

336:     function addLeg(

366:     function flipToBurnToken(TokenId self) internal pure returns (TokenId) {

404:     function countLongs(TokenId self) internal pure returns (uint256) {

416:     function asTicks(

432:     function countLegs(TokenId self) internal pure returns (uint256) {

464:     function clearLeg(TokenId self, uint256 i) internal pure returns (TokenId) {

500:     function validate(TokenId self) internal pure {

578:     function validateIsExercisable(TokenId self, int24 currentTick) internal pure {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="GAS-16"></a>[GAS-16] WETH address definition can be use directly

WETH is a wrap Ether contract with a specific address in the Ethereum network, giving the option to define it may cause false recognition, it is healthier to define it directly.

    Advantages of defining a specific contract directly:
    
    It saves gas,
    Prevents incorrect argument definition,
    Prevents execution on a different chain and re-signature issues,
    WETH Address : 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2

*Instances (2)*:

```solidity
File: contracts/PanopticFactory.sol

72:     address internal immutable WETH;

76:     uint256 internal constant FULL_RANGE_LIQUIDITY_AMOUNT_WETH = 0.1 ether;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

## Non Critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Missing checks for `address(0)` when assigning values to address state variables | 5 |
| [NC-2](#NC-2) | Array indices should be referenced via `enum`s rather than via numeric literals | 32 |
| [NC-3](#NC-3) | Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked` | 17 |
| [NC-4](#NC-4) | `constant`s should be defined rather than using magic numbers | 292 |
| [NC-5](#NC-5) | Control structures do not follow the Solidity Style Guide | 143 |
| [NC-6](#NC-6) | Unused `error` definition | 29 |
| [NC-7](#NC-7) | Events that mark critical parameter changes should contain both the old and the new value | 2 |
| [NC-8](#NC-8) | Function ordering does not follow the Solidity style guide | 6 |
| [NC-9](#NC-9) | Functions should not be longer than 50 lines | 138 |
| [NC-10](#NC-10) | Change int to int256 | 12 |
| [NC-11](#NC-11) | Lack of checks in setters | 2 |
| [NC-12](#NC-12) | Lines are too long | 1 |
| [NC-13](#NC-13) | `type(uint256).max` should be used instead of `2 ** 256 - 1` | 2 |
| [NC-14](#NC-14) | Missing Event for critical parameters change | 1 |
| [NC-15](#NC-15) | Incomplete NatSpec: `@param` is missing on actually documented functions | 2 |
| [NC-16](#NC-16) | Incomplete NatSpec: `@return` is missing on actually documented functions | 2 |
| [NC-17](#NC-17) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 12 |
| [NC-18](#NC-18) | Constant state variables defined more than once | 2 |
| [NC-19](#NC-19) | Adding a `return` statement when the function defines a named return variable, is redundant | 28 |
| [NC-20](#NC-20) | `require()` / `revert()` statements should have descriptive reason strings | 67 |
| [NC-21](#NC-21) | Take advantage of Custom Error's return value property | 51 |
| [NC-22](#NC-22) | Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`) | 1 |
| [NC-23](#NC-23) | Strings should use double quotes rather than single quotes | 11 |
| [NC-24](#NC-24) | Contract does not follow the Solidity style guide's suggested layout ordering | 6 |
| [NC-25](#NC-25) | Use Underscores for Number Literals (add an underscore every 3 digits) | 22 |
| [NC-26](#NC-26) | Internal and private variables and functions names should begin with an underscore | 160 |
| [NC-27](#NC-27) | Event is missing `indexed` fields | 12 |
| [NC-28](#NC-28) | Constants should be defined rather than using magic numbers | 33 |
| [NC-29](#NC-29) | `public` functions not called by the contract should be declared `external` instead | 7 |
| [NC-30](#NC-30) | Variables need not be initialized to zero | 30 |

### <a name="NC-1"></a>[NC-1] Missing checks for `address(0)` when assigning values to address state variables

*Instances (5)*:

```solidity
File: contracts/CollateralTracker.sol

240:         s_univ3token0 = token0;

241:         s_univ3token1 = token1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

115:         WETH = _WETH9;

118:         POOL_REFERENCE = _poolReference;

119:         COLLATERAL_REFERENCE = _collateralReference;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

### <a name="NC-2"></a>[NC-2] Array indices should be referenced via `enum`s rather than via numeric literals

*Instances (32)*:

```solidity
File: contracts/CollateralTracker.sol

1176:             TokenId tokenId = TokenId.wrap(positionBalanceArray[i][0]);

1179:             uint128 positionSize = PositionBalance.wrap(positionBalanceArray[i][1]).positionSize();

1185:                 ? int16(PositionBalance.wrap(positionBalanceArray[i][1]).utilization0())

1186:                 : int16(PositionBalance.wrap(positionBalanceArray[i][1]).utilization1());

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

389:             balances[k][0] = TokenId.unwrap(tokenId);

390:             balances[k][1] = PositionBalance.unwrap(s_positionBalance[c_user][tokenId]);

397:                     LeftRightUnsigned.wrap(balances[k][1]).rightSlot(),

836:             atTicks[0] = fastOracleTick;

837:             atTicks[1] = slowOracleTick;

838:             atTicks[2] = lastObservedTick;

839:             atTicks[3] = currentTick;

842:             atTicks[0] = fastOracleTick;

953:             atTicks[0] = fastOracleTick;

954:             atTicks[1] = twapTick;

955:             atTicks[2] = lastObservedTick;

956:             atTicks[3] = currentTick;

1088:         touchedId[0].validateIsExercisable(twapTick);

1094:             uint128 positionSize = s_positionBalance[account][touchedId[0]].positionSize();

1097:                 touchedId[0],

1106:                 touchedId[0],

1146:         emit ForcedExercised(msg.sender, account, touchedId[0], exerciseFees);

1466:                 (premiumAccumulatorsByLeg[leg][0], premiumAccumulatorsByLeg[leg][1]) = SFPM

1485:                                     ((premiumAccumulatorsByLeg[leg][0] -

1494:                                     ((premiumAccumulatorsByLeg[leg][1] -

1727:             uint256 accumulated0 = ((premiumAccumulators[0] - grossPremiumLast.rightSlot()) *

1729:             uint256 accumulated1 = ((premiumAccumulators[1] - grossPremiumLast.leftSlot()) *

1909:                                                     _premiumAccumulatorsByLeg[_leg][0] *

1925:                                                     _premiumAccumulatorsByLeg[_leg][1] *

1934:                             .wrap(uint128(premiumAccumulatorsByLeg[_leg][0]))

1935:                             .toLeftSlot(uint128(premiumAccumulatorsByLeg[_leg][1]));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

250:             return (int24(Math.sort(ticks)[cardinality / 2]), int24(ticks[0]));

361:             return int24(sortedTicks[9]);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="NC-3"></a>[NC-3] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

Solidity version 0.8.12 introduces `string.concat()` (vs `abi.encodePacked(<str>,<str>), which catches concatenation errors (in the event of a`bytes`data mixed in the concatenation)`)

*Instances (17)*:

```solidity
File: contracts/PanopticFactory.sol

194:             abi.encodePacked(

276:                 abi.encodePacked(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

408:                             abi.encodePacked(

1579:                 abi.encodePacked(

1618:                 abi.encodePacked(tokenId.strike(leg), tokenId.width(leg), tokenId.tokenType(leg))

1820:                 abi.encodePacked(tokenId.strike(leg), tokenId.width(leg), tokenId.tokenType(leg))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

542:                 abi.encodePacked(

551:                 abi.encodePacked(

820:             abi.encodePacked(

979:                     abi.encodePacked(

1255:             keccak256(abi.encodePacked(univ3pool, owner, tokenType, tickLower, tickUpper))

1283:             abi.encodePacked(univ3pool, owner, tokenType, tickLower, tickUpper)

1366:             keccak256(abi.encodePacked(univ3pool, owner, tokenType, tickLower, tickUpper))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

72:                 abi.encodePacked(

76:                             abi.encodePacked(

78:                                 abi.encodePacked(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

992:                             abi.encodePacked(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="NC-4"></a>[NC-4] `constant`s should be defined rather than using magic numbers

Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (292)*:

```solidity
File: contracts/CollateralTracker.sol

224:         totalSupply = 10 ** 6;

689:                                 2

775:                           |                  max ratio = 100%

776:                    100% - |                _------

779:                     20% - |---------¯

782:                                    50%    90% 100%     UTILIZATION

790:                 min_sell_ratio /= 2;

838:                  |   buy_ratio = 10%

839:            10% - |----------__       min_ratio = 5%

840:            5%  - | . . . . .  ¯¯¯--______

843:                           50%    90% 100%      UTILIZATION

855:                 return BUYER_COLLATERAL_RATIO / 2;

863:                     (SATURATED_POOL_UTIL - TARGET_POOL_UTIL)) / 2; // do the division by 2 at the end after all addition and multiplication; b/c y1 = buyCollateralRatio / 2

1110:                     DECIMALS * 100

1341:                                     Short put BPR = 100% - (price/strike) + SCR

1349:                            100% + SCR% - |--__           .    .    .

1350:                                   100% - | . .¯¯--__     .    .    .

1574:                     Put side of a short strangle, BPR = 100% - (100% - SCR/2)*(price/strike)

1580:                   100% - |--__                .

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

195:                 uint80(uint160(msg.sender) >> 80),

196:                 uint80(uint160(address(v3Pool)) >> 80),

277:                     uint80(uint160(deployerAddress) >> 80),

278:                     uint80(uint160(v3Pool) >> 80),

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

282:                 (uint256(block.timestamp) << 216) +

286:                 (uint256(uint24(currentTick)) << 24) + // add to slot 4

661:             return uint32(10_000 + (10_000 << 16));

698:             return utilization0 + (utilization1 << 16);

830:             int256(fastOracleTick - slowOracleTick) ** 2 +

831:                 int256(lastObservedTick - slowOracleTick) ** 2 +

832:                 int256(currentTick - slowOracleTick) ** 2 >

833:             MAX_TICKS_DELTA ** 2

1246:         return balanceCross >= Math.mulDivRoundingUp(thresholdCross, buffer, 10_000);

1258:                 uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1321:         if ((newHash >> 248) > MAX_POSITIONS) revert Errors.TooManyPositionsOpen();

1373:         return s_positionsHash[user] >> 248;

1423:             effectiveLiquidityFactorX32 = (uint256(removedLiquidity) * 2 ** 32) / netLiquidity;

1487:                                         (liquidityChunk.liquidity())) / 2 ** 64

1496:                                         (liquidityChunk.liquidity())) / 2 ** 64

1531:         if (tokenId.isLong(legIndex) == 0 || legIndex > 3) revert Errors.NotALongLeg();

1571:                 .toRightSlot(int128(int256((accumulatedPremium.rightSlot() * liquidity) / 2 ** 64)))

1572:                 .toLeftSlot(int128(int256((accumulatedPremium.leftSlot() * liquidity) / 2 ** 64)));

1728:                 totalLiquidity) / 2 ** 64;

1730:                 totalLiquidity) / 2 ** 64;

1911:                                                 )) + int256(legPremia.rightSlot() * 2 ** 64),

1927:                                                 )) + int256(legPremia.leftSlot()) * 2 ** 64,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

215:                                       = feeGrowthX128 * T * (1 + ν*R^2/(N*T))                (Eqn 2)

236:              s_accountPremiumOwed += feesCollected * T/N^2 * (1 - R/T + ν*R/T)          (Eqn 3)     

252:         However, since we require that Eqn 2 holds up-- ie. the gross fees collected should be equal

256:             s_accountPremiumGross += feesCollected * T/N^2 * (1 - R/T + ν*R^2/T^2)       (Eqn 4) 

265:                                 = ∆feeGrowthX128 * t * (1  + ν*R^2/(N*T))   (same as Eqn 2)

267:         where the last expression matches Eqn 2 exactly.

335:             s_AddrToPoolIdData[univ3pool] = uint256(poolId) + 2 ** 255;

778:         if (amount0 > uint128(type(int128).max - 4) || amount1 > uint128(type(int128).max - 4))

1178:                     totalLiquidity * 2 ** 64,

1179:                     netLiquidity ** 2

1183:                     totalLiquidity * 2 ** 64,

1184:                     netLiquidity ** 2

1193:                     uint256 numerator = netLiquidity + (removedLiquidity / 2 ** VEGOID);

1213:                     uint256 numerator = totalLiquidity ** 2 -

1216:                         ((removedLiquidity ** 2) / 2 ** (VEGOID));

1219:                         .mulDiv(premium0X64_base, numerator, totalLiquidity ** 2)

1222:                         .mulDiv(premium1X64_base, numerator, totalLiquidity ** 2)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

79:                                     LibString.toHexString(uint256(uint160(panopticPool)), 20),

126:             rarity < 18 ? rarity / 3 : rarity < 23 ? 23 - rarity : 0

139:                 metadata[bytes32("descriptions")][lastCharVal + 16 * (rarity / 8)]

161:             .replace("<!-- POOLADDRESS -->", LibString.toHexString(uint160(panopticPool), 20))

181:         } else if (block.chainid == 56) {

183:         } else if (block.chainid == 42161) {

185:         } else if (block.chainid == 8453) {

187:         } else if (block.chainid == 43114) {

189:         } else if (block.chainid == 137) {

191:         } else if (block.chainid == 10) {

193:         } else if (block.chainid == 42220) {

195:         } else if (block.chainid == 238) {

241:             if (_scale > 99) {

254:             LibString.toString(offset / 2),

266:         if (rarity < 3) {

267:             width = 1600;

268:         } else if (rarity < 9) {

269:             width = 1350;

270:         } else if (rarity < 12) {

271:             width = 1450;

272:         } else if (rarity < 15) {

273:             width = 1350;

274:         } else if (rarity < 19) {

275:             width = 1250;

276:         } else if (rarity < 20) {

277:             width = 1350;

278:         } else if (rarity < 21) {

279:             width = 1450;

280:         } else if (rarity < 23) {

281:             width = 1350;

282:         } else if (rarity >= 23) {

283:             width = 1600;

292:         if (rarity < 3) {

293:             width = 210;

294:         } else if (rarity < 6) {

295:             width = 220;

296:         } else if (rarity < 9) {

297:             width = 210;

298:         } else if (rarity < 12) {

299:             width = 220;

300:         } else if (rarity < 15) {

301:             width = 260;

302:         } else if (rarity < 19) {

303:             width = 225;

304:         } else if (rarity < 20) {

305:             width = 260;

306:         } else if (rarity < 21) {

307:             width = 220;

308:         } else if (rarity < 22) {

309:             width = 210;

310:         } else if (rarity < 23) {

311:             width = 220;

312:         } else if (rarity >= 23) {

313:             width = 210;

322:         if (rarity < 6) {

323:             width = 9000;

324:         } else if (rarity <= 22) {

325:             width = 3900;

326:         } else if (rarity > 22) {

327:             width = 9000;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/Multicall.sol

26:                     revert(add(result, 32), mload(result))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/Constants.sol

22:         1461446703485210103287273052203988822378723970342;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Constants.sol)

```solidity
File: contracts/libraries/Math.sol

94:                 x >>= 128;

95:                 r += 32;

98:                 x >>= 64;

99:                 r += 16;

102:                 x >>= 32;

103:                 r += 8;

106:                 x >>= 16;

107:                 r += 4;

110:                 x >>= 8;

111:                 r += 2;

142:             if (absTick & 0x2 != 0) sqrtR = (sqrtR * 0xfff97272373d413259a46990580e213a) >> 128;

144:             if (absTick & 0x4 != 0) sqrtR = (sqrtR * 0xfff2e50f5f656932ef12357cf3c7fdcc) >> 128;

146:             if (absTick & 0x8 != 0) sqrtR = (sqrtR * 0xffe5caca7e10e4e61c3624eaa0941cd0) >> 128;

148:             if (absTick & 0x10 != 0) sqrtR = (sqrtR * 0xffcb9843d60f6159c9db58835c926644) >> 128;

150:             if (absTick & 0x20 != 0) sqrtR = (sqrtR * 0xff973b41fa98c081472e6896dfb254c0) >> 128;

152:             if (absTick & 0x40 != 0) sqrtR = (sqrtR * 0xff2ea16466c96a3843ec78b326b52861) >> 128;

154:             if (absTick & 0x80 != 0) sqrtR = (sqrtR * 0xfe5dee046a99a2a811c461f1969c3053) >> 128;

156:             if (absTick & 0x100 != 0) sqrtR = (sqrtR * 0xfcbe86c7900a88aedcffc83b479aa3a4) >> 128;

158:             if (absTick & 0x200 != 0) sqrtR = (sqrtR * 0xf987a7253ac413176f2b074cf7815e54) >> 128;

160:             if (absTick & 0x400 != 0) sqrtR = (sqrtR * 0xf3392b0822b70005940c7a398e4b70f3) >> 128;

162:             if (absTick & 0x800 != 0) sqrtR = (sqrtR * 0xe7159475a2c29b7443b29c7fa6e889d9) >> 128;

164:             if (absTick & 0x1000 != 0) sqrtR = (sqrtR * 0xd097f3bdfd2022b8845ad8f792aa5825) >> 128;

166:             if (absTick & 0x2000 != 0) sqrtR = (sqrtR * 0xa9f746462d870fdf8a65dc1f90e061e5) >> 128;

168:             if (absTick & 0x4000 != 0) sqrtR = (sqrtR * 0x70d869a156d2a1b890bb3df62baf32f7) >> 128;

170:             if (absTick & 0x8000 != 0) sqrtR = (sqrtR * 0x31be135f97d08fd981231505542fcfa6) >> 128;

172:             if (absTick & 0x10000 != 0) sqrtR = (sqrtR * 0x9aa508b5b7a84e1c677de54f3e99bc9) >> 128;

174:             if (absTick & 0x20000 != 0) sqrtR = (sqrtR * 0x5d6af8dedb81196699c329225ee604) >> 128;

176:             if (absTick & 0x40000 != 0) sqrtR = (sqrtR * 0x2216e584f5fa1ea926041bedfe98) >> 128;

178:             if (absTick & 0x80000 != 0) sqrtR = (sqrtR * 0x48a170391f7dc42444e8fa2) >> 128;

184:             return uint160((sqrtR >> 32) + (sqrtR % (1 << 32) == 0 ? 0 : 1));

202:                     uint256(liquidityChunk.liquidity()) << 96,

419:             uint256 inv = (3 * denominator) ^ 2;

423:             inv *= 2 - denominator * inv; // inverse mod 2**8

424:             inv *= 2 - denominator * inv; // inverse mod 2**16

425:             inv *= 2 - denominator * inv; // inverse mod 2**32

426:             inv *= 2 - denominator * inv; // inverse mod 2**64

427:             inv *= 2 - denominator * inv; // inverse mod 2**128

428:             inv *= 2 - denominator * inv; // inverse mod 2**256

517:             uint256 inv = (3 * denominator) ^ 2;

521:             inv *= 2 - denominator * inv; // inverse mod 2**8

522:             inv *= 2 - denominator * inv; // inverse mod 2**16

523:             inv *= 2 - denominator * inv; // inverse mod 2**32

524:             inv *= 2 - denominator * inv; // inverse mod 2**64

525:             inv *= 2 - denominator * inv; // inverse mod 2**128

526:             inv *= 2 - denominator * inv; // inverse mod 2**256

587:             require(2 ** 64 > prod1);

614:             prod0 |= prod1 * 2 ** 192;

650:             require(2 ** 96 > prod1);

677:             prod0 |= prod1 * 2 ** 160;

690:             if (mulmod(a, b, 2 ** 96) > 0) {

727:             require(2 ** 128 > prod1);

754:             prod0 |= prod1 * 2 ** 128;

767:             if (mulmod(a, b, 2 ** 128) > 0) {

804:             require(2 ** 192 > prod1);

831:             prod0 |= prod1 * 2 ** 64;

844:             if (mulmod(a, b, 2 ** 192) > 0) {

875:             int256 pivot = arr[uint256(left + (right - left) / 2)];

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

52:             uint64 poolId = uint64(uint160(univ3pool) >> 112);

53:             poolId += uint64(uint24(tickSpacing)) << 48;

78:             return addr == address(0) ? 40 : 39 - Math.mostSignificantNibble(uint160(addr));

102:                 Strings.toString(fee / 100),

103:                 fee % 100 == 0

107:                         Strings.toString((fee / 10) % 10),

108:                         Strings.toString(fee % 10)

139:             ? uint8(existingHash >> 248) + 1

140:             : uint8(existingHash >> 248) - 1;

143:             return uint256(updatedHash) + (newPositionCount << 248);

250:             return (int24(Math.sort(ticks)[cardinality / 2]), int24(ticks[0]));

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

275:                 2;

278:             if (block.timestamp >= uint256(uint40(medianData >> 216)) + period) {

295:                 uint24 orderMap = uint24(medianData >> 192);

302:                 for (uint8 i; i < 8; ++i) {

304:                     rank = (orderMap >> (3 * i)) % 8;

306:                     if (rank == 7) {

312:                     entry = int24(uint24(medianData >> (rank * 24)));

322:                     (block.timestamp << 216) +

323:                     (uint256(newOrderMap) << 192) +

324:                     uint256(uint192(medianData << 24)) +

343:             for (uint256 i = 0; i < 20; ++i) {

344:                 secondsAgos[i] = uint32(((i + 1) * twapWindow) / 20);

351:             for (uint256 i = 0; i < 19; ++i) {

353:                     (tickCumulatives[i] - tickCumulatives[i + 1]) / int56(uint56(twapWindow / 20))

471:             (width * tickSpacing) / 2,

472:             int24(int256(Math.unsafeDivRoundingUp(uint24(width) * uint24(tickSpacing), 2)))

517:                 return Math.mulDiv192(amount, uint256(sqrtPriceX96) ** 2);

537:                 return Math.mulDiv192RoundingUp(amount, uint256(sqrtPriceX96) ** 2);

554:                 return Math.mulDiv(amount, 2 ** 192, uint256(sqrtPriceX96) ** 2);

556:                 return Math.mulDiv(amount, 2 ** 128, Math.mulDiv64(sqrtPriceX96, sqrtPriceX96));

574:                 return Math.mulDivRoundingUp(amount, 2 ** 192, uint256(sqrtPriceX96) ** 2);

579:                         2 ** 128,

597:                     .mulDiv192(Math.absUint(amount), uint256(sqrtPriceX96) ** 2)

620:                     .mulDiv(Math.absUint(amount), 2 ** 192, uint256(sqrtPriceX96) ** 2)

627:                         2 ** 128,

693:             amount0 = Math.mulDivRoundingUp(amount1, 2 ** 96, geometricMeanPriceX96).toUint128();

767:                 uint256 bonusCross = Math.min(balanceCross / 2, thresholdCross - balanceCross);

774:                         2 ** 128,

782:                             Math.mulDiv128(bonusCross, 2 ** 128 - requiredRatioX128),

790:                         2 ** 128,

798:                             Math.mulDiv128(bonusCross, 2 ** 128 - requiredRatioX128),

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

37:                 or(and(eq(mload(0), 1), gt(returndatasize(), 31)), iszero(returndatasize())),

41:                 call(gas(), token, 0, p, 100, 0, 32)

67:                 or(and(eq(mload(0), 1), gt(returndatasize(), 31)), iszero(returndatasize())),

71:                 call(gas(), token, 0, p, 68, 0, 32)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/types/LeftRight.sol

101:         return uint128(LeftRightUnsigned.unwrap(self) >> 128);

108:         return int128(LeftRightSigned.unwrap(self) >> 128);

125:             return LeftRightUnsigned.wrap(LeftRightUnsigned.unwrap(self) + (uint256(left) << 128));

135:             return LeftRightSigned.wrap(LeftRightSigned.unwrap(self) + (int256(left) << 128));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

78:                     (uint256(uint24(_tickLower)) << 232) +

79:                         (uint256(uint24(_tickUpper)) << 208) +

109:                     LiquidityChunk.unwrap(self) + (uint256(uint24(_tickLower)) << 232)

126:                     LiquidityChunk.unwrap(self) + ((uint256(uint24(_tickUpper))) << 208)

173:             return int24(int256(LiquidityChunk.unwrap(self) >> 232));

182:             return int24(int256(LiquidityChunk.unwrap(self) >> 208));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

23:             Pointer.wrap((uint256(_size) << 208) + (uint256(_start) << 160) + uint160(_location));

37:         return uint48(Pointer.unwrap(self) >> 160);

44:         return uint48(Pointer.unwrap(self) >> 208);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

50:                     (uint256(_tickData) << 160) +

51:                         (uint256(_utilizations) << 128) +

72:                 (uint96(uint24(_fastOracleTick)) << 24) +

73:                 (uint96(uint24(_slowOracleTick)) << 48) +

74:                 (uint96(uint24(_lastObservedTick)) << 72);

87:             return int24(int256(PositionBalance.unwrap(self) >> 232));

96:             return int24(int256(PositionBalance.unwrap(self) >> 208));

105:             return int24(int256(PositionBalance.unwrap(self) >> 184));

114:             return int24(int256(PositionBalance.unwrap(self) >> 160));

123:             return uint96(PositionBalance.unwrap(self) >> 160);

134:         PositionBalance self = PositionBalance.wrap(uint256(_tickData) << 160);

148:             return int256((PositionBalance.unwrap(self) >> 128) % 2 ** 16);

157:             return int256((PositionBalance.unwrap(self) >> 144) % 2 ** 16);

166:             return uint32(PositionBalance.unwrap(self) >> 128);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

98:             return int24(uint24((TokenId.unwrap(self) >> 48) % 2 ** 16));

110:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48)) % 2);

120:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 1)) % 128);

130:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 8)) % 2);

140:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 9)) % 2);

150:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 10)) % 4);

160:             return int24(int256(TokenId.unwrap(self) >> (64 + legIndex * 48 + 12)));

171:             return int24(int256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 36)) % 4096));

195:             return TokenId.wrap(TokenId.unwrap(self) + (uint256(uint24(_tickSpacing)) << 48));

212:                 TokenId.wrap(TokenId.unwrap(self) + (uint256(_asset % 2) << (64 + legIndex * 48)));

229:                     TokenId.unwrap(self) + (uint256(_optionRatio % 128) << (64 + legIndex * 48 + 1))

246:             return TokenId.wrap(TokenId.unwrap(self) + ((_isLong % 2) << (64 + legIndex * 48 + 8)));

263:                     TokenId.unwrap(self) + (uint256(_tokenType % 2) << (64 + legIndex * 48 + 9))

281:                     TokenId.unwrap(self) + (uint256(_riskPartner % 4) << (64 + legIndex * 48 + 10))

300:                         uint256((int256(_strike) & BITMASK_INT24) << (64 + legIndex * 48 + 12))

320:                         (uint256(uint24(_width) % 4096) << (64 + legIndex * 48 + 36))

376:             if (optionRatios < 2 ** 64) {

378:             } else if (optionRatios < 2 ** 112) {

380:             } else if (optionRatios < 2 ** 160) {

381:                 optionRatios = 2;

382:             } else if (optionRatios < 2 ** 208) {

383:                 optionRatios = 3;

385:                 optionRatios = 4;

439:         if (optionRatios < 2 ** 64) {

441:         } else if (optionRatios < 2 ** 112) {

443:         } else if (optionRatios < 2 ** 160) {

444:             return 2;

445:         } else if (optionRatios < 2 ** 208) {

446:             return 3;

448:         return 4;

477:         if (i == 2)

483:         if (i == 3)

506:             uint256 chunkData = (TokenId.unwrap(self) & CHUNK_MASK) >> 64;

507:             for (uint256 i = 0; i < 4; ++i) {

512:                     if ((TokenId.unwrap(self) >> (64 + 48 * i)) != 0)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-5"></a>[NC-5] Control structures do not follow the Solidity Style Guide

See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (143)*:

```solidity
File: contracts/CollateralTracker.sol

169:         if (msg.sender != address(s_panopticPool)) revert Errors.NotPanopticPool();

219:         if (s_initialized) revert Errors.CollateralTokenAlreadyInitialized();

316:         if (s_panopticPool.numberOfPositions(msg.sender) != 0) revert Errors.PositionCountNotZero();

335:         if (s_panopticPool.numberOfPositions(from) != 0) revert Errors.PositionCountNotZero();

403:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

461:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

498:         uint256 supply = totalSupply; // Saves an extra SLOAD if totalSupply is non-zero.

515:         if (assets > maxWithdraw(owner)) revert Errors.ExceedsMaximumRedemption();

523:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

565:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

616:         if (shares > maxRedeem(owner)) revert Errors.ExceedsMaximumRedemption();

622:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

682:                 if (positionId.isLong(leg) == 0) continue;

868:           LIFECYCLE OF A COLLATERAL TOKEN AND DELEGATE/REVOKE LOGIC

1227:                 if (tokenId.tokenType(index) != (underlyingIsToken0 ? 0 : 1)) continue;

1308:                 if (

1336:                     if (

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

7: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

63:     SemiFungiblePositionManager internal immutable SFPM;

107:         SemiFungiblePositionManager _SFPM,

140:         if (amount0Owed > 0)

147:         if (amount1Owed > 0)

183:         if (address(v3Pool) == address(0)) revert Errors.UniswapPoolNotInitialized();

185:         if (address(s_getPanopticPool[v3Pool]) != address(0))

229:         if (amount0 > amount0Max || amount1 > amount1Max) revert Errors.PriceBoundFail();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

6: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

149:     SemiFungiblePositionManager internal immutable SFPM;

272:         if (address(s_univ3pool) != address(0)) revert Errors.PoolAlreadyInitialized();

313:         if (

471:         if (medianData != 0) s_miniMedian = medianData;

517:         if (medianData != 0) s_miniMedian = medianData;

542:         if (medianData != 0) s_miniMedian = medianData;

573:         if (tokenId.poolId() != SFPM.getPoolId(address(s_univ3pool)))

578:         if (PositionBalance.unwrap(s_positionBalance[msg.sender][tokenId]) != 0)

606:         if (medianData != 0) s_miniMedian = medianData;

829:         if (

947:                 if (Math.abs(currentTick - twapTick) > MAX_TWAP_DELTA_LIQUIDATION)

1080:         if (touchedId.length != 1) revert Errors.InputListFail();

1143:         if (positionIdListExercisor.length > 0)

1204:         if (expectedSolvent && solvent != numberOfTicks) revert Errors.AccountInsolvent();

1205:         if (!expectedSolvent && solvent != 0) revert Errors.NotMarginCalled();

1300:         if (fingerprintIncomingList != currentHash) revert Errors.InputListFail();

1321:         if ((newHash >> 248) > MAX_POSITIONS) revert Errors.TooManyPositionsOpen();

1419:         if (netLiquidity == 0 && removedLiquidity == 0) return totalLiquidity;

1428:         if (effectiveLiquidityFactorX32 > uint256(effectiveLiquidityLimitX32))

1531:         if (tokenId.isLong(legIndex) == 0 || legIndex > 3) revert Errors.NotALongLeg();

1828:                 if (commitLongSettled)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

145:         We're tracking the amount of net and removed liquidity for the specific region:

174:         time, we call this the gross premia. If that liquidity has been removed, we also need to

197:         In addition to tracking, we also want to track those fees plus a small spread. Specifically,

274:         specific risk management profile of every smart contract. And simply setting the ν parameter

310:         if (univ3pool == address(0)) revert Errors.UniswapPoolNotInitialized();

316:         if (s_AddrToPoolIdData[univ3pool] != 0) return;

360:         if (amount0Owed > 0)

367:         if (amount1Owed > 0)

563:             if (

686:             if (swapAmount == 0) return LeftRightSigned.wrap(0);

727:         if (univ3pool == IUniswapV3Pool(address(0))) revert Errors.UniswapPoolNotInitialized();

778:         if (amount0 > uint128(type(int128).max - 4) || amount1 > uint128(type(int128).max - 4))

793:         if ((currentTick >= tickLimitHigh) || (currentTick <= tickLimitLow))

846:             if (chunkLiquidity == 0) revert Errors.ZeroLiquidity();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

36:         if (factory.getPool(features.token0, features.token1, features.fee) != sender)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

8: import {SemiFungiblePositionManager} from "@contracts/SemiFungiblePositionManager.sol";

25:         SemiFungiblePositionManager sfpm,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/Math.sol

131:             if (absTick > uint256(int256(Constants.MAX_V3POOL_TICK))) revert Errors.InvalidTick();

142:             if (absTick & 0x2 != 0) sqrtR = (sqrtR * 0xfff97272373d413259a46990580e213a) >> 128;

144:             if (absTick & 0x4 != 0) sqrtR = (sqrtR * 0xfff2e50f5f656932ef12357cf3c7fdcc) >> 128;

146:             if (absTick & 0x8 != 0) sqrtR = (sqrtR * 0xffe5caca7e10e4e61c3624eaa0941cd0) >> 128;

148:             if (absTick & 0x10 != 0) sqrtR = (sqrtR * 0xffcb9843d60f6159c9db58835c926644) >> 128;

150:             if (absTick & 0x20 != 0) sqrtR = (sqrtR * 0xff973b41fa98c081472e6896dfb254c0) >> 128;

152:             if (absTick & 0x40 != 0) sqrtR = (sqrtR * 0xff2ea16466c96a3843ec78b326b52861) >> 128;

154:             if (absTick & 0x80 != 0) sqrtR = (sqrtR * 0xfe5dee046a99a2a811c461f1969c3053) >> 128;

156:             if (absTick & 0x100 != 0) sqrtR = (sqrtR * 0xfcbe86c7900a88aedcffc83b479aa3a4) >> 128;

158:             if (absTick & 0x200 != 0) sqrtR = (sqrtR * 0xf987a7253ac413176f2b074cf7815e54) >> 128;

160:             if (absTick & 0x400 != 0) sqrtR = (sqrtR * 0xf3392b0822b70005940c7a398e4b70f3) >> 128;

162:             if (absTick & 0x800 != 0) sqrtR = (sqrtR * 0xe7159475a2c29b7443b29c7fa6e889d9) >> 128;

164:             if (absTick & 0x1000 != 0) sqrtR = (sqrtR * 0xd097f3bdfd2022b8845ad8f792aa5825) >> 128;

166:             if (absTick & 0x2000 != 0) sqrtR = (sqrtR * 0xa9f746462d870fdf8a65dc1f90e061e5) >> 128;

168:             if (absTick & 0x4000 != 0) sqrtR = (sqrtR * 0x70d869a156d2a1b890bb3df62baf32f7) >> 128;

170:             if (absTick & 0x8000 != 0) sqrtR = (sqrtR * 0x31be135f97d08fd981231505542fcfa6) >> 128;

172:             if (absTick & 0x10000 != 0) sqrtR = (sqrtR * 0x9aa508b5b7a84e1c677de54f3e99bc9) >> 128;

174:             if (absTick & 0x20000 != 0) sqrtR = (sqrtR * 0x5d6af8dedb81196699c329225ee604) >> 128;

176:             if (absTick & 0x40000 != 0) sqrtR = (sqrtR * 0x2216e584f5fa1ea926041bedfe98) >> 128;

178:             if (absTick & 0x80000 != 0) sqrtR = (sqrtR * 0x48a170391f7dc42444e8fa2) >> 128;

181:             if (tick > 0) sqrtR = type(uint256).max / sqrtR;

302:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) revert Errors.CastingError();

317:         if ((downcastedInt = int128(toCast)) < 0) revert Errors.CastingError();

324:         if (!((downcastedInt = int128(toCast)) == toCast)) revert Errors.CastingError();

331:         if (toCast > uint256(type(int256).max)) revert Errors.CastingError();

356:             uint256 prod0; // Least significant 256 bits of the product

357:             uint256 prod1; // Most significant 256 bits of the product

456:             uint256 prod0; // Least significant 256 bits of the product

457:             uint256 prod1; // Most significant 256 bits of the product

473:             if (denominator <= prod1) return type(uint256).max;

568:             uint256 prod0; // Least significant 256 bits of the product

569:             uint256 prod1; // Most significant 256 bits of the product

631:             uint256 prod0; // Least significant 256 bits of the product

632:             uint256 prod1; // Most significant 256 bits of the product

708:             uint256 prod0; // Least significant 256 bits of the product

709:             uint256 prod1; // Most significant 256 bits of the product

785:             uint256 prod0; // Least significant 256 bits of the product

786:             uint256 prod1; // Most significant 256 bits of the product

874:             if (i == j) return;

885:             if (left < j) quickSort(arr, left, j);

886:             if (i < right) quickSort(arr, i, right);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

78:             return addr == address(0) ? 40 : 39 - Math.mostSignificantNibble(uint160(addr));

298:                 uint24 shift = 1;

307:                         shift -= 1;

314:                         shift += 1;

318:                     newOrderMap = newOrderMap + ((rank + 1) << (3 * (i + shift - 1)));

451:             if (

910:             if (

934:             } else if (

969:                 if (haircut0 != 0) collateral0.exercise(_liquidatee, 0, 0, 0, int128(haircut0));

970:                 if (haircut1 != 0) collateral1.exercise(_liquidatee, 0, 0, 0, int128(haircut1));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

45:         if (!success) revert Errors.TransferFailed();

75:         if (!success) revert Errors.TransferFailed();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

101:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

113:             if (

137:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

164:             if (

223:             if (

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

84:         if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed - amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

```solidity
File: contracts/types/LeftRight.sol

159:             if (

182:             if (

198:             if (left128 != left) revert Errors.UnderOverFlow();

203:             if (right128 != right) revert Errors.UnderOverFlow();

221:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

239:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

261:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/TokenId.sol

465:         if (i == 0)

471:         if (i == 1)

477:         if (i == 2)

483:         if (i == 3)

501:         if (self.optionRatio(0) == 0) revert Errors.InvalidTokenIdParameter(1);

512:                     if ((TokenId.unwrap(self) >> (64 + 48 * i)) != 0)

528:                 if ((self.width(i) == 0)) revert Errors.InvalidTokenIdParameter(5);

530:                 if (

541:                     if (self.riskPartner(riskPartnerIndex) != i)

545:                     if (

560:                     if ((_isLong == isLongP) && (_tokenType == tokenTypeP))

566:                     if (((_isLong != isLongP) || _isLong == 1) && (_tokenType != tokenTypeP))

592:                     if (self.isLong(i) == 1) return; // validated

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-6"></a>[NC-6] Unused `error` definition

Note that there may be cases where an error superficially appears to be used, but this is only because there are multiple definitions of the error in different files. In such cases, the error definition should be moved into a separate file. The instances below are the unused definitions.

*Instances (29)*:

```solidity
File: contracts/libraries/Errors.sol

9:     error AccountInsolvent();

13:     error CastingError();

16:     error CollateralTokenAlreadyInitialized();

19:     error DepositTooLarge();

23:     error EffectiveLiquidityAboveThreshold();

26:     error ExceedsMaximumRedemption();

29:     error InputListFail();

32:     error InvalidTick();

35:     error InvalidNotionalValue();

39:     error InvalidTokenIdParameter(uint256 parameterType);

42:     error InvalidUniswapCallback();

45:     error LeftRightInputError();

48:     error NoLegsExercisable();

51:     error NotALongLeg();

54:     error NotEnoughLiquidity();

57:     error NotMarginCalled();

60:     error NotPanopticPool();

63:     error PoolAlreadyInitialized();

66:     error PositionAlreadyMinted();

69:     error PositionCountNotZero();

72:     error PositionTooLarge();

75:     error PriceBoundFail();

79:     error StaleTWAP();

82:     error TooManyPositionsOpen();

85:     error TransferFailed();

89:     error TicksNotInitializable();

92:     error UnderOverFlow();

95:     error UniswapPoolNotInitialized();

98:     error ZeroLiquidity();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Errors.sol)

### <a name="NC-7"></a>[NC-7] Events that mark critical parameter changes should contain both the old and the new value

This should especially be done if the new value is not required to be different from the old value

*Instances (2)*:

```solidity
File: contracts/PanopticPool.sol

1522:     function settleLongPremium(
              TokenId[] calldata positionIdList,
              address owner,
              uint256 legIndex
          ) external {
              _validatePositionList(owner, positionIdList, 0);
      
              TokenId tokenId = positionIdList[positionIdList.length - 1];
      
              if (tokenId.isLong(legIndex) == 0 || legIndex > 3) revert Errors.NotALongLeg();
      
              LiquidityChunk liquidityChunk = PanopticMath.getLiquidityChunk(
                  tokenId,
                  legIndex,
                  s_positionBalance[owner][tokenId].positionSize()
              );
      
              (, int24 currentTick, , , , , ) = s_univ3pool.slot0();
      
              LeftRightUnsigned accumulatedPremium;
              {
                  uint256 tokenType = tokenId.tokenType(legIndex);
                  (uint128 premiumAccumulator0, uint128 premiumAccumulator1) = SFPM.getAccountPremium(
                      address(s_univ3pool),
                      address(this),
                      tokenType,
                      liquidityChunk.tickLower(),
                      liquidityChunk.tickUpper(),
                      currentTick,
                      1
                  );
                  accumulatedPremium = LeftRightUnsigned.wrap(premiumAccumulator0).toLeftSlot(
                      premiumAccumulator1
                  );
      
                  // update the premium accumulator for the long position to the latest value
                  // (the entire premia delta will be settled)
                  LeftRightUnsigned premiumAccumulatorsLast = s_options[owner][tokenId][legIndex];
                  s_options[owner][tokenId][legIndex] = accumulatedPremium;
      
                  accumulatedPremium = accumulatedPremium.sub(premiumAccumulatorsLast);
              }
      
              unchecked {
                  uint256 liquidity = liquidityChunk.liquidity();
      
                  // update the realized premia
                  LeftRightSigned realizedPremia = LeftRightSigned
                      .wrap(0)
                      .toRightSlot(int128(int256((accumulatedPremium.rightSlot() * liquidity) / 2 ** 64)))
                      .toLeftSlot(int128(int256((accumulatedPremium.leftSlot() * liquidity) / 2 ** 64)));
      
                  // deduct the paid premium tokens from the owner's balance and add them to the cumulative settled token delta
                  s_collateralToken0.exercise(owner, 0, 0, 0, -realizedPremia.rightSlot());
                  s_collateralToken1.exercise(owner, 0, 0, 0, -realizedPremia.leftSlot());
      
                  bytes32 chunkKey = keccak256(
                      abi.encodePacked(
                          tokenId.strike(legIndex),
                          tokenId.width(legIndex),
                          tokenId.tokenType(legIndex)
                      )
                  );
                  // commit the delta in settled tokens (all of the premium paid by long chunks in the tokenIds list) to storage
                  s_settledTokens[chunkKey] = s_settledTokens[chunkKey].add(
                      LeftRightUnsigned.wrap(uint256(LeftRightSigned.unwrap(realizedPremia)))
                  );
      
                  emit PremiumSettled(owner, tokenId, realizedPremia);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

81:     function setApprovalForAll(address operator, bool approved) public {
            isApprovedForAll[msg.sender][operator] = approved;
    
            emit ApprovalForAll(msg.sender, operator, approved);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

### <a name="NC-8"></a>[NC-8] Function ordering does not follow the Solidity style guide

According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*Instances (6)*:

```solidity
File: contracts/CollateralTracker.sol

1: 
   Current order:
   external startToken
   external getPoolData
   external name
   external symbol
   external decimals
   public transfer
   public transferFrom
   external asset
   public totalAssets
   public convertToShares
   public convertToAssets
   external maxDeposit
   public previewDeposit
   external deposit
   external maxMint
   public previewMint
   external mint
   public maxWithdraw
   public previewWithdraw
   external withdraw
   external withdraw
   public maxRedeem
   public previewRedeem
   external redeem
   external exerciseCost
   internal _poolUtilization
   internal _sellCollateralRatio
   internal _buyCollateralRatio
   external delegate
   external revoke
   external settleLiquidation
   external refund
   external takeCommissionAddData
   external exercise
   internal _getExchangedAmount
   public getAccountMarginDetails
   internal _getTotalRequiredCollateral
   internal _getRequiredCollateralAtTickSinglePosition
   internal _getRequiredCollateralSingleLeg
   internal _getRequiredCollateralSingleLegNoPartner
   internal _getRequiredCollateralSingleLegPartner
   internal _getRequiredCollateralAtUtilization
   internal _computeSpread
   internal _computeStrangle
   
   Suggested order:
   external startToken
   external getPoolData
   external name
   external symbol
   external decimals
   external asset
   external maxDeposit
   external deposit
   external maxMint
   external mint
   external withdraw
   external withdraw
   external redeem
   external exerciseCost
   external delegate
   external revoke
   external settleLiquidation
   external refund
   external takeCommissionAddData
   external exercise
   public transfer
   public transferFrom
   public totalAssets
   public convertToShares
   public convertToAssets
   public previewDeposit
   public previewMint
   public maxWithdraw
   public previewWithdraw
   public maxRedeem
   public previewRedeem
   public getAccountMarginDetails
   internal _poolUtilization
   internal _sellCollateralRatio
   internal _buyCollateralRatio
   internal _getExchangedAmount
   internal _getTotalRequiredCollateral
   internal _getRequiredCollateralAtTickSinglePosition
   internal _getRequiredCollateralSingleLeg
   internal _getRequiredCollateralSingleLegNoPartner
   internal _getRequiredCollateralSingleLegPartner
   internal _getRequiredCollateralAtUtilization
   internal _computeSpread
   internal _computeStrangle

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

1: 
   Current order:
   external uniswapV3MintCallback
   external deployNewPool
   external minePoolAddress
   internal _mintFullRange
   external getPanopticPool
   
   Suggested order:
   external uniswapV3MintCallback
   external deployNewPool
   external minePoolAddress
   external getPanopticPool
   internal _mintFullRange

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

1: 
   Current order:
   external startPool
   external assertMinCollateralValues
   external validateCollateralWithdrawable
   external calculateAccumulatedFeesBatch
   internal _calculateAccumulatedPremia
   external pokeMedian
   external mintOptions
   external burnOptions
   external burnOptions
   internal _mintOptions
   internal _mintInSFPMAndUpdateCollateral
   internal _payCommissionAndWriteData
   internal _burnAllOptionsFrom
   internal _burnOptions
   internal _validateSolvency
   internal _checkSolvency
   internal _burnAndHandleExercise
   external liquidate
   external forceExercise
   internal _checkSolvencyAtTicks
   internal _isAccountSolvent
   public isSafeMode
   internal _validatePositionList
   internal _updatePositionsHash
   external univ3pool
   external collateralToken0
   external collateralToken1
   external getOracleTicks
   external numberOfPositions
   external positionData
   internal getUniV3TWAP
   internal _checkLiquiditySpread
   internal _getPremia
   external settleLongPremium
   internal _updateSettlementPostMint
   internal _getAvailablePremium
   internal _getLiquidities
   internal _updateSettlementPostBurn
   
   Suggested order:
   external startPool
   external assertMinCollateralValues
   external validateCollateralWithdrawable
   external calculateAccumulatedFeesBatch
   external pokeMedian
   external mintOptions
   external burnOptions
   external burnOptions
   external liquidate
   external forceExercise
   external univ3pool
   external collateralToken0
   external collateralToken1
   external getOracleTicks
   external numberOfPositions
   external positionData
   external settleLongPremium
   public isSafeMode
   internal _calculateAccumulatedPremia
   internal _mintOptions
   internal _mintInSFPMAndUpdateCollateral
   internal _payCommissionAndWriteData
   internal _burnAllOptionsFrom
   internal _burnOptions
   internal _validateSolvency
   internal _checkSolvency
   internal _burnAndHandleExercise
   internal _checkSolvencyAtTicks
   internal _isAccountSolvent
   internal _validatePositionList
   internal _updatePositionsHash
   internal getUniV3TWAP
   internal _checkLiquiditySpread
   internal _getPremia
   internal _updateSettlementPostMint
   internal _getAvailablePremium
   internal _getLiquidities
   internal _updateSettlementPostBurn

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

1: 
   Current order:
   external initializeAMMPool
   external uniswapV3MintCallback
   external uniswapV3SwapCallback
   external burnTokenizedPosition
   external mintTokenizedPosition
   public safeTransferFrom
   public safeBatchTransferFrom
   internal registerTokenTransfer
   internal swapInAMM
   internal _createPositionInAMM
   internal _createLegInAMM
   private _updateStoredPremia
   private _getFeesBase
   internal _mintLiquidity
   internal _burnLiquidity
   internal _collectAndWritePositionData
   private _getPremiaDeltas
   external getAccountLiquidity
   external getAccountPremium
   external getAccountFeesBase
   external getUniswapV3PoolFromId
   external getPoolId
   
   Suggested order:
   external initializeAMMPool
   external uniswapV3MintCallback
   external uniswapV3SwapCallback
   external burnTokenizedPosition
   external mintTokenizedPosition
   external getAccountLiquidity
   external getAccountPremium
   external getAccountFeesBase
   external getUniswapV3PoolFromId
   external getPoolId
   public safeTransferFrom
   public safeBatchTransferFrom
   internal registerTokenTransfer
   internal swapInAMM
   internal _createPositionInAMM
   internal _createLegInAMM
   internal _mintLiquidity
   internal _burnLiquidity
   internal _collectAndWritePositionData
   private _updateStoredPremia
   private _getFeesBase
   private _getPremiaDeltas

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

1: 
   Current order:
   internal getPoolId
   internal incrementPoolPattern
   external numberOfLeadingHexZeros
   external safeERC20Symbol
   internal uniswapFeeToString
   internal updatePositionsHash
   external getOracleTicks
   internal computeMedianObservedPrice
   public computeInternalMedian
   external twapFilter
   internal getLiquidityChunk
   internal getTicks
   internal getRangesFromStrike
   internal computeExercisedAmounts
   internal convert0to1
   internal convert0to1RoundingUp
   internal convert1to0
   internal convert1to0RoundingUp
   internal convert0to1
   internal convert1to0
   internal getCrossBalances
   internal getAmountsMoved
   internal _calculateIOAmounts
   external getLiquidationBonus
   external haircutPremia
   external getExerciseDeltas
   
   Suggested order:
   external numberOfLeadingHexZeros
   external safeERC20Symbol
   external getOracleTicks
   external twapFilter
   external getLiquidationBonus
   external haircutPremia
   external getExerciseDeltas
   public computeInternalMedian
   internal getPoolId
   internal incrementPoolPattern
   internal uniswapFeeToString
   internal updatePositionsHash
   internal computeMedianObservedPrice
   internal getLiquidityChunk
   internal getTicks
   internal getRangesFromStrike
   internal computeExercisedAmounts
   internal convert0to1
   internal convert0to1RoundingUp
   internal convert1to0
   internal convert1to0RoundingUp
   internal convert0to1
   internal convert1to0
   internal getCrossBalances
   internal getAmountsMoved
   internal _calculateIOAmounts

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/types/PositionBalance.sol

1: 
   Current order:
   internal storeBalanceData
   internal packTickData
   internal lastObservedTick
   internal slowOracleTick
   internal fastOracleTick
   internal currentTick
   internal tickData
   internal unpackTickData
   internal utilization0
   internal utilization1
   internal utilizations
   internal positionSize
   external unpackAll
   
   Suggested order:
   external unpackAll
   internal storeBalanceData
   internal packTickData
   internal lastObservedTick
   internal slowOracleTick
   internal fastOracleTick
   internal currentTick
   internal tickData
   internal unpackTickData
   internal utilization0
   internal utilization1
   internal utilizations
   internal positionSize

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

### <a name="NC-9"></a>[NC-9] Functions should not be longer than 50 lines

Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability

*Instances (138)*:

```solidity
File: contracts/CollateralTracker.sol

274:     function name() external view returns (string memory) {

288:     function symbol() external view returns (string memory) {

295:     function decimals() external view returns (uint8) {

346:     function asset() external view returns (address assetTokenAddress) {

355:     function totalAssets() public view returns (uint256) {

364:     function convertToShares(uint256 assets) public view returns (uint256 shares) {

371:     function convertToAssets(uint256 shares) public view returns (uint256 assets) {

377:     function maxDeposit(address) external pure returns (uint256 maxAssets) {

384:     function previewDeposit(uint256 assets) public view returns (uint256 shares) {

402:     function deposit(uint256 assets, address receiver) external returns (uint256 shares) {

427:     function maxMint(address) external view returns (uint256 maxShares) {

436:     function previewMint(uint256 shares) public view returns (uint256 assets) {

458:     function mint(uint256 shares, address receiver) external returns (uint256 assets) {

486:     function maxWithdraw(address owner) public view returns (uint256 maxAssets) {

497:     function previewWithdraw(uint256 assets) public view returns (uint256 shares) {

592:     function maxRedeem(address owner) public view returns (uint256 maxShares) {

601:     function previewRedeem(uint256 shares) public view returns (uint256 assets) {

753:     function _poolUtilization() internal view returns (uint256 poolUtilization) {

874:     function delegate(address delegatee) external onlyPanopticPool {

882:     function revoke(address delegatee) external onlyPanopticPool {

982:     function refund(address refunder, address refundee, int256 assets) external onlyPanopticPool {

1215:     function _getRequiredCollateralAtTickSinglePosition(

1279:     function _getRequiredCollateralSingleLegNoPartner(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

399:     function getPanopticPool(IUniswapV3Pool univ3pool) external view returns (PanopticPool) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

310:     function assertMinCollateralValues(uint256 minValue0, uint256 minValue1) external view {

1251:     function isSafeMode() public view returns (bool) {

1311:     function _updatePositionsHash(address account, TokenId tokenId, bool addFlag) internal {

1331:     function univ3pool() external view returns (IUniswapV3Pool) {

1337:     function collateralToken0() external view returns (CollateralTracker) {

1343:     function collateralToken1() external view returns (CollateralTracker) {

1372:     function numberOfPositions(address user) external view returns (uint256) {

1395:     function getUniV3TWAP() internal view returns (int24 twapTick) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

305:     function initializeAMMPool(address token0, address token1, uint24 fee) external {

530:     function registerTokenTransfer(address from, address to, TokenId id, uint256 amount) internal {

1384:     function getPoolId(address univ3pool) external view returns (uint64 poolId) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

40:     function tokenURI(uint256 tokenId) public view override returns (string memory) {

178:     function getChainName() internal view returns (string memory) {

205:     function write(string memory chars) internal view returns (string memory) {

265:     function maxSymbolWidth(uint256 rarity) internal pure returns (uint256 width) {

291:     function maxRarityLabelWidth(uint256 rarity) internal pure returns (uint256 width) {

321:     function maxStrategyLabelWidth(uint256 rarity) internal pure returns (uint256 width) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/Multicall.sol

12:     function multicall(bytes[] calldata data) public payable returns (bytes[] memory results) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

88:     function computeDecimals(address token) external view returns (uint8) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/Math.sol

25:     function min24(int24 a, int24 b) internal pure returns (int24) {

33:     function max24(int24 a, int24 b) internal pure returns (int24) {

41:     function min(uint256 a, uint256 b) internal pure returns (uint256) {

49:     function min(int256 a, int256 b) internal pure returns (int256) {

57:     function max(uint256 a, uint256 b) internal pure returns (uint256) {

65:     function max(int256 a, int256 b) internal pure returns (int256) {

73:     function abs(int256 x) internal pure returns (int256) {

81:     function absUint(int256 x) internal pure returns (uint256) {

91:     function mostSignificantNibble(uint160 x) internal pure returns (uint256 r) {

128:     function getSqrtRatioAtTick(int24 tick) internal pure returns (uint160) {

196:     function getAmount0ForLiquidity(LiquidityChunk liquidityChunk) internal pure returns (uint256) {

212:     function getAmount1ForLiquidity(LiquidityChunk liquidityChunk) internal pure returns (uint256) {

301:     function toUint128(uint256 toDowncast) internal pure returns (uint128 downcastedInt) {

307:     function toUint128Capped(uint256 toDowncast) internal pure returns (uint128 downcastedInt) {

316:     function toInt128(uint128 toCast) internal pure returns (int128 downcastedInt) {

323:     function toInt128(int256 toCast) internal pure returns (int128 downcastedInt) {

330:     function toInt256(uint256 toCast) internal pure returns (int256) {

561:     function mulDiv64(uint256 a, uint256 b) internal pure returns (uint256) {

624:     function mulDiv96(uint256 a, uint256 b) internal pure returns (uint256) {

687:     function mulDiv96RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

701:     function mulDiv128(uint256 a, uint256 b) internal pure returns (uint256) {

764:     function mulDiv128RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

778:     function mulDiv192(uint256 a, uint256 b) internal pure returns (uint256) {

841:     function mulDiv192RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

855:     function unsafeDivRoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

870:     function quickSort(int256[] memory arr, int256 left, int256 right) internal pure {

893:     function sort(int256[] memory data) internal pure returns (int256[] memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

49:     function getPoolId(address univ3pool) internal view returns (uint64) {

61:     function incrementPoolPattern(uint64 poolId) internal pure returns (uint64) {

76:     function numberOfLeadingHexZeros(address addr) external pure returns (uint256) {

85:     function safeERC20Symbol(address token) external view returns (string memory) {

99:     function uniswapFeeToString(uint24 fee) internal pure returns (string memory) {

336:     function twapFilter(IUniswapV3Pool univ3pool, uint32 twapWindow) external view returns (int24) {

512:     function convert0to1(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

549:     function convert1to0(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

591:     function convert0to1(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

614:     function convert1to0(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

21:     function safeTransferFrom(address token, address from, address to, uint256 amount) internal {

52:     function safeTransfer(address token, address to, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

81:     function setApprovalForAll(address operator, bool approved) public {

200:     function supportsInterface(bytes4 interfaceId) public pure returns (bool) {

214:     function _mint(address to, uint256 id, uint256 amount) internal {

236:     function _burn(address from, uint256 id, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

49:     function approve(address spender, uint256 amount) public returns (bool) {

61:     function transfer(address to, uint256 amount) public virtual returns (bool) {

81:     function transferFrom(address from, address to, uint256 amount) public virtual returns (bool) {

103:     function _transferFrom(address from, address to, uint256 amount) internal {

122:     function _mint(address to, uint256 amount) internal {

136:     function _burn(address from, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

```solidity
File: contracts/tokens/interfaces/IERC20Partial.sol

16:     function balanceOf(address account) external view returns (uint256);

22:     function approve(address spender, uint256 amount) external;

27:     function transfer(address to, uint256 amount) external;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/interfaces/IERC20Partial.sol)

```solidity
File: contracts/types/LeftRight.sol

38:     function rightSlot(LeftRightUnsigned self) internal pure returns (uint128) {

45:     function rightSlot(LeftRightSigned self) internal pure returns (int128) {

100:     function leftSlot(LeftRightUnsigned self) internal pure returns (uint128) {

107:     function leftSlot(LeftRightSigned self) internal pure returns (int128) {

133:     function toLeftSlot(LeftRightSigned self, int128 left) internal pure returns (LeftRightSigned) {

193:     function add(LeftRightUnsigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

213:     function add(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

231:     function sub(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

171:     function tickLower(LiquidityChunk self) internal pure returns (int24) {

180:     function tickUpper(LiquidityChunk self) internal pure returns (int24) {

189:     function liquidity(LiquidityChunk self) internal pure returns (uint128) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

29:     function location(Pointer self) internal pure returns (address) {

36:     function start(Pointer self) internal pure returns (uint48) {

43:     function size(Pointer self) internal pure returns (uint48) {

50:     function data(Pointer self) internal view returns (bytes memory) {

67:     function dataStr(Pointer self) internal view returns (string memory) {

74:     function decompressedDataStr(Pointer self) internal view returns (string memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

85:     function lastObservedTick(PositionBalance self) internal pure returns (int24) {

94:     function slowOracleTick(PositionBalance self) internal pure returns (int24) {

103:     function fastOracleTick(PositionBalance self) internal pure returns (int24) {

112:     function currentTick(PositionBalance self) internal pure returns (int24) {

121:     function tickData(PositionBalance self) internal pure returns (uint96) {

133:     function unpackTickData(uint96 _tickData) internal pure returns (int24, int24, int24, int24) {

146:     function utilization0(PositionBalance self) internal pure returns (int256) {

155:     function utilization1(PositionBalance self) internal pure returns (int256) {

164:     function utilizations(PositionBalance self) internal pure returns (uint32) {

173:     function positionSize(PositionBalance self) internal pure returns (uint128) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

87:     function poolId(TokenId self) internal pure returns (uint64) {

96:     function tickSpacing(TokenId self) internal pure returns (int24) {

108:     function asset(TokenId self, uint256 legIndex) internal pure returns (uint256) {

118:     function optionRatio(TokenId self, uint256 legIndex) internal pure returns (uint256) {

128:     function isLong(TokenId self, uint256 legIndex) internal pure returns (uint256) {

138:     function tokenType(TokenId self, uint256 legIndex) internal pure returns (uint256) {

148:     function riskPartner(TokenId self, uint256 legIndex) internal pure returns (uint256) {

158:     function strike(TokenId self, uint256 legIndex) internal pure returns (int24) {

169:     function width(TokenId self, uint256 legIndex) internal pure returns (int24) {

183:     function addPoolId(TokenId self, uint64 _poolId) internal pure returns (TokenId) {

193:     function addTickSpacing(TokenId self, int24 _tickSpacing) internal pure returns (TokenId) {

366:     function flipToBurnToken(TokenId self) internal pure returns (TokenId) {

404:     function countLongs(TokenId self) internal pure returns (uint256) {

432:     function countLegs(TokenId self) internal pure returns (uint256) {

464:     function clearLeg(TokenId self, uint256 i) internal pure returns (TokenId) {

578:     function validateIsExercisable(TokenId self, int24 currentTick) internal pure {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-10"></a>[NC-10] Change int to int256

Throughout the code base, some variables are declared as `int`. To favor explicitness, consider changing all instances of `int` to `int256`

*Instances (12)*:

```solidity
File: contracts/CollateralTracker.sol

1027:                 uint256 sharesToMint = convertToShares(uint256(-tokenToPay));

1076:                 uint256 sharesToMint = convertToShares(uint256(-tokenToPay));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

116:     bool internal constant MINT = false;

894:             Selling(isLong=0): Mint chunk of liquidity in Uniswap (defined by upper tick, lower tick, and amount)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/Math.sol

302:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) revert Errors.CastingError();

308:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) {

309:             downcastedInt = type(uint128).max;

317:         if ((downcastedInt = int128(toCast)) < 0) revert Errors.CastingError();

324:         if (!((downcastedInt = int128(toCast)) == toCast)) revert Errors.CastingError();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/types/LeftRight.sol

25:     int256 internal constant LEFT_HALF_BIT_MASK_INT =

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/PositionBalance.sol

210:         utilization0AtMint = self.utilization0();

211:         utilization1AtMint = self.utilization1();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

### <a name="NC-11"></a>[NC-11] Lack of checks in setters

Be it sanity checks (like checks against `0`-values) or initial setting checks: it's best for Setter functions to have them

*Instances (2)*:

```solidity
File: contracts/SemiFungiblePositionManager.sol

964:     /// @dev Stored fees base is rounded up and the current fees base is rounded down to minimize the amount of fees collected (Δfeesbase) in favor of the protocol.
         /// @param univ3pool The Uniswap pool
         /// @param liquidity The total amount of liquidity in the AMM for the specific position
         /// @param liquidityChunk The liquidity chunk in Uniswap to compute the feesBase for
         /// @param roundUp If true, round up the feesBase, otherwise round down
         function _getFeesBase(
             IUniswapV3Pool univ3pool,
             uint128 liquidity,
             LiquidityChunk liquidityChunk,
             bool roundUp
         ) private view returns (LeftRightSigned feesBase) {
             // read the latest feeGrowth directly from the Uniswap pool
             (, uint256 feeGrowthInside0LastX128, uint256 feeGrowthInside1LastX128, , ) = univ3pool
                 .positions(
                     keccak256(
                         abi.encodePacked(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

81:     function setApprovalForAll(address operator, bool approved) public {
            isApprovedForAll[msg.sender][operator] = approved;
    
            emit ApprovalForAll(msg.sender, operator, approved);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

### <a name="NC-12"></a>[NC-12] Lines are too long

Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*Instances (1)*:

```solidity
File: contracts/CollateralTracker.sol

863:                     (SATURATED_POOL_UTIL - TARGET_POOL_UTIL)) / 2; // do the division by 2 at the end after all addition and multiplication; b/c y1 = buyCollateralRatio / 2

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="NC-13"></a>[NC-13] `type(uint256).max` should be used instead of `2 ** 256 - 1`

*Instances (2)*:

```solidity
File: contracts/libraries/Math.sol

15:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

25:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="NC-14"></a>[NC-14] Missing Event for critical parameters change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

*Instances (1)*:

```solidity
File: contracts/CollateralTracker.sol

891:     function settleLiquidation(
             address liquidator,
             address liquidatee,
             int256 bonus
         ) external onlyPanopticPool {
             if (bonus < 0) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="NC-15"></a>[NC-15] Incomplete NatSpec: `@param` is missing on actually documented functions

The following functions are missing `@param` NatSpec comments.

*Instances (2)*:

```solidity
File: contracts/CollateralTracker.sol

304:     /// @dev See {IERC20-transfer}.
         /// Requirements:
         /// - the caller must have a balance of at least 'amount'.
         /// - the msg.sender must not have any position on the panoptic pool
         function transfer(
             address recipient,
             uint256 amount

321:     /// @dev See {IERC20-transferFrom}.
         /// Requirements:
         /// - the 'from' must have a balance of at least 'amount'.
         /// - the caller must have allowance for 'from' of at least 'amount' tokens.
         /// - 'from' must not have any open positions on the panoptic pool.
         function transferFrom(
             address from,
             address to,
             uint256 amount

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="NC-16"></a>[NC-16] Incomplete NatSpec: `@return` is missing on actually documented functions

The following functions are missing `@return` NatSpec comments.

*Instances (2)*:

```solidity
File: contracts/CollateralTracker.sol

304:     /// @dev See {IERC20-transfer}.
         /// Requirements:
         /// - the caller must have a balance of at least 'amount'.
         /// - the msg.sender must not have any position on the panoptic pool
         function transfer(
             address recipient,
             uint256 amount
         ) public override(ERC20Minimal) returns (bool) {

321:     /// @dev See {IERC20-transferFrom}.
         /// Requirements:
         /// - the 'from' must have a balance of at least 'amount'.
         /// - the caller must have allowance for 'from' of at least 'amount' tokens.
         /// - 'from' must not have any open positions on the panoptic pool.
         function transferFrom(
             address from,
             address to,
             uint256 amount
         ) public override(ERC20Minimal) returns (bool) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="NC-17"></a>[NC-17] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor

If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (12)*:

```solidity
File: contracts/CollateralTracker.sol

169:         if (msg.sender != address(s_panopticPool)) revert Errors.NotPanopticPool();

316:         if (s_panopticPool.numberOfPositions(msg.sender) != 0) revert Errors.PositionCountNotZero();

520:         if (msg.sender != owner) {

523:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

562:         if (msg.sender != owner) {

565:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

619:         if (msg.sender != owner) {

622:             if (allowed != type(uint256).max) allowance[owner][msg.sender] = allowed - shares;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

578:         if (PositionBalance.unwrap(s_positionBalance[msg.sender][tokenId]) != 0)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

101:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

137:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

84:         if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed - amount;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

### <a name="NC-18"></a>[NC-18] Constant state variables defined more than once

Rather than redefining state variable constant, consider using a library to store all constants as this will prevent data redundancy

*Instances (2)*:

```solidity
File: contracts/libraries/Math.sol

15:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

25:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="NC-19"></a>[NC-19] Adding a `return` statement when the function defines a named return variable, is redundant

*Instances (28)*:

```solidity
File: contracts/CollateralTracker.sol

344:     /// @notice Get the token contract address of the underlying asset being managed.
         /// @return assetTokenAddress The address of the underlying asset
         function asset() external view returns (address assetTokenAddress) {
             return s_underlyingToken;

361:     /// @notice Returns the amount of shares that can be minted for the given amount of assets.
         /// @param assets The amount of assets to be deposited
         /// @return shares The amount of shares that can be minted
         function convertToShares(uint256 assets) public view returns (uint256 shares) {
             return Math.mulDiv(assets, totalSupply, totalAssets());

368:     /// @notice Returns the amount of assets that can be redeemed for the given amount of shares.
         /// @param shares The amount of shares to be redeemed
         /// @return assets The amount of assets that can be redeemed
         function convertToAssets(uint256 shares) public view returns (uint256 assets) {
             return Math.mulDiv(shares, totalAssets(), totalSupply);

375:     /// @notice Returns the maximum deposit amount.
         /// @return maxAssets The maximum amount of assets that can be deposited
         function maxDeposit(address) external pure returns (uint256 maxAssets) {
             return type(uint104).max;

425:     /// @notice Returns the maximum shares received for a deposit.
         /// @return maxShares The maximum amount of shares that can be minted.
         function maxMint(address) external view returns (uint256 maxShares) {
             unchecked {
                 return (convertToShares(type(uint104).max) * (DECIMALS - COMMISSION_FEE)) / DECIMALS;

481:     /// @notice Returns The maximum amount of assets that can be withdrawn for a given user.
         /// If the user has any open positions, the max withdrawable balance is zero.
         /// @dev Calculated from the balance of the user; limited by the assets the pool has available.
         /// @param owner The address being withdrawn for.
         /// @return maxAssets The maximum amount of assets that can be withdrawn.
         function maxWithdraw(address owner) public view returns (uint256 maxAssets) {
             // We can only use the standard 4626 withdraw function if the user has no open positions
             // For the sake of simplicity assets can only be withdrawn through the redeem function
             uint256 available = s_poolAssets - 1;
             uint256 balance = convertToAssets(balanceOf[owner]);
             return s_panopticPool.numberOfPositions(owner) == 0 ? Math.min(available, balance) : 0;

494:     /// @notice Returns the amount of shares that would be burned to withdraw a given amount of assets.
         /// @param assets The amount of assets to be withdrawn.
         /// @return shares The amount of shares that would be burned.
         function previewWithdraw(uint256 assets) public view returns (uint256 shares) {
             uint256 supply = totalSupply; // Saves an extra SLOAD if totalSupply is non-zero.
     
             return Math.mulDivRoundingUp(assets, supply, totalAssets());

588:     /// @notice Returns the maximum amount of shares that can be redeemed for a given user.
         /// If the user has any open positions, the max redeemable balance is zero.
         /// @param owner The redeeming address.
         /// @return maxShares The maximum amount of shares that can be redeemed.
         function maxRedeem(address owner) public view returns (uint256 maxShares) {
             uint256 available = convertToShares(s_poolAssets - 1);
             uint256 balance = balanceOf[owner];
             return s_panopticPool.numberOfPositions(owner) == 0 ? Math.min(available, balance) : 0;

598:     /// @notice returns the amount of assets resulting from a given amount of shares being redeemed
         /// @param shares the amount of shares to be redeemed
         /// @return assets the amount of assets resulting from the redemption
         function previewRedeem(uint256 shares) public view returns (uint256 assets) {
             return convertToAssets(shares);

751:     /// @dev compute: inAMM/totalAssets().
         /// @return poolUtilization The pool utilization in basis points
         function _poolUtilization() internal view returns (uint256 poolUtilization) {
             unchecked {
                 return (s_inAMM * DECIMALS) / totalAssets();

759:     /// @notice Get the (sell) collateral ratio that is paid when a short option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalAssets() that belongs to the Uniswap Pool
         /// @return sellCollateralRatio The sell collateral ratio at `utilization`
         function _sellCollateralRatio(
             int256 utilization
         ) internal view returns (uint256 sellCollateralRatio) {
             // the sell ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,min_sell_ratio) and
             //   (x1,y1) = (saturatedPoolUtilization,max_sell_ratio)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             /**
                 SELL
                 COLLATERAL
                 RATIO
                               ^
                               |                  max ratio = 100%
                        100% - |                _------
                               |             _-¯
                               |          _-¯
                         20% - |---------¯
                               |         .       . .
                               +---------+-------+-+--->   POOL_
                                        50%    90% 100%     UTILIZATION
             */
     
             uint256 min_sell_ratio = SELLER_COLLATERAL_RATIO;
             /// if utilization is less than zero, this is the calculation for a strangle, which gets 2x the capital efficiency at low pool utilization
             /// at 0% utilization, strangle legs do not compound efficiency
             if (utilization < 0) {
                 unchecked {
                     min_sell_ratio /= 2;
                     utilization = -utilization;
                 }
             }
     
             // return the basal sell ratio if pool utilization is lower than target
             if (uint256(utilization) < TARGET_POOL_UTIL) {
                 return min_sell_ratio;
             }
     
             // return 100% collateral ratio if utilization is above saturated pool utilization
             // this means all new positions are fully collateralized, which reduces risks of insolvency at high pool utilization
             if (uint256(utilization) > SATURATED_POOL_UTIL) {
                 return DECIMALS;
             }
     
             unchecked {
                 return

759:     /// @notice Get the (sell) collateral ratio that is paid when a short option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalAssets() that belongs to the Uniswap Pool
         /// @return sellCollateralRatio The sell collateral ratio at `utilization`
         function _sellCollateralRatio(
             int256 utilization
         ) internal view returns (uint256 sellCollateralRatio) {
             // the sell ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,min_sell_ratio) and
             //   (x1,y1) = (saturatedPoolUtilization,max_sell_ratio)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             /**
                 SELL
                 COLLATERAL
                 RATIO
                               ^
                               |                  max ratio = 100%
                        100% - |                _------
                               |             _-¯
                               |          _-¯
                         20% - |---------¯
                               |         .       . .
                               +---------+-------+-+--->   POOL_
                                        50%    90% 100%     UTILIZATION
             */
     
             uint256 min_sell_ratio = SELLER_COLLATERAL_RATIO;
             /// if utilization is less than zero, this is the calculation for a strangle, which gets 2x the capital efficiency at low pool utilization
             /// at 0% utilization, strangle legs do not compound efficiency
             if (utilization < 0) {
                 unchecked {
                     min_sell_ratio /= 2;
                     utilization = -utilization;
                 }
             }
     
             // return the basal sell ratio if pool utilization is lower than target
             if (uint256(utilization) < TARGET_POOL_UTIL) {
                 return min_sell_ratio;

759:     /// @notice Get the (sell) collateral ratio that is paid when a short option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalAssets() that belongs to the Uniswap Pool
         /// @return sellCollateralRatio The sell collateral ratio at `utilization`
         function _sellCollateralRatio(
             int256 utilization
         ) internal view returns (uint256 sellCollateralRatio) {
             // the sell ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,min_sell_ratio) and
             //   (x1,y1) = (saturatedPoolUtilization,max_sell_ratio)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             /**
                 SELL
                 COLLATERAL
                 RATIO
                               ^
                               |                  max ratio = 100%
                        100% - |                _------
                               |             _-¯
                               |          _-¯
                         20% - |---------¯
                               |         .       . .
                               +---------+-------+-+--->   POOL_
                                        50%    90% 100%     UTILIZATION
             */
     
             uint256 min_sell_ratio = SELLER_COLLATERAL_RATIO;
             /// if utilization is less than zero, this is the calculation for a strangle, which gets 2x the capital efficiency at low pool utilization
             /// at 0% utilization, strangle legs do not compound efficiency
             if (utilization < 0) {
                 unchecked {
                     min_sell_ratio /= 2;
                     utilization = -utilization;
                 }
             }
     
             // return the basal sell ratio if pool utilization is lower than target
             if (uint256(utilization) < TARGET_POOL_UTIL) {
                 return min_sell_ratio;
             }
     
             // return 100% collateral ratio if utilization is above saturated pool utilization
             // this means all new positions are fully collateralized, which reduces risks of insolvency at high pool utilization
             if (uint256(utilization) > SATURATED_POOL_UTIL) {
                 return DECIMALS;

814:     /// @notice Get the (buy) collateral ratio that is paid when a long option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalBalance() that belongs to the Uniswap Pool
         /// @return buyCollateralRatio The buy collateral ratio at `utilization`
         function _buyCollateralRatio(
             uint16 utilization
         ) internal view returns (uint256 buyCollateralRatio) {
             // linear from BUY to BUY/2 between 50% and 90%
             // the buy ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,buyCollateralRatio) and
             //   (x1,y1) = (saturatedPoolUtilization,buyCollateralRatio / 2)
             // note that y1<y0 so the slope is negative:
             // aka the buy ratio starts high and drops to a lower value with increased utilization; the sell ratio does the opposite (slope is positive)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             // but since a<0, we rewrite as:
             // y = a' * (x0 - x) + y0, where a' = (y0 - y1) / (x1 - x0)
     
             // HOWEVER, if the utilization is larger than 10_000, then default to 100% buying power requirement.
             // this denotes a situation where the median is too far away from the current price, so we need to require fully collateralized positions for safety
             /**
               BUY
               COLLATERAL
               RATIO
                      ^
                      |   buy_ratio = 10%
                10% - |----------__       min_ratio = 5%
                5%  - | . . . . .  ¯¯¯--______
                      |         .       . .
                      +---------+-------+-+--->   POOL_
                               50%    90% 100%      UTILIZATION
              */
     
             // return the basal buy ratio if pool utilization is lower than target
             if (utilization < TARGET_POOL_UTIL) {
                 return BUYER_COLLATERAL_RATIO;
             }
     
             // return the basal ratio divided by 2 if pool utilization is above saturated pool utilization
             /// this is incentivized buying, which returns funds to the panoptic pool
             if (utilization > SATURATED_POOL_UTIL) {
                 unchecked {
                     return BUYER_COLLATERAL_RATIO / 2;
                 }
             }
     
             unchecked {
                 return
                     (BUYER_COLLATERAL_RATIO +

814:     /// @notice Get the (buy) collateral ratio that is paid when a long option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalBalance() that belongs to the Uniswap Pool
         /// @return buyCollateralRatio The buy collateral ratio at `utilization`
         function _buyCollateralRatio(
             uint16 utilization
         ) internal view returns (uint256 buyCollateralRatio) {
             // linear from BUY to BUY/2 between 50% and 90%
             // the buy ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,buyCollateralRatio) and
             //   (x1,y1) = (saturatedPoolUtilization,buyCollateralRatio / 2)
             // note that y1<y0 so the slope is negative:
             // aka the buy ratio starts high and drops to a lower value with increased utilization; the sell ratio does the opposite (slope is positive)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             // but since a<0, we rewrite as:
             // y = a' * (x0 - x) + y0, where a' = (y0 - y1) / (x1 - x0)
     
             // HOWEVER, if the utilization is larger than 10_000, then default to 100% buying power requirement.
             // this denotes a situation where the median is too far away from the current price, so we need to require fully collateralized positions for safety
             /**
               BUY
               COLLATERAL
               RATIO
                      ^
                      |   buy_ratio = 10%
                10% - |----------__       min_ratio = 5%
                5%  - | . . . . .  ¯¯¯--______
                      |         .       . .
                      +---------+-------+-+--->   POOL_
                               50%    90% 100%      UTILIZATION
              */
     
             // return the basal buy ratio if pool utilization is lower than target
             if (utilization < TARGET_POOL_UTIL) {
                 return BUYER_COLLATERAL_RATIO;

814:     /// @notice Get the (buy) collateral ratio that is paid when a long option is minted at a specific pool utilization.
         /// @dev This is computed at the time the position is minted.
         /// @param utilization The fraction of totalBalance() that belongs to the Uniswap Pool
         /// @return buyCollateralRatio The buy collateral ratio at `utilization`
         function _buyCollateralRatio(
             uint16 utilization
         ) internal view returns (uint256 buyCollateralRatio) {
             // linear from BUY to BUY/2 between 50% and 90%
             // the buy ratio is on a straight line defined between two points (x0,y0) and (x1,y1):
             //   (x0,y0) = (targetPoolUtilization,buyCollateralRatio) and
             //   (x1,y1) = (saturatedPoolUtilization,buyCollateralRatio / 2)
             // note that y1<y0 so the slope is negative:
             // aka the buy ratio starts high and drops to a lower value with increased utilization; the sell ratio does the opposite (slope is positive)
             // the line's formula: y = a * (x - x0) + y0, where a = (y1 - y0) / (x1 - x0)
             // but since a<0, we rewrite as:
             // y = a' * (x0 - x) + y0, where a' = (y0 - y1) / (x1 - x0)
     
             // HOWEVER, if the utilization is larger than 10_000, then default to 100% buying power requirement.
             // this denotes a situation where the median is too far away from the current price, so we need to require fully collateralized positions for safety
             /**
               BUY
               COLLATERAL
               RATIO
                      ^
                      |   buy_ratio = 10%
                10% - |----------__       min_ratio = 5%
                5%  - | . . . . .  ¯¯¯--______
                      |         .       . .
                      +---------+-------+-+--->   POOL_
                               50%    90% 100%      UTILIZATION
              */
     
             // return the basal buy ratio if pool utilization is lower than target
             if (utilization < TARGET_POOL_UTIL) {
                 return BUYER_COLLATERAL_RATIO;
             }
     
             // return the basal ratio divided by 2 if pool utilization is above saturated pool utilization
             /// this is incentivized buying, which returns funds to the panoptic pool
             if (utilization > SATURATED_POOL_UTIL) {
                 unchecked {
                     return BUYER_COLLATERAL_RATIO / 2;

1240:     /// @notice Calculate the required amount of collateral for a single leg 'index' of position 'tokenId'.
          /// @param tokenId The option position
          /// @param index The leg index (associated with a liquidity chunk) to consider a partner for
          /// @param positionSize The size of the position
          /// @param atTick The tick at which to evaluate the account's positions
          /// @param poolUtilization The pool utilization: how much funds are in the Panoptic pool versus the AMM pool
          /// @return required The required amount collateral needed for this leg 'index'
          function _getRequiredCollateralSingleLeg(
              TokenId tokenId,
              uint256 index,
              uint128 positionSize,
              int24 atTick,
              int16 poolUtilization
          ) internal view returns (uint256 required) {
              return
                  tokenId.riskPartner(index) == index // does this leg have a risk partner? Affects required collateral

1551:     /// @notice Calculate the required amount of collateral for a strangle leg.
          /// @dev Strangle legs are evaluated at 2x capital efficiency at low pool utilizations.
          /// @dev A strangle can only have only one of its leg tested at the same time, so this reduces the total risk and collateral requirement.
          /// @param tokenId The option position
          /// @param positionSize The size of the position
          /// @param index The leg index (associated with a liquidity chunk) to consider a partner for
          /// @param atTick The tick at which to evaluate the account's positions
          /// @param poolUtilization The pool utilization: how much funds are in the Panoptic pool versus the AMM pool
          /// @return strangleRequired The required amount of collateral needed for the strangle leg
          function _computeStrangle(
              TokenId tokenId,
              uint256 index,
              uint128 positionSize,
              int24 atTick,
              int16 poolUtilization
          ) internal view returns (uint256 strangleRequired) {
              // If both tokenTypes are the same, then this is a long or short strangle.
              // A strangle is an options strategy in which the investor holds a position
              // in both a call and a put option with different strike prices,
              // but with the same expiration date and underlying asset.
      
              /// collateral requirement is for short strangles depicted:
              /**
                          Put side of a short strangle, BPR = 100% - (100% - SCR/2)*(price/strike)
                 BUYING
                 POWER
                 REQUIREMENT
                               ^                    .
                               |           <- ITM   .  OTM ->
                        100% - |--__                .
                               |    ¯¯--__          .
                               |          ¯¯--__    .
                       SCR/2 - |                ¯¯--______ <------ base collateral is half that of a single-leg
                               +--------------------+--->   current
                               0                  strike     price
               */
              unchecked {
                  // A negative pool utilization is used to denote a position which is a strangle
                  // at low pool utilization's strangle legs are evaluated at 2x capital efficiency
      
                  // add 1 to handle poolUtilization = 0
                  poolUtilization = -(poolUtilization == 0 ? int16(1) : poolUtilization);
      
                  return
                      strangleRequired = _getRequiredCollateralSingleLegNoPartner(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

1403:     /// @notice Ensure the effective liquidity in a given chunk is above a certain threshold.
          /// @param tokenId An option position
          /// @param leg A leg index of `tokenId` corresponding to a tickLower-tickUpper chunk
          /// @param effectiveLiquidityLimitX32 Maximum amount of "spread" defined as removedLiquidity/netLiquidity for a new position
          /// denominated as X32 = (ratioLimit * 2**32)
          /// @return totalLiquidity The total liquidity deposited in that chunk: `totalLiquidity = netLiquidity + removedLiquidity`
          function _checkLiquiditySpread(
              TokenId tokenId,
              uint256 leg,
              uint64 effectiveLiquidityLimitX32
          ) internal view returns (uint256 totalLiquidity) {
              uint128 netLiquidity;
              uint128 removedLiquidity;
              (totalLiquidity, netLiquidity, removedLiquidity) = _getLiquidities(tokenId, leg);
      
              // compute and return effective liquidity. Return if short=net=0, which is closing short position
              if (netLiquidity == 0 && removedLiquidity == 0) return totalLiquidity;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

629:                         token0: _univ3pool.token0(),
                             token1: _univ3pool.token1(),
                             fee: _univ3pool.fee()
                         }),
                         payer: msg.sender
                     })
                 );
     
                 // NOTE: upstream users of this function such as the Panoptic Pool should ensure users always compensate for the ITM amount delta
                 // the netting swap is not perfectly accurate, and it is possible for swaps to run out of liquidity, so we do not want to rely on it
                 // this is simply a convenience feature, and should be treated as such
                 if ((itm0 != 0) && (itm1 != 0)) {
                     (uint160 sqrtPriceX96, , , , , , ) = _univ3pool.slot0();
     
                     // implement a single "netting" swap. Thank you @danrobinson for this puzzle/idea
                     // NOTE: negative ITM amounts denote a surplus of tokens (burning liquidity), while positive amounts denote a shortage of tokens (minting liquidity)
                     // compute the approximate delta of token0 that should be resolved in the swap at the current tick
                     // we do this by flipping the signs on the token1 ITM amount converting+deducting it against the token0 ITM amount
                     // couple examples (price = 2 1/0):
                     //  - 100 surplus 0, 100 surplus 1 (itm0 = -100, itm1 = -100)
                     //    normal swap 0: 100 0 => 200 1
                     //    normal swap 1: 100 1 => 50 0
                     //    final swap amounts: 50 0 => 100 1
                     //    netting swap: net0 = -100 - (-100/2) = -50, ZF1 = true, 50 0 => 100 1
                     // - 100 surplus 0, 100 shortage 1 (itm0 = -100, itm1 = 100)
                     //    normal swap 0: 100 0 => 200 1
                     //    normal swap 1: 50 0 => 100 1
                     //    final swap amounts: 150 0 => 300 1
                     //    netting swap: net0 = -100 - (100/2) = -150, ZF1 = true, 150 0 => 300 1
                     // - 100 shortage 0, 100 surplus 1 (itm0 = 100, itm1 = -100)
                     //    normal swap 0: 200 1 => 100 0
                     //    normal swap 1: 100 1 => 50 0
                     //    final swap amounts: 300 1 => 150 0
                     //    netting swap: net0 = 100 - (-100/2) = 150, ZF1 = false, 300 1 => 150 0
                     // - 100 shortage 0, 100 shortage 1 (itm0 = 100, itm1 = 100)
                     //    normal swap 0: 200 1 => 100 0
                     //    normal swap 1: 50 0 => 100 1
                     //    final swap amounts: 100 1 => 50 0
                     //    netting swap: net0 = 100 - (100/2) = 50, ZF1 = false, 100 1 => 50 0
                     // - = Net surplus of token0
                     // + = Net shortage of token0
                     int256 net0 = itm0 - PanopticMath.convert1to0(itm1, sqrtPriceX96);
     
                     zeroForOne = net0 < 0;
     
                     //compute the swap amount, set as positive (exact input)
                     swapAmount = -net0;
                 } else if (itm0 != 0) {
                     zeroForOne = itm0 < 0;
                     swapAmount = -itm0;
                 } else {
                     zeroForOne = itm1 > 0;
                     swapAmount = -itm1;
                 }
     
                 // NOTE: can occur if itm0 and itm1 have the same value
                 // in that case, swapping would be pointless so skip
                 if (swapAmount == 0) return LeftRightSigned.wrap(0);
     
                 // swap tokens in the Uniswap pool
                 // NOTE: this triggers our swap callback function
                 (int256 swap0, int256 swap1) = _univ3pool.swap(
                     msg.sender,
                     zeroForOne,
                     swapAmount,
                     zeroForOne
                         ? Constants.MIN_V3POOL_SQRT_RATIO + 1
                         : Constants.MAX_V3POOL_SQRT_RATIO - 1,
                     data
                 );
     
                 // Add amounts swapped to totalSwapped variable
                 totalSwapped = LeftRightSigned.wrap(0).toRightSlot(swap0.toInt128()).toLeftSlot(
                     swap1.toInt128()
                 );
             }
         }
     
         /// @notice Create the position in the AMM given in the tokenId.
         /// @dev Loops over each leg in the tokenId and calls _createLegInAMM for each, which does the mint/burn in the AMM.
         /// @param tickLimitLow The lower bound of an acceptable open interval for the ending price

1388: 

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/Math.sol

339:     /// @notice Calculates floor(a×b÷denominator) with full precision. Throws if result overflows a uint256 or denominator == 0.
         /// @param a The multiplicand
         /// @param b The multiplier
         /// @param denominator The divisor
         /// @return result The 256-bit result
         /// @dev Credit to Remco Bloemen under MIT license https://xn--2-umb.com/21/muldiv
         function mulDiv(
             uint256 a,
             uint256 b,
             uint256 denominator
         ) internal pure returns (uint256 result) {
             unchecked {
                 // 512-bit multiply [prod1 prod0] = a * b
                 // Compute the product mod 2**256 and mod 2**256 - 1
                 // then use the Chinese Remainder Theorem to reconstruct
                 // the 512 bit result. The result is stored in two 256
                 // variables such that product = prod1 * 2**256 + prod0
                 uint256 prod0; // Least significant 256 bits of the product
                 uint256 prod1; // Most significant 256 bits of the product
                 assembly ("memory-safe") {
                     let mm := mulmod(a, b, not(0))
                     prod0 := mul(a, b)
                     prod1 := sub(sub(mm, prod0), lt(mm, prod0))
                 }
     
                 // Handle non-overflow cases, 256 by 256 division
                 if (prod1 == 0) {
                     require(denominator > 0);
                     assembly ("memory-safe") {
                         result := div(prod0, denominator)
                     }
                     return result;

440:     /// @notice Calculates min(floor(a×b÷denominator), 2^256-1) with full precision.
         /// @param a The multiplicand
         /// @param b The multiplier
         /// @param denominator The divisor
         /// @return result The 256-bit result
         function mulDivCapped(
             uint256 a,
             uint256 b,
             uint256 denominator
         ) internal pure returns (uint256 result) {
             unchecked {
                 // 512-bit multiply [prod1 prod0] = a * b
                 // Compute the product mod 2**256 and mod 2**256 - 1
                 // then use the Chinese Remainder Theorem to reconstruct
                 // the 512 bit result. The result is stored in two 256
                 // variables such that product = prod1 * 2**256 + prod0
                 uint256 prod0; // Least significant 256 bits of the product
                 uint256 prod1; // Most significant 256 bits of the product
                 assembly ("memory-safe") {
                     let mm := mulmod(a, b, not(0))
                     prod0 := mul(a, b)
                     prod1 := sub(sub(mm, prod0), lt(mm, prod0))
                 }
     
                 // Handle non-overflow cases, 256 by 256 division
                 if (prod1 == 0) {
                     require(denominator > 0);
                     assembly ("memory-safe") {
                         result := div(prod0, denominator)
                     }
                     return result;
                 }
     
                 if (denominator <= prod1) return type(uint256).max;

440:     /// @notice Calculates min(floor(a×b÷denominator), 2^256-1) with full precision.
         /// @param a The multiplicand
         /// @param b The multiplier
         /// @param denominator The divisor
         /// @return result The 256-bit result
         function mulDivCapped(
             uint256 a,
             uint256 b,
             uint256 denominator
         ) internal pure returns (uint256 result) {
             unchecked {
                 // 512-bit multiply [prod1 prod0] = a * b
                 // Compute the product mod 2**256 and mod 2**256 - 1
                 // then use the Chinese Remainder Theorem to reconstruct
                 // the 512 bit result. The result is stored in two 256
                 // variables such that product = prod1 * 2**256 + prod0
                 uint256 prod0; // Least significant 256 bits of the product
                 uint256 prod1; // Most significant 256 bits of the product
                 assembly ("memory-safe") {
                     let mm := mulmod(a, b, not(0))
                     prod0 := mul(a, b)
                     prod1 := sub(sub(mm, prod0), lt(mm, prod0))
                 }
     
                 // Handle non-overflow cases, 256 by 256 division
                 if (prod1 == 0) {
                     require(denominator > 0);
                     assembly ("memory-safe") {
                         result := div(prod0, denominator)
                     }
                     return result;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/types/LeftRight.sol

189:     /// @notice Add two LeftRight-encoded words; revert on overflow or underflow.
         /// @param x The augend
         /// @param y The addend
         /// @return z The sum `x + y`
         function add(LeftRightUnsigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {
             unchecked {
                 int256 left = int256(uint256(x.leftSlot())) + y.leftSlot();
                 int128 left128 = int128(left);
     
                 if (left128 != left) revert Errors.UnderOverFlow();
     
                 int256 right = int256(uint256(x.rightSlot())) + y.rightSlot();
                 int128 right128 = int128(right);
     
                 if (right128 != right) revert Errors.UnderOverFlow();
     
                 return z.toRightSlot(right128).toLeftSlot(left128);

209:     /// @notice Add two LeftRight-encoded words; revert on overflow or underflow.
         /// @param x The augend
         /// @param y The addend
         /// @return z The sum `x + y`
         function add(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {
             unchecked {
                 int256 left256 = int256(x.leftSlot()) + y.leftSlot();
                 int128 left128 = int128(left256);
     
                 int256 right256 = int256(x.rightSlot()) + y.rightSlot();
                 int128 right128 = int128(right256);
     
                 if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();
     
                 return z.toRightSlot(right128).toLeftSlot(left128);

227:     /// @notice Subtract two LeftRight-encoded words; revert on overflow or underflow.
         /// @param x The minuend
         /// @param y The subtrahend
         /// @return z The difference `x - y`
         function sub(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {
             unchecked {
                 int256 left256 = int256(x.leftSlot()) - y.leftSlot();
                 int128 left128 = int128(left256);
     
                 int256 right256 = int256(x.rightSlot()) - y.rightSlot();
                 int128 right128 = int128(right256);
     
                 if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();
     
                 return z.toRightSlot(right128).toLeftSlot(left128);

245:     /// @notice Subtract two LeftRight-encoded words; revert on overflow or underflow.
         /// @notice For each slot, rectify difference `x - y` to 0 if negative.
         /// @param x The minuend
         /// @param y The subtrahend
         /// @return z The difference `x - y`
         function subRect(
             LeftRightSigned x,
             LeftRightSigned y
         ) internal pure returns (LeftRightSigned z) {
             unchecked {
                 int256 left256 = int256(x.leftSlot()) - y.leftSlot();
                 int128 left128 = int128(left256);
     
                 int256 right256 = int256(x.rightSlot()) - y.rightSlot();
                 int128 right128 = int128(right256);
     
                 if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();
     
                 return

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

### <a name="NC-20"></a>[NC-20] `require()` / `revert()` statements should have descriptive reason strings

*Instances (67)*:

```solidity
File: contracts/CollateralTracker.sol

169:         if (msg.sender != address(s_panopticPool)) revert Errors.NotPanopticPool();

219:         if (s_initialized) revert Errors.CollateralTokenAlreadyInitialized();

316:         if (s_panopticPool.numberOfPositions(msg.sender) != 0) revert Errors.PositionCountNotZero();

335:         if (s_panopticPool.numberOfPositions(from) != 0) revert Errors.PositionCountNotZero();

403:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

461:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

515:         if (assets > maxWithdraw(owner)) revert Errors.ExceedsMaximumRedemption();

616:         if (shares > maxRedeem(owner)) revert Errors.ExceedsMaximumRedemption();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

183:         if (address(v3Pool) == address(0)) revert Errors.UniswapPoolNotInitialized();

186:             revert Errors.PoolAlreadyInitialized();

229:         if (amount0 > amount0Max || amount1 > amount1Max) revert Errors.PriceBoundFail();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

272:         if (address(s_univ3pool) != address(0)) revert Errors.PoolAlreadyInitialized();

316:         ) revert Errors.AccountInsolvent();

574:             revert Errors.InvalidTokenIdParameter(0);

579:             revert Errors.PositionAlreadyMinted();

948:                     revert Errors.StaleTWAP();

1080:         if (touchedId.length != 1) revert Errors.InputListFail();

1204:         if (expectedSolvent && solvent != numberOfTicks) revert Errors.AccountInsolvent();

1205:         if (!expectedSolvent && solvent != 0) revert Errors.NotMarginCalled();

1300:         if (fingerprintIncomingList != currentHash) revert Errors.InputListFail();

1321:         if ((newHash >> 248) > MAX_POSITIONS) revert Errors.TooManyPositionsOpen();

1429:             revert Errors.EffectiveLiquidityAboveThreshold();

1531:         if (tokenId.isLong(legIndex) == 0 || legIndex > 3) revert Errors.NotALongLeg();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

323:         // There are 281,474,976,710,655 possible pool patterns.

585:     /// @dev When a position is minted or burnt in-the-money (ITM) we are *not* 100% token0 or 100% token1: we have a mix of both tokens.

754:                 tokenId,

799:     /// @dev  - mints any new liquidity in the AMM needed (via _mintLiquidity)

810:     /// @return collectedSingleLeg LeftRight encoded words containing the amount of token0 and token1 collected as fees

862:                 if (startingLiquidity < chunkLiquidity) {

887:         }

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

37:             revert Errors.InvalidUniswapCallback();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Math.sol

131:             if (absTick > uint256(int256(Constants.MAX_V3POOL_TICK))) revert Errors.InvalidTick();

302:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) revert Errors.CastingError();

317:         if ((downcastedInt = int128(toCast)) < 0) revert Errors.CastingError();

324:         if (!((downcastedInt = int128(toCast)) == toCast)) revert Errors.CastingError();

331:         if (toCast > uint256(type(int256).max)) revert Errors.CastingError();

366:                 require(denominator > 0);

375:             require(denominator > prod1);

466:                 require(denominator > 0);

551:                 require(result < type(uint256).max);

587:             require(2 ** 64 > prod1);

650:             require(2 ** 96 > prod1);

691:                 require(result < type(uint256).max);

727:             require(2 ** 128 > prod1);

768:                 require(result < type(uint256).max);

804:             require(2 ** 192 > prod1);

845:                 require(result < type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

460:     /// @notice Returns the distances of the upper and lower ticks from the strike for a position with the given width and tickSpacing.

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

45:         if (!success) revert Errors.TransferFailed();

75:         if (!success) revert Errors.TransferFailed();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/types/LeftRight.sol

162:             ) revert Errors.UnderOverFlow();

185:             ) revert Errors.UnderOverFlow();

198:             if (left128 != left) revert Errors.UnderOverFlow();

203:             if (right128 != right) revert Errors.UnderOverFlow();

221:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

239:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

261:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/TokenId.sol

501:         if (self.optionRatio(0) == 0) revert Errors.InvalidTokenIdParameter(1);

513:                         revert Errors.InvalidTokenIdParameter(1);

522:                         revert Errors.InvalidTokenIdParameter(6);

528:                 if ((self.width(i) == 0)) revert Errors.InvalidTokenIdParameter(5);

533:                 ) revert Errors.InvalidTokenIdParameter(4);

542:                         revert Errors.InvalidTokenIdParameter(3);

548:                     ) revert Errors.InvalidTokenIdParameter(3);

561:                         revert Errors.InvalidTokenIdParameter(4);

567:                         revert Errors.InvalidTokenIdParameter(5);

598:         revert Errors.NoLegsExercisable();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-21"></a>[NC-21] Take advantage of Custom Error's return value property

An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (51)*:

```solidity
File: contracts/CollateralTracker.sol

169:         if (msg.sender != address(s_panopticPool)) revert Errors.NotPanopticPool();

219:         if (s_initialized) revert Errors.CollateralTokenAlreadyInitialized();

316:         if (s_panopticPool.numberOfPositions(msg.sender) != 0) revert Errors.PositionCountNotZero();

335:         if (s_panopticPool.numberOfPositions(from) != 0) revert Errors.PositionCountNotZero();

403:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

461:         if (assets > type(uint104).max) revert Errors.DepositTooLarge();

515:         if (assets > maxWithdraw(owner)) revert Errors.ExceedsMaximumRedemption();

616:         if (shares > maxRedeem(owner)) revert Errors.ExceedsMaximumRedemption();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

183:         if (address(v3Pool) == address(0)) revert Errors.UniswapPoolNotInitialized();

186:             revert Errors.PoolAlreadyInitialized();

229:         if (amount0 > amount0Max || amount1 > amount1Max) revert Errors.PriceBoundFail();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

272:         if (address(s_univ3pool) != address(0)) revert Errors.PoolAlreadyInitialized();

316:         ) revert Errors.AccountInsolvent();

579:             revert Errors.PositionAlreadyMinted();

948:                     revert Errors.StaleTWAP();

1080:         if (touchedId.length != 1) revert Errors.InputListFail();

1204:         if (expectedSolvent && solvent != numberOfTicks) revert Errors.AccountInsolvent();

1205:         if (!expectedSolvent && solvent != 0) revert Errors.NotMarginCalled();

1300:         if (fingerprintIncomingList != currentHash) revert Errors.InputListFail();

1321:         if ((newHash >> 248) > MAX_POSITIONS) revert Errors.TooManyPositionsOpen();

1429:             revert Errors.EffectiveLiquidityAboveThreshold();

1531:         if (tokenId.isLong(legIndex) == 0 || legIndex > 3) revert Errors.NotALongLeg();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

310:         if (univ3pool == address(0)) revert Errors.UniswapPoolNotInitialized();

566:             ) revert Errors.TransferFailed();

727:         if (univ3pool == IUniswapV3Pool(address(0))) revert Errors.UniswapPoolNotInitialized();

779:             revert Errors.PositionTooLarge();

794:             revert Errors.PriceBoundFail();

846:             if (chunkLiquidity == 0) revert Errors.ZeroLiquidity();

867:                     revert Errors.NotEnoughLiquidity();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

37:             revert Errors.InvalidUniswapCallback();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Math.sol

131:             if (absTick > uint256(int256(Constants.MAX_V3POOL_TICK))) revert Errors.InvalidTick();

302:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) revert Errors.CastingError();

317:         if ((downcastedInt = int128(toCast)) < 0) revert Errors.CastingError();

324:         if (!((downcastedInt = int128(toCast)) == toCast)) revert Errors.CastingError();

331:         if (toCast > uint256(type(int256).max)) revert Errors.CastingError();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

456:             ) revert Errors.TicksNotInitializable();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

45:         if (!success) revert Errors.TransferFailed();

75:         if (!success) revert Errors.TransferFailed();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

101:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

117:                 revert UnsafeRecipient();

137:         if (!(msg.sender == from || isApprovedForAll[from][msg.sender])) revert NotAuthorized();

168:                 revert UnsafeRecipient();

227:                 revert UnsafeRecipient();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/types/LeftRight.sol

162:             ) revert Errors.UnderOverFlow();

185:             ) revert Errors.UnderOverFlow();

198:             if (left128 != left) revert Errors.UnderOverFlow();

203:             if (right128 != right) revert Errors.UnderOverFlow();

221:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

239:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

261:             if (left128 != left256 || right128 != right256) revert Errors.UnderOverFlow();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/TokenId.sol

598:         revert Errors.NoLegsExercisable();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-22"></a>[NC-22] Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)

While this won't save gas in the recent solidity versions, this is shorter and more readable (this is especially true in calculations).

*Instances (1)*:

```solidity
File: contracts/CollateralTracker.sol

224:         totalSupply = 10 ** 6;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="NC-23"></a>[NC-23] Strings should use double quotes rather than single quotes

See the Solidity Style Guide: <https://docs.soliditylang.org/en/v0.8.20/style-guide.html#other-recommendations>

*Instances (11)*:

```solidity
File: contracts/base/FactoryNFT.sol

77:                                 '{"name":"',

87:                                 '", "description":"',

97:                                 '", "attributes": [{"trait_type": "Rarity", "value": "',

103:                                 '"}, {"trait_type": "Strategy", "value": "',

105:                                 '"}, {"trait_type": "ChainId", "value": "',

107:                                 '"}], "image": "data:image/svg+xml;base64,',

109:                                 '"}'

228:                 '<g transform="translate(-',

230:                 ', 0)">',

251:             '<g transform="scale(0.0',

255:             ', 0)">',

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

### <a name="NC-24"></a>[NC-24] Contract does not follow the Solidity style guide's suggested layout ordering

The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be:

1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

However, the contract(s) below do not follow this ordering

*Instances (6)*:

```solidity
File: contracts/CollateralTracker.sol

1: 
   Current order:
   UsingForDirective.Math
   EventDefinition.Deposit
   EventDefinition.Withdraw
   VariableDeclaration.TICKER_PREFIX
   VariableDeclaration.NAME_PREFIX
   VariableDeclaration.DECIMALS
   VariableDeclaration.DECIMALS_128
   VariableDeclaration.s_underlyingToken
   VariableDeclaration.s_initialized
   VariableDeclaration.s_univ3token0
   VariableDeclaration.s_univ3token1
   VariableDeclaration.s_underlyingIsToken0
   VariableDeclaration.s_panopticPool
   VariableDeclaration.s_poolAssets
   VariableDeclaration.s_inAMM
   VariableDeclaration.s_ITMSpreadFee
   VariableDeclaration.s_poolFee
   VariableDeclaration.COMMISSION_FEE
   VariableDeclaration.SELLER_COLLATERAL_RATIO
   VariableDeclaration.BUYER_COLLATERAL_RATIO
   VariableDeclaration.FORCE_EXERCISE_COST
   VariableDeclaration.TARGET_POOL_UTIL
   VariableDeclaration.SATURATED_POOL_UTIL
   VariableDeclaration.ITM_SPREAD_MULTIPLIER
   ModifierDefinition.onlyPanopticPool
   FunctionDefinition.constructor
   FunctionDefinition.startToken
   FunctionDefinition.getPoolData
   FunctionDefinition.name
   FunctionDefinition.symbol
   FunctionDefinition.decimals
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.asset
   FunctionDefinition.totalAssets
   FunctionDefinition.convertToShares
   FunctionDefinition.convertToAssets
   FunctionDefinition.maxDeposit
   FunctionDefinition.previewDeposit
   FunctionDefinition.deposit
   FunctionDefinition.maxMint
   FunctionDefinition.previewMint
   FunctionDefinition.mint
   FunctionDefinition.maxWithdraw
   FunctionDefinition.previewWithdraw
   FunctionDefinition.withdraw
   FunctionDefinition.withdraw
   FunctionDefinition.maxRedeem
   FunctionDefinition.previewRedeem
   FunctionDefinition.redeem
   FunctionDefinition.exerciseCost
   FunctionDefinition._poolUtilization
   FunctionDefinition._sellCollateralRatio
   FunctionDefinition._buyCollateralRatio
   FunctionDefinition.delegate
   FunctionDefinition.revoke
   FunctionDefinition.settleLiquidation
   FunctionDefinition.refund
   FunctionDefinition.takeCommissionAddData
   FunctionDefinition.exercise
   FunctionDefinition._getExchangedAmount
   FunctionDefinition.getAccountMarginDetails
   FunctionDefinition._getTotalRequiredCollateral
   FunctionDefinition._getRequiredCollateralAtTickSinglePosition
   FunctionDefinition._getRequiredCollateralSingleLeg
   FunctionDefinition._getRequiredCollateralSingleLegNoPartner
   FunctionDefinition._getRequiredCollateralSingleLegPartner
   FunctionDefinition._getRequiredCollateralAtUtilization
   FunctionDefinition._computeSpread
   FunctionDefinition._computeStrangle
   
   Suggested order:
   UsingForDirective.Math
   VariableDeclaration.TICKER_PREFIX
   VariableDeclaration.NAME_PREFIX
   VariableDeclaration.DECIMALS
   VariableDeclaration.DECIMALS_128
   VariableDeclaration.s_underlyingToken
   VariableDeclaration.s_initialized
   VariableDeclaration.s_univ3token0
   VariableDeclaration.s_univ3token1
   VariableDeclaration.s_underlyingIsToken0
   VariableDeclaration.s_panopticPool
   VariableDeclaration.s_poolAssets
   VariableDeclaration.s_inAMM
   VariableDeclaration.s_ITMSpreadFee
   VariableDeclaration.s_poolFee
   VariableDeclaration.COMMISSION_FEE
   VariableDeclaration.SELLER_COLLATERAL_RATIO
   VariableDeclaration.BUYER_COLLATERAL_RATIO
   VariableDeclaration.FORCE_EXERCISE_COST
   VariableDeclaration.TARGET_POOL_UTIL
   VariableDeclaration.SATURATED_POOL_UTIL
   VariableDeclaration.ITM_SPREAD_MULTIPLIER
   EventDefinition.Deposit
   EventDefinition.Withdraw
   ModifierDefinition.onlyPanopticPool
   FunctionDefinition.constructor
   FunctionDefinition.startToken
   FunctionDefinition.getPoolData
   FunctionDefinition.name
   FunctionDefinition.symbol
   FunctionDefinition.decimals
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.asset
   FunctionDefinition.totalAssets
   FunctionDefinition.convertToShares
   FunctionDefinition.convertToAssets
   FunctionDefinition.maxDeposit
   FunctionDefinition.previewDeposit
   FunctionDefinition.deposit
   FunctionDefinition.maxMint
   FunctionDefinition.previewMint
   FunctionDefinition.mint
   FunctionDefinition.maxWithdraw
   FunctionDefinition.previewWithdraw
   FunctionDefinition.withdraw
   FunctionDefinition.withdraw
   FunctionDefinition.maxRedeem
   FunctionDefinition.previewRedeem
   FunctionDefinition.redeem
   FunctionDefinition.exerciseCost
   FunctionDefinition._poolUtilization
   FunctionDefinition._sellCollateralRatio
   FunctionDefinition._buyCollateralRatio
   FunctionDefinition.delegate
   FunctionDefinition.revoke
   FunctionDefinition.settleLiquidation
   FunctionDefinition.refund
   FunctionDefinition.takeCommissionAddData
   FunctionDefinition.exercise
   FunctionDefinition._getExchangedAmount
   FunctionDefinition.getAccountMarginDetails
   FunctionDefinition._getTotalRequiredCollateral
   FunctionDefinition._getRequiredCollateralAtTickSinglePosition
   FunctionDefinition._getRequiredCollateralSingleLeg
   FunctionDefinition._getRequiredCollateralSingleLegNoPartner
   FunctionDefinition._getRequiredCollateralSingleLegPartner
   FunctionDefinition._getRequiredCollateralAtUtilization
   FunctionDefinition._computeSpread
   FunctionDefinition._computeStrangle

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

1: 
   Current order:
   EventDefinition.PoolDeployed
   UsingForDirective.Clones
   VariableDeclaration.UNIV3_FACTORY
   VariableDeclaration.SFPM
   VariableDeclaration.POOL_REFERENCE
   VariableDeclaration.COLLATERAL_REFERENCE
   VariableDeclaration.WETH
   VariableDeclaration.FULL_RANGE_LIQUIDITY_AMOUNT_WETH
   VariableDeclaration.FULL_RANGE_LIQUIDITY_AMOUNT_TOKEN
   VariableDeclaration.CARDINALITY_INCREASE
   VariableDeclaration.s_getPanopticPool
   FunctionDefinition.constructor
   FunctionDefinition.uniswapV3MintCallback
   FunctionDefinition.deployNewPool
   FunctionDefinition.minePoolAddress
   FunctionDefinition._mintFullRange
   FunctionDefinition.getPanopticPool
   
   Suggested order:
   UsingForDirective.Clones
   VariableDeclaration.UNIV3_FACTORY
   VariableDeclaration.SFPM
   VariableDeclaration.POOL_REFERENCE
   VariableDeclaration.COLLATERAL_REFERENCE
   VariableDeclaration.WETH
   VariableDeclaration.FULL_RANGE_LIQUIDITY_AMOUNT_WETH
   VariableDeclaration.FULL_RANGE_LIQUIDITY_AMOUNT_TOKEN
   VariableDeclaration.CARDINALITY_INCREASE
   VariableDeclaration.s_getPanopticPool
   EventDefinition.PoolDeployed
   FunctionDefinition.constructor
   FunctionDefinition.uniswapV3MintCallback
   FunctionDefinition.deployNewPool
   FunctionDefinition.minePoolAddress
   FunctionDefinition._mintFullRange
   FunctionDefinition.getPanopticPool

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

1: 
   Current order:
   EventDefinition.AccountLiquidated
   EventDefinition.ForcedExercised
   EventDefinition.PremiumSettled
   EventDefinition.OptionBurnt
   EventDefinition.OptionMinted
   VariableDeclaration.MIN_SWAP_TICK
   VariableDeclaration.MAX_SWAP_TICK
   VariableDeclaration.COMPUTE_ALL_PREMIA
   VariableDeclaration.COMPUTE_LONG_PREMIA
   VariableDeclaration.ONLY_AVAILABLE_PREMIUM
   VariableDeclaration.COMMIT_LONG_SETTLED
   VariableDeclaration.DONOT_COMMIT_LONG_SETTLED
   VariableDeclaration.ASSERT_SOLVENCY
   VariableDeclaration.ASSERT_INSOLVENCY
   VariableDeclaration.ADD
   VariableDeclaration.TWAP_WINDOW
   VariableDeclaration.MAX_TWAP_DELTA_LIQUIDATION
   VariableDeclaration.MAX_TICKS_DELTA
   VariableDeclaration.MAX_SPREAD
   VariableDeclaration.MAX_POSITIONS
   VariableDeclaration.BP_DECREASE_BUFFER
   VariableDeclaration.NO_BUFFER
   VariableDeclaration.SFPM
   VariableDeclaration.s_univ3pool
   VariableDeclaration.s_miniMedian
   VariableDeclaration.s_collateralToken0
   VariableDeclaration.s_collateralToken1
   VariableDeclaration.s_options
   VariableDeclaration.s_grossPremiumLast
   VariableDeclaration.s_settledTokens
   VariableDeclaration.s_positionBalance
   VariableDeclaration.s_positionsHash
   FunctionDefinition.constructor
   FunctionDefinition.startPool
   FunctionDefinition.assertMinCollateralValues
   FunctionDefinition.validateCollateralWithdrawable
   FunctionDefinition.calculateAccumulatedFeesBatch
   FunctionDefinition._calculateAccumulatedPremia
   FunctionDefinition.pokeMedian
   FunctionDefinition.mintOptions
   FunctionDefinition.burnOptions
   FunctionDefinition.burnOptions
   FunctionDefinition._mintOptions
   FunctionDefinition._mintInSFPMAndUpdateCollateral
   FunctionDefinition._payCommissionAndWriteData
   FunctionDefinition._burnAllOptionsFrom
   FunctionDefinition._burnOptions
   FunctionDefinition._validateSolvency
   FunctionDefinition._checkSolvency
   FunctionDefinition._burnAndHandleExercise
   FunctionDefinition.liquidate
   FunctionDefinition.forceExercise
   FunctionDefinition._checkSolvencyAtTicks
   FunctionDefinition._isAccountSolvent
   FunctionDefinition.isSafeMode
   FunctionDefinition._validatePositionList
   FunctionDefinition._updatePositionsHash
   FunctionDefinition.univ3pool
   FunctionDefinition.collateralToken0
   FunctionDefinition.collateralToken1
   FunctionDefinition.getOracleTicks
   FunctionDefinition.numberOfPositions
   FunctionDefinition.positionData
   FunctionDefinition.getUniV3TWAP
   FunctionDefinition._checkLiquiditySpread
   FunctionDefinition._getPremia
   FunctionDefinition.settleLongPremium
   FunctionDefinition._updateSettlementPostMint
   FunctionDefinition._getAvailablePremium
   FunctionDefinition._getLiquidities
   FunctionDefinition._updateSettlementPostBurn
   
   Suggested order:
   VariableDeclaration.MIN_SWAP_TICK
   VariableDeclaration.MAX_SWAP_TICK
   VariableDeclaration.COMPUTE_ALL_PREMIA
   VariableDeclaration.COMPUTE_LONG_PREMIA
   VariableDeclaration.ONLY_AVAILABLE_PREMIUM
   VariableDeclaration.COMMIT_LONG_SETTLED
   VariableDeclaration.DONOT_COMMIT_LONG_SETTLED
   VariableDeclaration.ASSERT_SOLVENCY
   VariableDeclaration.ASSERT_INSOLVENCY
   VariableDeclaration.ADD
   VariableDeclaration.TWAP_WINDOW
   VariableDeclaration.MAX_TWAP_DELTA_LIQUIDATION
   VariableDeclaration.MAX_TICKS_DELTA
   VariableDeclaration.MAX_SPREAD
   VariableDeclaration.MAX_POSITIONS
   VariableDeclaration.BP_DECREASE_BUFFER
   VariableDeclaration.NO_BUFFER
   VariableDeclaration.SFPM
   VariableDeclaration.s_univ3pool
   VariableDeclaration.s_miniMedian
   VariableDeclaration.s_collateralToken0
   VariableDeclaration.s_collateralToken1
   VariableDeclaration.s_options
   VariableDeclaration.s_grossPremiumLast
   VariableDeclaration.s_settledTokens
   VariableDeclaration.s_positionBalance
   VariableDeclaration.s_positionsHash
   EventDefinition.AccountLiquidated
   EventDefinition.ForcedExercised
   EventDefinition.PremiumSettled
   EventDefinition.OptionBurnt
   EventDefinition.OptionMinted
   FunctionDefinition.constructor
   FunctionDefinition.startPool
   FunctionDefinition.assertMinCollateralValues
   FunctionDefinition.validateCollateralWithdrawable
   FunctionDefinition.calculateAccumulatedFeesBatch
   FunctionDefinition._calculateAccumulatedPremia
   FunctionDefinition.pokeMedian
   FunctionDefinition.mintOptions
   FunctionDefinition.burnOptions
   FunctionDefinition.burnOptions
   FunctionDefinition._mintOptions
   FunctionDefinition._mintInSFPMAndUpdateCollateral
   FunctionDefinition._payCommissionAndWriteData
   FunctionDefinition._burnAllOptionsFrom
   FunctionDefinition._burnOptions
   FunctionDefinition._validateSolvency
   FunctionDefinition._checkSolvency
   FunctionDefinition._burnAndHandleExercise
   FunctionDefinition.liquidate
   FunctionDefinition.forceExercise
   FunctionDefinition._checkSolvencyAtTicks
   FunctionDefinition._isAccountSolvent
   FunctionDefinition.isSafeMode
   FunctionDefinition._validatePositionList
   FunctionDefinition._updatePositionsHash
   FunctionDefinition.univ3pool
   FunctionDefinition.collateralToken0
   FunctionDefinition.collateralToken1
   FunctionDefinition.getOracleTicks
   FunctionDefinition.numberOfPositions
   FunctionDefinition.positionData
   FunctionDefinition.getUniV3TWAP
   FunctionDefinition._checkLiquiditySpread
   FunctionDefinition._getPremia
   FunctionDefinition.settleLongPremium
   FunctionDefinition._updateSettlementPostMint
   FunctionDefinition._getAvailablePremium
   FunctionDefinition._getLiquidities
   FunctionDefinition._updateSettlementPostBurn

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

1: 
   Current order:
   EventDefinition.PoolInitialized
   EventDefinition.TokenizedPositionBurnt
   EventDefinition.TokenizedPositionMinted
   UsingForDirective.Math
   UsingForDirective.Math
   VariableDeclaration.MINT
   VariableDeclaration.BURN
   VariableDeclaration.VEGOID
   VariableDeclaration.FACTORY
   VariableDeclaration.s_AddrToPoolIdData
   VariableDeclaration.s_poolIdToAddr
   VariableDeclaration.s_accountLiquidity
   VariableDeclaration.s_accountPremiumOwed
   VariableDeclaration.s_accountPremiumGross
   VariableDeclaration.s_accountFeesBase
   FunctionDefinition.constructor
   FunctionDefinition.initializeAMMPool
   FunctionDefinition.uniswapV3MintCallback
   FunctionDefinition.uniswapV3SwapCallback
   FunctionDefinition.burnTokenizedPosition
   FunctionDefinition.mintTokenizedPosition
   FunctionDefinition.safeTransferFrom
   FunctionDefinition.safeBatchTransferFrom
   FunctionDefinition.registerTokenTransfer
   FunctionDefinition.swapInAMM
   FunctionDefinition._createPositionInAMM
   FunctionDefinition._createLegInAMM
   FunctionDefinition._updateStoredPremia
   FunctionDefinition._getFeesBase
   FunctionDefinition._mintLiquidity
   FunctionDefinition._burnLiquidity
   FunctionDefinition._collectAndWritePositionData
   FunctionDefinition._getPremiaDeltas
   FunctionDefinition.getAccountLiquidity
   FunctionDefinition.getAccountPremium
   FunctionDefinition.getAccountFeesBase
   FunctionDefinition.getUniswapV3PoolFromId
   FunctionDefinition.getPoolId
   
   Suggested order:
   UsingForDirective.Math
   UsingForDirective.Math
   VariableDeclaration.MINT
   VariableDeclaration.BURN
   VariableDeclaration.VEGOID
   VariableDeclaration.FACTORY
   VariableDeclaration.s_AddrToPoolIdData
   VariableDeclaration.s_poolIdToAddr
   VariableDeclaration.s_accountLiquidity
   VariableDeclaration.s_accountPremiumOwed
   VariableDeclaration.s_accountPremiumGross
   VariableDeclaration.s_accountFeesBase
   EventDefinition.PoolInitialized
   EventDefinition.TokenizedPositionBurnt
   EventDefinition.TokenizedPositionMinted
   FunctionDefinition.constructor
   FunctionDefinition.initializeAMMPool
   FunctionDefinition.uniswapV3MintCallback
   FunctionDefinition.uniswapV3SwapCallback
   FunctionDefinition.burnTokenizedPosition
   FunctionDefinition.mintTokenizedPosition
   FunctionDefinition.safeTransferFrom
   FunctionDefinition.safeBatchTransferFrom
   FunctionDefinition.registerTokenTransfer
   FunctionDefinition.swapInAMM
   FunctionDefinition._createPositionInAMM
   FunctionDefinition._createLegInAMM
   FunctionDefinition._updateStoredPremia
   FunctionDefinition._getFeesBase
   FunctionDefinition._mintLiquidity
   FunctionDefinition._burnLiquidity
   FunctionDefinition._collectAndWritePositionData
   FunctionDefinition._getPremiaDeltas
   FunctionDefinition.getAccountLiquidity
   FunctionDefinition.getAccountPremium
   FunctionDefinition.getAccountFeesBase
   FunctionDefinition.getUniswapV3PoolFromId
   FunctionDefinition.getPoolId

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

1: 
   Current order:
   EventDefinition.TransferSingle
   EventDefinition.TransferBatch
   EventDefinition.ApprovalForAll
   ErrorDefinition.NotAuthorized
   ErrorDefinition.UnsafeRecipient
   VariableDeclaration.balanceOf
   VariableDeclaration.isApprovedForAll
   FunctionDefinition.setApprovalForAll
   FunctionDefinition.safeTransferFrom
   FunctionDefinition.safeBatchTransferFrom
   FunctionDefinition.balanceOfBatch
   FunctionDefinition.supportsInterface
   FunctionDefinition._mint
   FunctionDefinition._burn
   
   Suggested order:
   VariableDeclaration.balanceOf
   VariableDeclaration.isApprovedForAll
   ErrorDefinition.NotAuthorized
   ErrorDefinition.UnsafeRecipient
   EventDefinition.TransferSingle
   EventDefinition.TransferBatch
   EventDefinition.ApprovalForAll
   FunctionDefinition.setApprovalForAll
   FunctionDefinition.safeTransferFrom
   FunctionDefinition.safeBatchTransferFrom
   FunctionDefinition.balanceOfBatch
   FunctionDefinition.supportsInterface
   FunctionDefinition._mint
   FunctionDefinition._burn

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

1: 
   Current order:
   EventDefinition.Transfer
   EventDefinition.Approval
   VariableDeclaration.totalSupply
   VariableDeclaration.balanceOf
   VariableDeclaration.allowance
   FunctionDefinition.approve
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition._transferFrom
   FunctionDefinition._mint
   FunctionDefinition._burn
   
   Suggested order:
   VariableDeclaration.totalSupply
   VariableDeclaration.balanceOf
   VariableDeclaration.allowance
   EventDefinition.Transfer
   EventDefinition.Approval
   FunctionDefinition.approve
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition._transferFrom
   FunctionDefinition._mint
   FunctionDefinition._burn

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

### <a name="NC-25"></a>[NC-25] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (22)*:

```solidity
File: contracts/base/FactoryNFT.sol

183:         } else if (block.chainid == 42161) {

185:         } else if (block.chainid == 8453) {

187:         } else if (block.chainid == 43114) {

193:         } else if (block.chainid == 42220) {

267:             width = 1600;

269:             width = 1350;

271:             width = 1450;

273:             width = 1350;

275:             width = 1250;

277:             width = 1350;

279:             width = 1450;

281:             width = 1350;

283:             width = 1600;

323:             width = 9000;

325:             width = 3900;

327:             width = 9000;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Constants.sol

15:     int24 internal constant MAX_V3POOL_TICK = 887272;

18:     uint160 internal constant MIN_V3POOL_SQRT_RATIO = 4295128739;

22:         1461446703485210103287273052203988822378723970342;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Constants.sol)

```solidity
File: contracts/types/TokenId.sol

171:             return int24(int256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 36)) % 4096));

172:         } // "% 4096" = take last (2 ** 12 = 4096) 12 bits

320:                         (uint256(uint24(_width) % 4096) << (64 + legIndex * 48 + 36))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-26"></a>[NC-26] Internal and private variables and functions names should begin with an underscore

According to the Solidity Style Guide, Non-`external` variable and function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*Instances (160)*:

```solidity
File: contracts/CollateralTracker.sol

89:     address internal s_underlyingToken;

93:     bool internal s_initialized;

96:     address internal s_univ3token0;

99:     address internal s_univ3token1;

102:     bool internal s_underlyingIsToken0;

109:     PanopticPool internal s_panopticPool;

112:     uint128 internal s_poolAssets;

115:     uint128 internal s_inAMM;

121:     uint128 internal s_ITMSpreadFee;

124:     uint24 internal s_poolFee;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

90:     mapping(IUniswapV3Pool univ3pool => PanopticPool panopticPool) internal s_getPanopticPool;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

156:     IUniswapV3Pool internal s_univ3pool;

195:     uint256 internal s_miniMedian;

202:     CollateralTracker internal s_collateralToken0;

204:     CollateralTracker internal s_collateralToken1;

209:     mapping(address account => mapping(TokenId tokenId => mapping(uint256 leg => LeftRightUnsigned premiaGrowth)))

216:     mapping(bytes32 chunkKey => LeftRightUnsigned lastGrossPremium) internal s_grossPremiumLast;

222:     mapping(bytes32 chunkKey => LeftRightUnsigned settledTokens) internal s_settledTokens;

229:     mapping(address account => mapping(TokenId tokenId => PositionBalance positionBalance))

245:     mapping(address account => uint256 positionsHash) internal s_positionsHash;

1395:     function getUniV3TWAP() internal view returns (int24 twapTick) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

152:           │  │    │          mints      

155:           │  │    │         │    │         │    │     

182:         Let`s say Charlie the smart contract deposited T into the AMM and later removed R from that

287:     /// @dev feesBase represents the baseline fees collected by the position last time it was updated - this is recalculated every time the position is collected from with the new value.

290:     /*//////////////////////////////////////////////////////////////

304:     /// @param fee The fee level of the of the underlying Uniswap v3 pool, denominated in hundredths of bips

551:                 abi.encodePacked(

633:                     payer: msg.sender

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

121:     function generateSVGArt(

153:     function generateSVGInfo(

178:     function getChainName() internal view returns (string memory) {

205:     function write(string memory chars) internal view returns (string memory) {

213:     function write(

265:     function maxSymbolWidth(uint256 rarity) internal pure returns (uint256 width) {

291:     function maxRarityLabelWidth(uint256 rarity) internal pure returns (uint256 width) {

321:     function maxStrategyLabelWidth(uint256 rarity) internal pure returns (uint256 width) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

17:     mapping(bytes32 property => mapping(uint256 index => Pointer pointer)) internal metadata;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

30:     function validateCallback(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Math.sol

25:     function min24(int24 a, int24 b) internal pure returns (int24) {

33:     function max24(int24 a, int24 b) internal pure returns (int24) {

41:     function min(uint256 a, uint256 b) internal pure returns (uint256) {

49:     function min(int256 a, int256 b) internal pure returns (int256) {

57:     function max(uint256 a, uint256 b) internal pure returns (uint256) {

65:     function max(int256 a, int256 b) internal pure returns (int256) {

73:     function abs(int256 x) internal pure returns (int256) {

81:     function absUint(int256 x) internal pure returns (uint256) {

91:     function mostSignificantNibble(uint160 x) internal pure returns (uint256 r) {

128:     function getSqrtRatioAtTick(int24 tick) internal pure returns (uint160) {

196:     function getAmount0ForLiquidity(LiquidityChunk liquidityChunk) internal pure returns (uint256) {

212:     function getAmount1ForLiquidity(LiquidityChunk liquidityChunk) internal pure returns (uint256) {

226:     function getAmountsForLiquidity(

246:     function getLiquidityForAmount0(

276:     function getLiquidityForAmount1(

301:     function toUint128(uint256 toDowncast) internal pure returns (uint128 downcastedInt) {

307:     function toUint128Capped(uint256 toDowncast) internal pure returns (uint128 downcastedInt) {

316:     function toInt128(uint128 toCast) internal pure returns (int128 downcastedInt) {

323:     function toInt128(int256 toCast) internal pure returns (int128 downcastedInt) {

330:     function toInt256(uint256 toCast) internal pure returns (int256) {

345:     function mulDiv(

445:     function mulDivCapped(

543:     function mulDivRoundingUp(

561:     function mulDiv64(uint256 a, uint256 b) internal pure returns (uint256) {

624:     function mulDiv96(uint256 a, uint256 b) internal pure returns (uint256) {

687:     function mulDiv96RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

701:     function mulDiv128(uint256 a, uint256 b) internal pure returns (uint256) {

764:     function mulDiv128RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

778:     function mulDiv192(uint256 a, uint256 b) internal pure returns (uint256) {

841:     function mulDiv192RoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

855:     function unsafeDivRoundingUp(uint256 a, uint256 b) internal pure returns (uint256 result) {

870:     function quickSort(int256[] memory arr, int256 left, int256 right) internal pure {

893:     function sort(int256[] memory data) internal pure returns (int256[] memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

49:     function getPoolId(address univ3pool) internal view returns (uint64) {

61:     function incrementPoolPattern(uint64 poolId) internal pure returns (uint64) {

99:     function uniswapFeeToString(uint24 fee) internal pure returns (string memory) {

125:     function updatePositionsHash(

220:     function computeMedianObservedPrice(

387:         uint128 positionSize

437:     ) internal pure returns (int24 tickLower, int24 tickUpper) {

469:     ) internal pure returns (int24, int24) {

488:     ) internal pure returns (LeftRightSigned longAmounts, LeftRightSigned shortAmounts) {

512:     function convert0to1(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

532:     ) internal pure returns (uint256) {

549:     function convert1to0(uint256 amount, uint160 sqrtPriceX96) internal pure returns (uint256) {

569:     ) internal pure returns (uint256) {

591:     function convert0to1(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

614:     function convert1to0(int256 amount, uint160 sqrtPriceX96) internal pure returns (int256) {

644:         LeftRightUnsigned tokenData1,

672:         uint256 legIndex

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

21:     function safeTransferFrom(address token, address from, address to, uint256 amount) internal {

52:     function safeTransfer(address token, address to, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/types/LeftRight.sol

38:     function rightSlot(LeftRightUnsigned self) internal pure returns (uint128) {

45:     function rightSlot(LeftRightSigned self) internal pure returns (int128) {

58:     function toRightSlot(

77:     function toRightSlot(

100:     function leftSlot(LeftRightUnsigned self) internal pure returns (uint128) {

107:     function leftSlot(LeftRightSigned self) internal pure returns (int128) {

120:     function toLeftSlot(

133:     function toLeftSlot(LeftRightSigned self, int128 left) internal pure returns (LeftRightSigned) {

147:     function add(

170:     function sub(

193:     function add(LeftRightUnsigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

213:     function add(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

231:     function sub(LeftRightSigned x, LeftRightSigned y) internal pure returns (LeftRightSigned z) {

250:     function subRect(

278:     function addCapped(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

75:         unchecked {

94:             return LiquidityChunk.wrap(LiquidityChunk.unwrap(self) + amount);

107:             return

123:             // convert tick upper to uint24 as explicit conversion from int24 to uint256 is not allowed

139:         unchecked {

155:         unchecked {

173:             return int24(int256(LiquidityChunk.unwrap(self) >> 232));

182:             return int24(int256(LiquidityChunk.unwrap(self) >> 208));

191:             return uint128(LiquidityChunk.unwrap(self));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

17:     function createPointer(

29:     function location(Pointer self) internal pure returns (address) {

36:     function start(Pointer self) internal pure returns (uint48) {

43:     function size(Pointer self) internal pure returns (uint48) {

50:     function data(Pointer self) internal view returns (bytes memory) {

67:     function dataStr(Pointer self) internal view returns (string memory) {

74:     function decompressedDataStr(Pointer self) internal view returns (string memory) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

42:     function storeBalanceData(

63:     function packTickData(

85:     function lastObservedTick(PositionBalance self) internal pure returns (int24) {

94:     function slowOracleTick(PositionBalance self) internal pure returns (int24) {

103:     function fastOracleTick(PositionBalance self) internal pure returns (int24) {

112:     function currentTick(PositionBalance self) internal pure returns (int24) {

121:     function tickData(PositionBalance self) internal pure returns (uint96) {

133:     function unpackTickData(uint96 _tickData) internal pure returns (int24, int24, int24, int24) {

146:     function utilization0(PositionBalance self) internal pure returns (int256) {

155:     function utilization1(PositionBalance self) internal pure returns (int256) {

164:     function utilizations(PositionBalance self) internal pure returns (uint32) {

173:     function positionSize(PositionBalance self) internal pure returns (uint128) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

87:     function poolId(TokenId self) internal pure returns (uint64) {

96:     function tickSpacing(TokenId self) internal pure returns (int24) {

108:     function asset(TokenId self, uint256 legIndex) internal pure returns (uint256) {

118:     function optionRatio(TokenId self, uint256 legIndex) internal pure returns (uint256) {

128:     function isLong(TokenId self, uint256 legIndex) internal pure returns (uint256) {

138:     function tokenType(TokenId self, uint256 legIndex) internal pure returns (uint256) {

148:     function riskPartner(TokenId self, uint256 legIndex) internal pure returns (uint256) {

158:     function strike(TokenId self, uint256 legIndex) internal pure returns (int24) {

169:     function width(TokenId self, uint256 legIndex) internal pure returns (int24) {

183:     function addPoolId(TokenId self, uint64 _poolId) internal pure returns (TokenId) {

193:     function addTickSpacing(TokenId self, int24 _tickSpacing) internal pure returns (TokenId) {

205:     function addAsset(

221:     function addOptionRatio(

240:     function addIsLong(

255:     function addTokenType(

273:     function addRiskPartner(

291:     function addStrike(

310:     function addWidth(

336:     function addLeg(

366:     function flipToBurnToken(TokenId self) internal pure returns (TokenId) {

404:     function countLongs(TokenId self) internal pure returns (uint256) {

416:     function asTicks(

432:     function countLegs(TokenId self) internal pure returns (uint256) {

464:     function clearLeg(TokenId self, uint256 i) internal pure returns (TokenId) {

500:     function validate(TokenId self) internal pure {

578:     function validateIsExercisable(TokenId self, int24 currentTick) internal pure {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-27"></a>[NC-27] Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (12)*:

```solidity
File: contracts/CollateralTracker.sol

49:     event Deposit(address indexed sender, address indexed owner, uint256 assets, uint256 shares);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

40:     event PoolDeployed(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

36:     event AccountLiquidated(

59:     event PremiumSettled(

70:     event OptionBurnt(

84:     event OptionMinted(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

94:     /// @dev `recipient` is used to track whether it was minted directly by the user or through an option contract.

99:         address indexed caller,

115:     /// @notice Flag used to indicate a regular position mint.

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

48:     event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

18:     event Transfer(address indexed from, address indexed to, uint256 amount);

24:     event Approval(address indexed owner, address indexed spender, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

### <a name="NC-28"></a>[NC-28] Constants should be defined rather than using magic numbers

*Instances (33)*:

```solidity
File: contracts/PanopticPool.sol

1258:                 uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/base/FactoryNFT.sol

240:             uint256 _scale = (3400 * maxWidth) / offset;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Math.sol

581:                     res := shr(64, prod0)

608:                 prod0 := shr(64, prod0)

644:                     res := shr(96, prod0)

671:                 prod0 := shr(96, prod0)

798:                     res := shr(192, prod0)

825:                 prod0 := shr(192, prod0)

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

337:         uint32[] memory secondsAgos = new uint32[](20);

339:         int256[] memory twapMeasurement = new int256[](19);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

31:             mstore(add(36, p), to) // Append the "to" argument.

32:             mstore(add(68, p), amount) // Append the "amount" argument.

62:             mstore(add(36, p), amount) // Append the "amount" argument.

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/types/TokenId.sol

110:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48)) % 2);

120:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 1)) % 128);

130:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 8)) % 2);

140:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 9)) % 2);

150:             return uint256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 10)) % 4);

160:             return int24(int256(TokenId.unwrap(self) >> (64 + legIndex * 48 + 12)));

171:             return int24(int256((TokenId.unwrap(self) >> (64 + legIndex * 48 + 36)) % 4096));

212:                 TokenId.wrap(TokenId.unwrap(self) + (uint256(_asset % 2) << (64 + legIndex * 48)));

229:                     TokenId.unwrap(self) + (uint256(_optionRatio % 128) << (64 + legIndex * 48 + 1))

246:             return TokenId.wrap(TokenId.unwrap(self) + ((_isLong % 2) << (64 + legIndex * 48 + 8)));

263:                     TokenId.unwrap(self) + (uint256(_tokenType % 2) << (64 + legIndex * 48 + 9))

281:                     TokenId.unwrap(self) + (uint256(_riskPartner % 4) << (64 + legIndex * 48 + 10))

300:                         uint256((int256(_strike) & BITMASK_INT24) << (64 + legIndex * 48 + 12))

320:                         (uint256(uint24(_width) % 4096) << (64 + legIndex * 48 + 36))

395:                         ((LONG_MASK >> (48 * (4 - optionRatios))) & CLEAR_POOLID_MASK)

512:                     if ((TokenId.unwrap(self) >> (64 + 48 * i)) != 0)

521:                     if (uint48(chunkData >> (48 * i)) == uint48(chunkData >> (48 * j))) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="NC-29"></a>[NC-29] `public` functions not called by the contract should be declared `external` instead

*Instances (7)*:

```solidity
File: contracts/CollateralTracker.sol

1141:     function getAccountMarginDetails(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/base/Multicall.sol

12:     function multicall(bytes[] calldata data) public payable returns (bytes[] memory results) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/FeesCalc.sol

52:         // extract the amount of AMM fees collected within the liquidity chunk

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/FeesCalc.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

81:     function setApprovalForAll(address operator, bool approved) public {

178:     function balanceOfBatch(

200:     function supportsInterface(bytes4 interfaceId) public pure returns (bool) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/tokens/ERC20Minimal.sol

49:     function approve(address spender, uint256 amount) public returns (bool) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC20Minimal.sol)

### <a name="NC-30"></a>[NC-30] Variables need not be initialized to zero

The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (30)*:

```solidity
File: contracts/CollateralTracker.sol

680:             for (uint256 leg = 0; leg < positionId.countLegs(); ++leg) {

1174:         for (uint256 i = 0; i < totalIterations; ) {

1225:             for (uint256 index = 0; index < numLegs; ++index) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

386:         for (uint256 k = 0; k < pLength; ) {

404:             for (uint256 leg = 0; leg < numLegs; ) {

722:         for (uint256 i = 0; i < positionIdList.length; ) {

1288:         for (uint256 i = 0; i < pLength; ) {

1456:         for (uint256 leg = 0; leg < numLegs; ) {

1614:         for (uint256 leg = 0; leg < numLegs; ++leg) {

1816:         for (uint256 leg = 0; leg < numLegs; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

512:         for (uint256 i = 0; i < ids.length; ) {

534:         for (uint256 leg = 0; leg < numLegs; ) {

736:         for (uint256 leg = 0; leg < numLegs; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

220:         for (uint256 i = 0; i < bytes(chars).length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

28:         for (uint256 i = 0; i < properties.length; i++) {

29:             for (uint256 j = 0; j < indices[i].length; j++) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/base/Multicall.sol

14:         for (uint256 i = 0; i < data.length; ) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/Multicall.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

232:             for (uint256 i = 0; i < cardinality + 1; ++i) {

243:             for (uint256 i = 0; i < cardinality; ++i) {

343:             for (uint256 i = 0; i < 20; ++i) {

351:             for (uint256 i = 0; i < 19; ++i) {

490:         for (uint256 leg = 0; leg < numLegs; ) {

893:             for (uint256 i = 0; i < positionIdList.length; ++i) {

896:                 for (uint256 leg = 0; leg < numLegs; ++leg) {

973:             for (uint256 i = 0; i < positionIdList.length; i++) {

976:                 for (uint256 leg = 0; leg < tokenId.countLegs(); ++leg) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/tokens/ERC1155Minimal.sol

143:         for (uint256 i = 0; i < ids.length; ) {

187:             for (uint256 i = 0; i < owners.length; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/tokens/ERC1155Minimal.sol)

```solidity
File: contracts/types/TokenId.sol

507:             for (uint256 i = 0; i < 4; ++i) {

581:             for (uint256 i = 0; i < numLegs; ++i) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

## Low Issues

| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | `approve()`/`safeApprove()` may revert if the current approval is not zero | 4 |
| [L-2](#L-2) | Some tokens may revert when zero value transfers are made | 2 |
| [L-3](#L-3) | Missing checks for `address(0)` when assigning values to address state variables | 5 |
| [L-4](#L-4) | `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` | 18 |
| [L-5](#L-5) | `decimals()` is not a part of the ERC-20 standard | 1 |
| [L-6](#L-6) | Deprecated approve() function | 4 |
| [L-7](#L-7) | Division by zero not prevented | 17 |
| [L-8](#L-8) | External calls in an un-bounded `for-`loop may result in a DOS | 14 |
| [L-9](#L-9) | Prevent accidentally burning tokens | 27 |
| [L-10](#L-10) | NFT ownership doesn't support hard forks | 1 |
| [L-11](#L-11) | Possible rounding issue | 6 |
| [L-12](#L-12) | Loss of precision | 12 |
| [L-13](#L-13) | Solidity version 0.8.20+ may not work on other chains due to `PUSH0` | 19 |
| [L-14](#L-14) | `symbol()` is not a part of the ERC-20 standard | 1 |
| [L-15](#L-15) | Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting | 98 |
| [L-16](#L-16) | Unsafe ERC20 operation(s) | 6 |
| [L-17](#L-17) | Upgradeable contract not initialized | 15 |

### <a name="L-1"></a>[L-1] `approve()`/`safeApprove()` may revert if the current approval is not zero

- Some tokens (like the *very popular* USDT) do not work when changing the allowance from an existing non-zero allowance value (it will revert if the current approval is not zero to protect against front-running changes of approvals). These tokens must first be approved for zero and then the actual allowance can be approved.
- Furthermore, OZ's implementation of safeApprove would throw an error if an approve is attempted from a non-zero value (`"SafeERC20: approve from non-zero to non-zero allowance"`)

Set the allowance to zero immediately before each of the existing allowance calls

*Instances (4)*:

```solidity
File: contracts/libraries/InteractionHelper.sol

32:         IERC20Partial(token0).approve(address(sfpm), type(uint256).max);

33:         IERC20Partial(token1).approve(address(sfpm), type(uint256).max);

36:         IERC20Partial(token0).approve(address(ct0), type(uint256).max);

37:         IERC20Partial(token1).approve(address(ct1), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

### <a name="L-2"></a>[L-2] Some tokens may revert when zero value transfers are made

Example: <https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers>.

In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*Instances (2)*:

```solidity
File: contracts/CollateralTracker.sol

318:         return ERC20Minimal.transfer(recipient, amount);

337:         return ERC20Minimal.transferFrom(from, to, amount);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="L-3"></a>[L-3] Missing checks for `address(0)` when assigning values to address state variables

*Instances (5)*:

```solidity
File: contracts/CollateralTracker.sol

240:         s_univ3token0 = token0;

241:         s_univ3token1 = token1;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

115:         WETH = _WETH9;

118:         POOL_REFERENCE = _poolReference;

119:         COLLATERAL_REFERENCE = _collateralReference;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

### <a name="L-4"></a>[L-4] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`

Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).
If all arguments are strings and or bytes, `bytes.concat()` should be used instead

*Instances (18)*:

```solidity
File: contracts/base/FactoryNFT.sol

73:                     "data:application/json;base64,",

74:                     Base64.encode(

77:                                 '{"name":"',

78:                                 abi.encodePacked(

79:                                     LibString.toHexString(uint256(uint160(panopticPool)), 20),

80:                                     "-",

81:                                     string.concat(

87:                                 '", "description":"',

88:                                 string.concat(

97:                                 '", "attributes": [{"trait_type": "Rarity", "value": "',

98:                                 string.concat(

103:                                 '"}, {"trait_type": "Strategy", "value": "',

104:                                 metadata[bytes32("strategies")][lastCharVal].dataStr(),

105:                                 '"}, {"trait_type": "ChainId", "value": "',

106:                                 getChainName(),

107:                                 '"}], "image": "data:image/svg+xml;base64,',

108:                                 Base64.encode(bytes(svgOut)),

109:                                 '"}'

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

### <a name="L-5"></a>[L-5] `decimals()` is not a part of the ERC-20 standard

The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (1)*:

```solidity
File: contracts/libraries/InteractionHelper.sol

91:         try IERC20Metadata(token).decimals() returns (uint8 _decimals) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

### <a name="L-6"></a>[L-6] Deprecated approve() function

Due to the inheritance of ERC20's approve function, there's a vulnerability to the ERC20 approve and double spend front running attack. Briefly, an authorized spender could spend both allowances by front running an allowance-changing transaction. Consider implementing OpenZeppelin's `.safeApprove()` function to help mitigate this.

*Instances (4)*:

```solidity
File: contracts/libraries/InteractionHelper.sol

32:         IERC20Partial(token0).approve(address(sfpm), type(uint256).max);

33:         IERC20Partial(token1).approve(address(sfpm), type(uint256).max);

36:         IERC20Partial(token0).approve(address(ct0), type(uint256).max);

37:         IERC20Partial(token1).approve(address(ct1), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

### <a name="L-7"></a>[L-7] Division by zero not prevented

The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (17)*:

```solidity
File: contracts/CollateralTracker.sol

695:                         uint256(Math.abs(currentTick - positionId.strike(leg)) / range)

755:             return (s_inAMM * DECIMALS) / totalAssets();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

371:             tickLower = (Constants.MIN_V3POOL_TICK / tickSpacing) * tickSpacing;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

1423:             effectiveLiquidityFactorX32 = (uint256(removedLiquidity) * 2 ** 32) / netLiquidity;

1691:                                     totalLiquidityBefore) / totalLiquidity

1699:                                     totalLiquidityBefore) / totalLiquidity

1914:                                     ) / totalLiquidity

1930:                                     ) / totalLiquidity

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

220:               s_accountPremiumOwed += feeGrowthX128 * R * (1 + ν*R/N) / R

263:                                 = ∆feeGrowthX128 * t * (T - R + ν*R^2/T) / N 

264:                                 = ∆feeGrowthX128 * t * (N + ν*R^2/T) / N

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

240:             uint256 _scale = (3400 * maxWidth) / offset;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Math.sol

181:             if (tick > 0) sqrtR = type(uint256).max / sqrtR;

205:                 ) / lowPriceX96;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

353:                     (tickCumulatives[i] - tickCumulatives[i + 1]) / int56(uint56(twapWindow / 20))

441:             int24 minTick = (Constants.MIN_V3POOL_TICK / tickSpacing) * tickSpacing;

442:             int24 maxTick = (Constants.MAX_V3POOL_TICK / tickSpacing) * tickSpacing;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="L-8"></a>[L-8] External calls in an un-bounded `for-`loop may result in a DOS

Consider limiting the number of iterations in for-loops that make external calls

*Instances (14)*:

```solidity
File: contracts/PanopticPool.sol

1622:             s_settledTokens[chunkKey] = s_settledTokens[chunkKey].add(collectedByLeg[leg]);

1824:             LeftRightUnsigned settledTokens = s_settledTokens[chunkKey].add(collectedByLeg[leg]);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/base/FactoryNFT.sol

223:                 bytes32(metadata[bytes32("charOffsets")][uint256(bytes32(bytes(chars)[i]))].data())

232:                 metadata[bytes32("charPaths")][uint256(bytes32(bytes(chars)[i]))].dataStr(),

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

984:                             uint128(longPremium.rightSlot())

984:                             uint128(longPremium.rightSlot())

988:                             uint128(longPremium.leftSlot())

988:                             uint128(longPremium.leftSlot())

1005:                         settled1 = Math.max(

1005:                         settled1 = Math.max(

1010:                         _settledTokens[chunkKey] = _settledTokens[chunkKey].add(

1010:                         _settledTokens[chunkKey] = _settledTokens[chunkKey].add(

1011:                             LeftRightUnsigned.wrap(uint128(settled0)).toLeftSlot(uint128(settled1))

1011:                             LeftRightUnsigned.wrap(uint128(settled0)).toLeftSlot(uint128(settled1))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="L-9"></a>[L-9] Prevent accidentally burning tokens

Minting and burning tokens to address(0) prevention

*Instances (27)*:

```solidity
File: contracts/CollateralTracker.sol

417:         _mint(receiver, shares);

473:         _mint(receiver, shares);

527:         _burn(owner, shares);

569:         _burn(owner, shares);

628:         _burn(owner, shares);

905:             _mint(liquidatee, convertToShares(bonusAbs));

959:                         liquidator,

1024:                 _burn(optionOwner, sharesToBurn);

1028:                 _mint(optionOwner, sharesToMint);

1073:                 _burn(optionOwner, sharesToBurn);

1077:                 _mint(optionOwner, sharesToMint);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

227:         (uint256 amount0, uint256 amount1) = _mintFullRange(v3Pool, token0, token1, fee);

233:         _mint(msg.sender, tokenId);

383:             IUniswapV3Pool(v3Pool).mint(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

492:         _mintOptions(

512:         _burnOptions(COMMIT_LONG_SETTLED, tokenId, msg.sender, tickLimitLow, tickLimitHigh);

531:         _burnAllOptionsFrom(

582:         uint32 poolUtilizations = _mintInSFPMAndUpdateCollateral(

724:             (paidAmounts, premiasByLeg[i]) = _burnOptions(

757:         (premiaOwed, premiaByLeg, paidAmounts) = _burnAndHandleExercise(

1012:             (netPaid, premiasByLeg) = _burnAllOptionsFrom(

1118:         _burnAllOptionsFrom(account, MIN_SWAP_TICK, MAX_SWAP_TICK, COMMIT_LONG_SETTLED, touchedId);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

445:     /// @param slippageTickLimitLow The lower bound of an acceptable open interval for the ending price

478:     /// @notice Transfer a single token from one user to another.

940:     /// @param collectedAmounts The amount of tokens (token0 and token1) collected from Uniswap

942:         bytes32 positionKey,

1056:         // burn that option's liquidity in the Uniswap Pool.

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

### <a name="L-10"></a>[L-10] NFT ownership doesn't support hard forks

To ensure clarity regarding the ownership of the NFT on a specific chain, it is recommended to add `require(block.chainid == 1, "Invalid Chain")` or the desired chain ID in the functions below.

Alternatively, consider including the chain ID in the URI itself. By doing so, any confusion regarding the chain responsible for owning the NFT will be eliminated.

*Instances (1)*:

```solidity
File: contracts/base/FactoryNFT.sol

40:     function tokenURI(uint256 tokenId) public view override returns (string memory) {
            address panopticPool = address(uint160(tokenId));
    
            return

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

### <a name="L-11"></a>[L-11] Possible rounding issue

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator. Also, there is indication of multiplication and division without the use of parenthesis which could result in issues.

*Instances (6)*:

```solidity
File: contracts/CollateralTracker.sol

755:             return (s_inAMM * DECIMALS) / totalAssets();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

1691:                                     totalLiquidityBefore) / totalLiquidity

1699:                                     totalLiquidityBefore) / totalLiquidity

1914:                                     ) / totalLiquidity

1930:                                     ) / totalLiquidity

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

767:                 uint256 bonusCross = Math.min(balanceCross / 2, thresholdCross - balanceCross);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="L-12"></a>[L-12] Loss of precision

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (12)*:

```solidity
File: contracts/CollateralTracker.sol

248:             s_ITMSpreadFee = uint128((ITM_SPREAD_MULTIPLIER * fee) / DECIMALS);

429:             return (convertToShares(type(uint104).max) * (DECIMALS - COMMISSION_FEE)) / DECIMALS;

744:                 .toRightSlot(int128((longAmounts.rightSlot() * fee) / DECIMALS_128))

745:                 .toLeftSlot(int128((longAmounts.leftSlot() * fee) / DECIMALS_128));

755:             return (s_inAMM * DECIMALS) / totalAssets();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticPool.sol

1691:                                     totalLiquidityBefore) / totalLiquidity

1699:                                     totalLiquidityBefore) / totalLiquidity

1914:                                     ) / totalLiquidity

1930:                                     ) / totalLiquidity

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

1193:                     uint256 numerator = netLiquidity + (removedLiquidity / 2 ** VEGOID);

1216:                         ((removedLiquidity ** 2) / 2 ** (VEGOID));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

767:                 uint256 bonusCross = Math.min(balanceCross / 2, thresholdCross - balanceCross);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="L-13"></a>[L-13] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`

The compiler for Solidity 0.8.20 switches the default target EVM version to [Shanghai](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which includes the new `PUSH0` op code. This op code may not yet be implemented on all L2s, so deployment on these chains will fail. To work around this issue, use an earlier [EVM](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) [version](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version). While the project itself may or may not compile with 0.8.20, other projects with which it integrates, or which extend this project may, and those projects will have problems deploying these contracts/libraries.

*Instances (19)*:

```solidity
File: contracts/CollateralTracker.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/base/FactoryNFT.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/base/MetadataStore.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/MetadataStore.sol)

```solidity
File: contracts/libraries/CallbackLib.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/CallbackLib.sol)

```solidity
File: contracts/libraries/Constants.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Constants.sol)

```solidity
File: contracts/libraries/Errors.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Errors.sol)

```solidity
File: contracts/libraries/FeesCalc.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/FeesCalc.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/Math.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/libraries/SafeTransferLib.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/SafeTransferLib.sol)

```solidity
File: contracts/types/LeftRight.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/LiquidityChunk.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LiquidityChunk.sol)

```solidity
File: contracts/types/Pointer.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

2: pragma solidity ^0.8.24;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="L-14"></a>[L-14] `symbol()` is not a part of the ERC-20 standard

The `symbol()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (1)*:

```solidity
File: contracts/libraries/PanopticMath.sol

88:         try IERC20Metadata(token).symbol() returns (string memory symbol) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

### <a name="L-15"></a>[L-15] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting

Downcasting from `uint256`/`int256` in Solidity does not revert on overflow. This can result in undesired exploitation or bugs, since developers usually assume that overflows raise errors. [OpenZeppelin's SafeCast library](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) restores this intuition by reverting the transaction when such an operation overflows. Using this library eliminates an entire class of bugs, so it's recommended to use it always. Some exceptions are acceptable like with the classic `uint256(uint160(address(variable)))`

*Instances (98)*:

```solidity
File: contracts/CollateralTracker.sol

248:             s_ITMSpreadFee = uint128((ITM_SPREAD_MULTIPLIER * fee) / DECIMALS);

420:         s_poolAssets += uint128(assets);

476:         s_poolAssets += uint128(assets);

531:             s_poolAssets -= uint128(assets);

572:         s_poolAssets -= uint128(assets);

632:             s_poolAssets -= uint128(assets);

685:                     int24 range = int24(

731:                         .toRightSlot(int128(uint128(currentValue0)) - int128(uint128(oracleValue0)))

732:                         .toLeftSlot(int128(uint128(currentValue1)) - int128(uint128(oracleValue1)))

907:             s_poolAssets += uint128(bonusAbs);

1035:             s_poolAssets = uint256(updatedAssets).toUint128();

1036:             s_inAMM = uint256(int256(uint256(s_inAMM)) + (shortAmount - longAmount)).toUint128();

1036:             s_inAMM = uint256(int256(uint256(s_inAMM)) + (shortAmount - longAmount)).toUint128();

1038:             utilization = uint32(_poolUtilization());

1083:             s_poolAssets = uint256(updatedAssets + realizedPremium).toUint128();

1084:             s_inAMM = uint256(int256(uint256(s_inAMM)) - (shortAmount - longAmount)).toUint128();

1084:             s_inAMM = uint256(int256(uint256(s_inAMM)) - (shortAmount - longAmount)).toUint128();

1185:                 ? int16(PositionBalance.wrap(positionBalanceArray[i][1]).utilization0())

1186:                 : int16(PositionBalance.wrap(positionBalanceArray[i][1]).utilization1());

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

331:                 fullRangeLiquidity = uint128(

335:                 fullRangeLiquidity = uint128(

344:                 uint128 liquidity0 = uint128(

347:                 uint128 liquidity1 = uint128(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

1258:                 uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))

1258:                 uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1259:             ) + int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) / 2;

1483:                             int128(

1492:                             int128(

1646:                     .wrap(uint128(grossCurrent0))

1647:                     .toLeftSlot(uint128(grossCurrent1));

1655:                 isLong == 0 ? MAX_SPREAD : uint64(Math.min(effectiveLiquidityLimitX32, MAX_SPREAD))

1687:                             uint128(

1695:                             uint128(

1735:                         uint128(

1744:                         uint128(

1902:                                 uint128(

1918:                                 uint128(

1934:                             .wrap(uint128(premiumAccumulatorsByLeg[_leg][0]))

1935:                             .toLeftSlot(uint128(premiumAccumulatorsByLeg[_leg][1]));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/base/FactoryNFT.sol

41:         address panopticPool = address(uint160(tokenId));

94:                                     PanopticMath.uniswapFeeToString(uint24(fee)),

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/base/FactoryNFT.sol)

```solidity
File: contracts/libraries/Math.sol

184:             return uint160((sqrtR >> 32) + (sqrtR % (1 << 32) == 0 ? 0 : 1));

302:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) revert Errors.CastingError();

308:         if ((downcastedInt = uint128(toDowncast)) != toDowncast) {

324:         if (!((downcastedInt = int128(toCast)) == toCast)) revert Errors.CastingError();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Math.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

134:         uint256 updatedHash = uint248(existingHash) ^

139:             ? uint8(existingHash >> 248) + 1

140:             : uint8(existingHash >> 248) - 1;

250:             return (int24(Math.sort(ticks)[cardinality / 2]), int24(ticks[0]));

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

273:                 (int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 3)) % 8) * 24))) +

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

274:                     int24(uint24(medianData >> ((uint24(medianData >> (192 + 3 * 4)) % 8) * 24)))) /

278:             if (block.timestamp >= uint256(uint40(medianData >> 216)) + period) {

278:             if (block.timestamp >= uint256(uint40(medianData >> 216)) + period) {

295:                 uint24 orderMap = uint24(medianData >> 192);

312:                     entry = int24(uint24(medianData >> (rank * 24)));

312:                     entry = int24(uint24(medianData >> (rank * 24)));

324:                     uint256(uint192(medianData << 24)) +

324:                     uint256(uint192(medianData << 24)) +

344:                 secondsAgos[i] = uint32(((i + 1) * twapWindow) / 20);

361:             return int24(sortedTicks[9]);

689:             amount1 = Math.mulDiv96RoundingUp(amount0, geometricMeanPriceX96).toUint128();

693:             amount0 = Math.mulDivRoundingUp(amount1, 2 ** 96, geometricMeanPriceX96).toUint128();

970:                 if (haircut1 != 0) collateral1.exercise(_liquidatee, 0, 0, 0, int128(haircut1));

1044:                 int256(ct0.convertToShares(uint128(-exerciseFees.rightSlot())));

1077:                 int256(ct1.convertToShares(uint128(-exerciseFees.leftSlot())));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/types/LeftRight.sol

39:         return uint128(LeftRightUnsigned.unwrap(self));

46:         return int128(LeftRightSigned.unwrap(self));

68:                         uint256(uint128(LeftRightUnsigned.unwrap(self)) + right)

68:                         uint256(uint128(LeftRightUnsigned.unwrap(self)) + right)

101:         return uint128(LeftRightUnsigned.unwrap(self) >> 128);

108:         return int128(LeftRightSigned.unwrap(self) >> 128);

161:                 (uint128(LeftRightUnsigned.unwrap(z)) < uint128(LeftRightUnsigned.unwrap(x)))

184:                 (uint128(LeftRightUnsigned.unwrap(z)) > uint128(LeftRightUnsigned.unwrap(x)))

196:             int128 left128 = int128(left);

201:             int128 right128 = int128(right);

216:             int128 left128 = int128(left256);

219:             int128 right128 = int128(right256);

234:             int128 left128 = int128(left256);

237:             int128 right128 = int128(right256);

256:             int128 left128 = int128(left256);

259:             int128 right128 = int128(right256);

265:                     int128(Math.max(left128, 0))

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/LeftRight.sol)

```solidity
File: contracts/types/Pointer.sol

30:         return address(uint160(Pointer.unwrap(self)));

30:         return address(uint160(Pointer.unwrap(self)));

37:         return uint48(Pointer.unwrap(self) >> 160);

44:         return uint48(Pointer.unwrap(self) >> 208);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/Pointer.sol)

```solidity
File: contracts/types/PositionBalance.sol

123:             return uint96(PositionBalance.unwrap(self) >> 160);

166:             return uint32(PositionBalance.unwrap(self) >> 128);

175:             return uint128(PositionBalance.unwrap(self));

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

```solidity
File: contracts/types/TokenId.sol

89:             return uint64(TokenId.unwrap(self));

98:             return int24(uint24((TokenId.unwrap(self) >> 48) % 2 ** 16));

521:                     if (uint48(chunkData >> (48 * i)) == uint48(chunkData >> (48 * j))) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/TokenId.sol)

### <a name="L-16"></a>[L-16] Unsafe ERC20 operation(s)

*Instances (6)*:

```solidity
File: contracts/CollateralTracker.sol

318:         return ERC20Minimal.transfer(recipient, amount);

337:         return ERC20Minimal.transferFrom(from, to, amount);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

32:         IERC20Partial(token0).approve(address(sfpm), type(uint256).max);

33:         IERC20Partial(token1).approve(address(sfpm), type(uint256).max);

36:         IERC20Partial(token0).approve(address(ct0), type(uint256).max);

37:         IERC20Partial(token1).approve(address(ct1), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

### <a name="L-17"></a>[L-17] Upgradeable contract not initialized

Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (15)*:

```solidity
File: contracts/CollateralTracker.sol

93:     bool internal s_initialized;

219:         if (s_initialized) revert Errors.CollateralTokenAlreadyInitialized();

220:         s_initialized = true;

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/PanopticFactory.sol

183:         if (address(v3Pool) == address(0)) revert Errors.UniswapPoolNotInitialized();

186:             revert Errors.PoolAlreadyInitialized();

189:         SFPM.initializeAMMPool(token0, token1, fee);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticFactory.sol)

```solidity
File: contracts/PanopticPool.sol

272:         if (address(s_univ3pool) != address(0)) revert Errors.PoolAlreadyInitialized();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/PanopticPool.sol)

```solidity
File: contracts/SemiFungiblePositionManager.sol

81:     event PoolInitialized(address indexed uniswapPool, uint64 poolId);

305:     function initializeAMMPool(address token0, address token1, uint24 fee) external {

310:         if (univ3pool == address(0)) revert Errors.UniswapPoolNotInitialized();

338:         emit PoolInitialized(univ3pool, poolId);

727:         if (univ3pool == IUniswapV3Pool(address(0))) revert Errors.UniswapPoolNotInitialized();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

```solidity
File: contracts/libraries/Errors.sol

16:     error CollateralTokenAlreadyInitialized();

63:     error PoolAlreadyInitialized();

95:     error UniswapPoolNotInitialized();

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/Errors.sol)

## Medium Issues

| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | `_safeMint()` should be used rather than `_mint()` wherever possible | 1 |
| [M-2](#M-2) | Library function isn't `internal` or `private` | 14 |
| [M-3](#M-3) | Return values of `transfer()`/`transferFrom()` not checked | 2 |
| [M-4](#M-4) | Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20` | 6 |

### <a name="M-1"></a>[M-1] `_safeMint()` should be used rather than `_mint()` wherever possible

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren't lost if they're minted to contracts that cannot transfer them back out.

Be careful however to respect the CEI pattern or add a re-entrancy guard as `_safeMint` adds a callback-check (`_checkOnERC721Received`) and a malicious `onERC721Received` could be exploited if not careful.

Reading material:

- <https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a>
- <https://samczsun.com/the-dangers-of-surprising-code/>
- <https://github.com/KadenZipfel/smart-contract-attack-vectors/blob/master/vulnerabilities/unprotected-callback.md>

*Instances (1)*:

```solidity
File: contracts/SemiFungiblePositionManager.sol

456:         _mint(msg.sender, TokenId.unwrap(tokenId), positionSize);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/SemiFungiblePositionManager.sol)

### <a name="M-2"></a>[M-2] Library function isn't `internal` or `private`

In a library, using an external or public visibility means that we won't be going through the library with a DELEGATECALL but with a CALL. This changes the context and should be done carefully.

*Instances (14)*:

```solidity
File: contracts/libraries/FeesCalc.sol

52:         // extract the amount of AMM fees collected within the liquidity chunk

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/FeesCalc.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

24:     function doApprovals(

48:     function computeName(

78:     function computeSymbol(

88:     function computeDecimals(address token) external view returns (uint8) {

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

```solidity
File: contracts/libraries/PanopticMath.sol

76:     function numberOfLeadingHexZeros(address addr) external pure returns (uint256) {

85:     function safeERC20Symbol(address token) external view returns (string memory) {

159:     function getOracleTicks(

263:     function computeInternalMedian(

336:     function twapFilter(IUniswapV3Pool univ3pool, uint32 twapWindow) external view returns (int24) {

749:         LeftRightUnsigned tokenData1,

883:         LeftRightSigned[4][] memory premiasByLeg,

1034:         int24 atTick,

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/PanopticMath.sol)

```solidity
File: contracts/types/PositionBalance.sol

188:     function unpackAll(

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/types/PositionBalance.sol)

### <a name="M-3"></a>[M-3] Return values of `transfer()`/`transferFrom()` not checked

Not all `IERC20` implementations `revert()` when there's a failure in `transfer()`/`transferFrom()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

*Instances (2)*:

```solidity
File: contracts/CollateralTracker.sol

318:         return ERC20Minimal.transfer(recipient, amount);

337:         return ERC20Minimal.transferFrom(from, to, amount);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

### <a name="M-4"></a>[M-4] Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20`

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens.  For example Tether (USDT)'s `transfer()` and `transferFrom()` functions on L1 do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to `IERC20`, their [function signatures](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca) do not match and therefore the calls made, revert (see [this](https://gist.github.com/IllIllI000/2b00a32e8f0559e8f386ea4f1800abc5) link for a test case). Use OpenZeppelin's `SafeERC20`'s `safeTransfer()`/`safeTransferFrom()` instead

*Instances (6)*:

```solidity
File: contracts/CollateralTracker.sol

318:         return ERC20Minimal.transfer(recipient, amount);

337:         return ERC20Minimal.transferFrom(from, to, amount);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/CollateralTracker.sol)

```solidity
File: contracts/libraries/InteractionHelper.sol

32:         IERC20Partial(token0).approve(address(sfpm), type(uint256).max);

33:         IERC20Partial(token1).approve(address(sfpm), type(uint256).max);

36:         IERC20Partial(token0).approve(address(ct0), type(uint256).max);

37:         IERC20Partial(token1).approve(address(ct1), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)

## High Issues

| |Issue|Instances|
|-|:-|:-:|
| [H-1](#H-1) | IERC20.approve() will revert for USDT | 4 |

### <a name="H-1"></a>[H-1] IERC20.approve() will revert for USDT

Use forceApprove() from SafeERC20

*Instances (4)*:

```solidity
File: contracts/libraries/InteractionHelper.sol

32:         IERC20Partial(token0).approve(address(sfpm), type(uint256).max);

33:         IERC20Partial(token1).approve(address(sfpm), type(uint256).max);

36:         IERC20Partial(token0).approve(address(ct0), type(uint256).max);

37:         IERC20Partial(token1).approve(address(ct1), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2024-09-panoptic/blob/main/contracts/libraries/InteractionHelper.sol)
