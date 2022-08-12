---
id: delegation
title: Delegation (Validator shares)
description: For the Polygon's Proof of Security based consensus, all the 2/3+1 proof verification and handling of staking, rewards are executed on the Ethereum smart contract. The whole design follows this philosophy of doing less on the Mainnet contract.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## Overview

Polygon supports delegation via validator shares. By using this design, it is easier to distribute rewards and slash with scale (thousands of delegators) on Ethereum contracts without much computation.

Delegators delegate by purchasing shares of a finite pool from validators. Each validator will have their own validator share token. Let's call these fungible tokens `VATIC` for a validator `A`. As soon as a user delegates to a validator `A`, they will be issued `VATIC` based on an exchange rate of `MATIC/VATIC` pair. As users accrue value the exchange rate indicates that they can now withdraw more `MATIC` for each `VATIC` and when users get slashed, users withdraw less `MATIC` for their `VATIC`.

Note that `MATIC` is a staking token. A delegator needs to have `MATIC` tokens to participate in the delegation.

Initially, a delegator `D` buys tokens from validator `A` specific pool when `1 MATIC per 1 VATIC`.

When a validator gets rewarded with more `MATIC` tokens, new tokens are added to the pool. Let's say with the current pool of `100 MATIC` tokens,  `10 MATIC` rewards are added to the pool. But since the total supply of `VATIC` tokens didn't change due to rewards, the exchange rate becomes `1 MATIC per 0.9 VATIC`. Now, delegator `D` gets more `MATIC` for the same shares. Similar to slashing, if `10 MATIC` gets slashed from the pool, the new exchange rate will be `1 Matic per 1.1 VATIC`.

`VATIC`: Validator specific minted validator share tokens (ERC20 tokens)

## Technical specification

```java
uint256 public validatorId; // Delegation contract for validator
uint256 public validatorRewards; // accumulated rewards for validator
uint256 public commissionRate; // validator's cut %
uint256 public validatorDelegatorRatio = 10; // to be implemented/used

uint256 public totalStake;
uint256 public rewards; // rewards for pool of delegation stake
uint256 public activeAmount; // # of tokens delegated which are part of active stake
```

Exchange rate is calculated as below:

```js
ExchangeRate = (totalDelegatedPower + delegatorRewardPool) / totalDelegatorShares
```

### buyVoucher

```js
function buyVoucher(uint256 _amount) public;
```

- Transfer the `_amount` to stakeManager and update the timeline data structure for active stake.
- `updateValidatorState` is used to update timeline DS.
- `Mint` delegation shares using current `exchangeRate` for `_amount`.
- `amountStaked` is used to keep track of active stake of each delegator in order to calculate liquid rewards.

### sellVoucher

```js
function sellVoucher() public;
```

- Using current  `exchangeRate` and # of shares calculate total amount(active stake+ rewards).
- unBond active stake from validator and transfer rewards to delegator if any.
- Must remove active stake from timeline using `updateValidatorState` in stakeManger.
- Move active stake of delegator into withdrawal period for slashing reasons.
- `delegators` mapping is used to keep track of stake in withdrawal period.

### withdrawRewards

```js
function withdrawRewards() public;
```

- For a delegator calculate the rewards and transfer and depending upon `exchangeRate` burn # of shares.
- i.e. delegator owns 100 share and exchange rate is 200 so rewards are 100 tokens, transfer 100 tokens to delegator, remaining stake is 100 so using exchange rate 200 now it is worth 50 shares so burn 50 shares. Delegator now has 50 shares worth 100 tokens(which he initially staked/delegated).

### reStake

- Restake can work in two ways delegator can buy more shares using `buyVoucher` or reStake rewards.

```js
function reStake() public;
```

- Above function is used to reStake rewards
- The number of shares aren’t affected because `exchangeRate` is the same; so just the rewards are moved into active stake for both validator share contract and stakeManager timeline.
- `getLiquidRewards` is used for calculating accumulated rewards.
- i.e. delegator owns 100 share and exchange rate is 200 so rewards are 100 tokens, move 100 tokens into active stake, since exchange rate is still same number of share will also remain same. Only difference is that now 200 tokens are considered into active stake and can't be withdrawn immediately(not a part of liquid rewards).
- Purpose of reStaking is that since delegator's validator has now more active stake and she will earn more rewards for that so will the delegator.

### unStakeClaimTokens

```js
function unStakeClaimTokens()
```

- Once withdrawal period is over delegators who've sold their shares can claim their matic tokens.
- Must transfer tokens to user.
- Once slashing is in place must check for all the slashing happened in that withdrawal period and take into account.

### updateCommissionRate

```js
function updateCommissionRate(uint256 newCommissionRate)
        external
        onlyValidator
```

- Updates commission % for the validator.

### updateRewards

```js
function updateRewards(uint256 reward, uint256 checkpointStakePower, uint256 validatorStake)
        external
        onlyOwner
        returns (uint256)
```

- When a validator gets rewards for submitting checkpoint this function is called for disbursements of rewards between validator and delegators.

For more details here is a video explaining the whole mechanism in details:<!-- \[https://www.youtube.com/watch?v=8nODLU9C3mw\](https://www.youtube.com/watch?v=8nODLU9C3mw) -->[![create liquid staking assets - video](https://img.youtube.com/vi/8nODLU9C3mw/0.jpg)](https://www.youtube.com/watch?v=8nODLU9C3mw)
