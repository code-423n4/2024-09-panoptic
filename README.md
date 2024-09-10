# Panoptic audit details
- Total Prize Pool: $XX,XXX in USDC
  - HM awards: $XX,XXX in USDC
  - Judge awards: $X,XXX in USDC
  - Validator awards: $X,XXX in USDC 
  - Scout awards: $XXX in USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-06-panoptic/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts Sept XX, 2024 20:00 UTC
- Ends Sept XX, 2024 20:00 UTC

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found @TODO: ~~[here](https://github.com/code-423n4/2024-09-panoptic/blob/main/4naly3er-report.md).~~



_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._



- Transfers of ERC1155 SFPM tokens are very limited by design. It is expected that certain accounts will be unable to transfer or receive tokens. 
Some tokens may not be transferable at all.
- Construction helper functions (prefixed with add) in the TokenId library and other types do not perform extensive input validation. Passing invalid or nonsensical inputs into these functions or attempting to overwrite already filled slots may yield unexpected or invalid results. This is by design, so it is expected that users
of these functions will validate the inputs beforehand. 
- Tokens with a supply exceeding 2^127 - 1 are not supported.
- If one token on a pool is broken/does not meet listed criteria/is malicious there are no guarantees as to the security of the other token in that pool, as long as other pools with two legitimate and compliant tokens are not affected.
- Price/oracle manipulation that is not atomic or requires attackers to hold a price across more than one block (i.e., to manipulate a Uniswap observation, you need to set the manipulated price at the end of one block, and then keep it there until the next block) is not in scope
- Attacks that stem from the TWAP being extremely stale compared to the market price within its period (currently 10 minutes) 
- As a general rule, only price manipulation issues that can be triggered by manipulating the price atomically from a normal pool/oracle state are valid
- The constants VEGOID, TWAP_WINDOW, SLOW_ORACLE_UNISWAP_MODE, MEDIAN_PERIOD, FAST_ORACLE_CARDINALITY, SLOW_ORACLE_CARDINALITY, SLOW_ORACLE_PERIOD, FAST_ORACLE_PERIOD, MAX_TWAP_DELTA_LIQUIDATION, MAX_SLOW_FAST_DELTA, MAX_SPREAD, MAX_POSITIONS, BP_DECREASE_BUFFER, and NO_BUFFER are all parameters and subject to change, but within reasonable levels. Cardinalities should be odd, and we're aware that the oracle/pool deployment will fail if the period is too large for the pool. It's important that the SLOW_ORACLE_UNISWAP_MODE flag works as intended, but otherwise not looking for anything other than a super glaring issue with the implementation of those parameters. Playing with the parameters to find an extreme example that technically won't work isn't going to be very useful to us.
 - Given a small enough pool and low seller diversity, premium manipulation by swapping back and forth in Uniswap is a known risk. As long as it's not possible to do it between two of your own accounts *profitably* and doesn't cause protocol loss, that's acceptable
 - Front-running via insufficient slippage specification is not in scope
 - It's known that liquidators sometimes have a limited capacity to force liquidations to execute at a less favorable price and extract some additional profit from that. This is acceptable even if it causes some amount of unnecessary protocol loss.
 - It's possible to leverage the rounding direction to artificially inflate the total gross premium and significantly decrease the rate of premium option sellers earn/are able to withdraw (but not the premium buyers pay) in the future (only significant for very-low-decimal pools, since this must be done one token at a time).
 - It's also possible for options buyers to avoid paying premium by calling `settleLongPremium` if the amount of premium owed is sufficiently small.
- Premium accumulation can become permanently capped if the accumulator exceeds the maximum value; this can happen if a low amount of liquidity earns a large amount of (token) fees
- The liquidator may not be able to execute a liquidation if MAX_POSITIONS is too high for the deployed chain due to an insufficient gas limit. This parameter is not final and will be adjusted by deployed chain such that the most expensive liquidation is well within a safe margin of the gas limit.
- It's expected that liquidators may have to sell options, perform force exercises, and deposit collateral to perform some liquidations. In some situations, the liquidation may not be profitable.
- In some situations (stale TWAP tick), force exercised users will be worse off than if they had burnt their position.
 - For the purposes of this competition, assume the constructor arguments to the CollateralTracker are: `10, 2_000, 1_000, -128, 5_000, 9_000, 20_000`
- Depending on the token, the amount of funds required for the initial factory deployment may be high or unrealistic
- It is feasible for the share supply of the CollateralTracker to approach 2**256 - 1 (given the token supply constraints, this can happen through repeated protocol-loss-causing liquidations), which can cause various reverts and overflows. Generally, issues with an extremely high share supply as a precondition (delegation reverts due to user's balance being too high, other DoS caused by overflows in calculations with share supply or balances, etc.) are not valid unless that share supply can be created through means other than repeated liquidations/high protocol loss.  



# Overview

Panoptic is a permissionless options trading protocol. It enables the trading of perpetual options on top of any [Uniswap V3](https://uniswap.org/) pool

The Panoptic protocol is noncustodial, has no counterparty risk, offers instantaneous settlement, and is designed to remain fully collateralized at all times.

## Core Contracts

### SemiFungiblePositionManager

A gas-efficient alternative to Uniswap’s NonFungiblePositionManager that manages complex, multi-leg Uniswap positions encoded in ERC1155 tokenIds, performs swaps allowing users to mint positions with only one type of token, and, most crucially, supports the minting of both typical LP positions where liquidity is added to Uniswap and “long” positions where Uniswap liquidity is burnt. While the SFPM is enshrined as a core component of the protocol and we consider it to be the “engine” of Panoptic, it is also a public good that we hope savvy Uniswap V3 LPs will grow to find an essential tool and upgrade for managing their liquidity.

### CollateralTracker

An ERC4626 vault where token liquidity from passive Panoptic Liquidity Providers (PLPs) and collateral for option positions are deposited. CollateralTrackers are also responsible for paying out commission fees and options premia, handling payments of intrinsic value for options and distributing P&L, calculating liquidation bonuses, and determining costs for forcefully exercising another user’s options. However, by far the most important functionality of the CollateralTracker is to calculate the collateral requirement for every account and position. Each time positions are minted or burned in Panoptic, the CollateralTracker updates the collateral balances and provides information on the collateral requirement, ensuring that the protocol remains solvent and we retain the ability to liquidate distressed positions when needed.

### PanopticPool

The Panoptic Pool exposes the core functionality of the protocol. If the SFPM is the “engine” of Panoptic, the Panoptic Pool is the “conductor”. All interactions with the protocol, be it minting or burning positions, liquidating or force exercising distressed accounts, or just checking position balances and accumulating premiums, originate in this contract. It is responsible for orchestrating the required calls to the SFPM to actually create option positions in Uniswap, tracking user balances of and accumulating the premia on those positions, and calling the CollateralTracker with the data it needs to settle position changes.

## Architecture & Actors

Each instance of the Panoptic protocol on a Uniswap pool contains:

- One PanopticPool that orchestrates all interactions in the protocol
- Two CollateralTrackers, one for each constituent token0/token1 in the Uniswap pool
- A canonical SFPM - the SFPM manages liquidity across every Panoptic Pool

There are five primary roles assumed by actors in this Panoptic Ecosystem:

### Panoptic Liquidity Providers (PLPs)

Users who deposit tokens into one or both CollateralTracker vaults. The liquidity deposited by these users is borrowed by option sellers to create their positions - their liquidity is what enables undercollateralized positions. In return, they receive commission fees on both the notional and intrinsic values of option positions when they are minted. Note that options buyers and sellers are PLPs too - they must deposit collateral to open their positions. We consider users who deposit collateral but do not _trade_ on Panoptic to be “passive” PLPs

### Option Sellers

These users deposit liquidity into the Uniswap pool through Panoptic, making it available for options buyers to remove. This role is similar to providing liquidity directly to Uniswap V3, but offers numerous benefits including advanced tools to manage risky, complex positions and a multiplier on the fees/premia generated by their liquidity when it is removed by option buyers. Sold option positions on Panoptic have similar payoffs to traditional options.

### Option Buyers

These users remove liquidity added by option sellers from the Uniswap Pool and move the tokens back into Panoptic. The premia they pay to sellers for the privilege is equivalent to the fees that would have been generated by the removed liquidity, plus a spread multiplier based on the portion of available liquidity in their Uniswap liquidity chunk that has been removed or utilized.

### Liquidators

These users are responsible for liquidating distressed accounts that no longer meet the collateral requirements needed to maintain their positions. They provide the tokens necessary to close all positions in the distressed account and receive a bonus from the remaining collateral. Sometimes, they may also need to buy or sell options to allow lower liquidity positions to be exercised

### Force Exercisors

These are usually options sellers. They provide the required tokens and forcefully exercise long positions (from option buyers) in out-of-range strikes that are no longer generating premia, so the liquidity from those positions is added back to Uniswap and the sellers can exercise their positions (which involves burning that liquidity). They pay a fee to the exercised user for the inconvenience.

## Flow

All protocol users first onboard by depositing tokens into one or both CollateralTracker vaults and being issued shares (becoming PLPs in the process). Panoptic’s CollateralTracker supports the full ERC4626 interface, making deposits and withdrawals a simple and standardized process. Passive PLPs stop here.

Once they have deposited, there are many options for the other actors in the protocol. Buyers and sellers can call :

- `mintOptions` - create an option position with up to four distinct legs with a specially encoded - positionID/tokenID, each of which is its own short (sold/added) or long (bought/removed) liquidity chunk
- `burnOptions` - burn or exercise a position created through `mintOptions`
- `settleLongPremium` - Force a solvent option buyer to pay any premium owed to sellers. Options sellers will want to call this if a large portion of the total premium owed in their chunk is pending/has not been settled.
- `pokeMedian` - Takes a new internal median observation and inserts it into the internal median ring buffer if enough time has passed since the last observation. Otherwise, the buffer is poked during `mintOptions`/`burnOptions`

Meanwhile, force exercisers and liquidators can perform their respective roles with the `forceExercise` and `liquidateAccount` functions.

## Links

- **Previous audits:**  None public
- [Panoptic's Website](https://www.panoptic.xyz)
- [Whitepaper](https://paper.panoptic.xyz/)
- [Litepaper](https://intro.panoptic.xyz)
- [Documentation](https://docs.panoptic.xyz/)
- [Twitter](https://twitter.com/Panoptic_xyz)
- [Discord](https://discord.gg/7fE8SN9pRT)
- [Blog](https://www.panoptic.xyz/blog)
- [YouTube](https://www.youtube.com/@Panopticxyz)

### Further Reading

Panoptic has been presented at conferences and was conceived with the first Panoptic's Genesis blog post in mid-summer 2021:

- [Panoptic @ ETH Denver 2023](https://www.youtube.com/watch?v=Dt5AdCNavjs)
- [Panoptic @ ETH Denver 2022](https://www.youtube.com/watch?v=mtd4JphPcuA)
- [Panoptic @ DeFi Guild](https://www.youtube.com/watch?v=vlPIFYfG0FU)
- [Panoptic's Genesis: Blog Series](https://lambert-guillaume.medium.com/)
---

# Scope


### Files in scope

@TODO: everything in `contracts` folder except for factorynft/metadatastory

### Files out of scope

@TODO

## Scoping Q &amp; A

- The post-diff version of the contracts (defined as the latest commit on main in 2024-09-panoptic) should not contain or introduce any High or Medium severity issues.

- The diff should start after the commit <https://github.com/code-423n4/2024-09-panoptic/commit/cdadf6972277cb1d26115b1dcac95850493bfa97> on the 2024-09-panoptic repo.

- Here is the diff: [https://github.com/code-423n4/2024-09-panoptic/compare/cdadf6972277cb1d26115b1dcac95850493bfa97...a1c0890dccf13e5ecd63a5d2555d187e82aa7ecd](https://github.com/code-423n4/2024-09-panoptic/compare/cdadf6972277cb1d26115b1dcac95850493bfa97...a1c0890dccf13e5ecd63a5d2555d187e82aa7ecd) 

### General questions


| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| Test coverage                           | -                          |
| ERC20 used by the protocol              |       ERC4626 collateral vaults              |
| ERC721 used  by the protocol            |            PanopticFactory NFT        |
| ERC777 used by the protocol             |           no                |
| ERC1155 used by the protocol            |              SFPM tokenIds            |
| Chains the protocol will be deployed on | Ethereum |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      | ✅ Yes  |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  | ❌ No |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) | ❌ No   |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 | ❌ No |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              | ✅ Yes   |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      | ❌ No   |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              | ✅ Yes   |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            | ✅ Yes   |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    | ✅ Yes   |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    | ✅ Yes   |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    | ✅ Yes   |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  | ❌ No   |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   | ❌ No  |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#multiple-token-addresses)                                          | ❌ No   |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 | ✅ Yes |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              | ✅ Yes   |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                | ❌ No  |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance checklist
N/A


# Additional context

## Main invariants

SFPM:
- Users of the SFPM can only remove liquidity (via isLong==1 or burning positions) that they have previously added under the same (tickLower, tickUpper, tokenType) key. They cannot remove liquidity owned by other users.
- Fees collected from Uniswap during any given operation should not exceed the amount of fees earned by the liquidity owned by the user performing the operation.
- Fees paid to a given user should not exceed the amount of fees earned by the liquidity owned by that user.
Panoptic:
- Commission paid by an account should be greater than or equal to 1 (of either) token
- Option sellers must pay back the exact amounts of tokens that were borrowed (the "shortAmounts") when their positions are burned
- Option buyers must add back the same amount of liquidity that was borrowed when their positions are burned
- Users should not be allowed to mint/burn options or pay premium if their end state is insolvent (at the fast oracle tick, or both ticks if the fast and slow oracle ticks are further away than the threshold)
- Users should not be allowed to mint more positions than the limit
- Option sellers should not receive premium that has not settled (by collection from Uniswap or payments from long option holders) yet
- Option sellers in a given chunk should not receive premium, as a fraction of the premium owed to them , that is greater than the ratio of total owed premium to total settled premium in that chunk
- Note that some executable liquidations may not be profitable due to the required actions to execute the liquidation (depositing collateral, force exercises) or the price the token conversions occur at
- Note that it is acceptable for an account to cause protocol loss (shares minted to liquidator) during a liquidation even if they have a residual token balance in their account, this may happen if the value of tokens remaining correspond to less than 1 of the other token
- If, at the final state after a liquidation, any premium paid to sellers DURING the liquidation has not been revoked, there must not be any protocol loss (shares minted to liquidator). In other words, this means that premium cannot be paid over the protocol loss threshold.
- The value of the bonus paid to the liquidator must not exceed the liquidated user's collateral balance at the beginning of the call


## Attack ideas (where to focus for bugs)
The factory contract and usage of libraries by external integrators is relatively unimportant -- wardens should focus their efforts on the security of the SFPM (previously audited by C4, code has changed since that audit however), PanopticPool, and CollateralTracker


## All trusted roles in the protocol
N/A


## Running tests

⚠️**Note**: You will need to provide your own Ethereum Mainnet eth_rpc_url (Works best with a local archives node) in the Foundry.toml.

```bash
git clone --recurse-submodules https://github.com/code-423n4/2024-09-panoptic.git
cd 2024-09-panoptic
export FOUNDRY_PROFILE=ci_test  # (tests WILL fail without this because of a Foundry bug)
forge build
forge test
```

To run gas benchmarks:
```bash
forge test --gas-report
```

## Miscellaneous
Employees of Panoptic and employees' family members are ineligible to participate in this audit.
