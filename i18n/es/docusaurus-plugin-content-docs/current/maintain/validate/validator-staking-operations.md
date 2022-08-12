---
id: validator-staking-operations
title: Stake on Polygon
description: "Learn how to stake as a validator on the Polygon Network."
keywords:
  - docs
  - matic
  - polygon
  - stake
  - claim
  - unstake
slug: validator-staking-operations
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

## Prerequisites

### Full node set up

Your validator node fully set up and synced.

See:

* [Run a Validator Node with Ansible](run-validator-ansible)
* [Run a Validator Node from Binaries](run-validator-binaries)

### Account setup

On your validator node, check that the account is set up.

To check, run the following command **on the validator node**:

```sh
    heimdalld show-account
```

Your output should appear in the following format:

```json
{
    "address": "0x6c468CF8c9879006E22EC4029696E005C2319C9D",
    "pub_key": "0x04b12d8b2f6e3d45a7ace12c4b2158f79b95e4c28ebe5ad54c439be9431d7fc9dc1164210bf6a5c3b8523528b931e772c86a307e8cff4b725e6b4a77d21417bf19"
}
```

This will display your address and public key for your validator node. **Note that this address must match with your signer address on Ethereum.**

### Show private key

This step is optional.

On your validator node, check that the private key is correct.

To check, run the following command **on the validator node**:

```sh
heimdalld show-privatekey
```

The following output should appear:

```json
{
    "priv_key": "0x********************************************************"
}
```

## Stake on Polygon

You can stake on Polygon using the [validator dashboard](https://wallet.polygon.technology/staking/validators/).

### Stake using the staking dashboard

1. Access the [validator dashboard](https://wallet.polygon.technology/staking/validators/).
1. Log in with your wallet. MetaMask is the recommended wallet.

   You have to make sure that you login using the same address where your MATIC tokens are present.

1. Click **Become a Validator**

   You will be asked to set up your node. If you haven't already set up your node by now, you will need to do so, else if you proceed ahead you will receive an error when you attempt to stake.

1. On the next screen, add your validator details, the commission rate, and the staking amount.
1. Click **Stake Now**.

Once the transaction is completed you will have staked successfully to become a validator. You will be asked thrice to confirm the transaction.

* Approve Transaction — this will approve your stake transaction.
* Stake — This will confirm your stake transaction.
* Save —ß This will save your validator details.

:::note

For the changes to take effect on the [staking dashboard](https://wallet.polygon.technology/staking/my-account), it requires a minimum of 12 block confirmations.

:::

### Check the balance

To check the balance of your address:

```sh
heimdallcli query auth account SIGNER_ADDRESS --chain-id CHAIN_ID
```

where

* SIGNER_ADDRESS — your [signer address](../glossary#validator).
* CHAIN_ID — the Polygon mainnet chain ID with the client prefix: `heimdall-137`.

The following output should appear:

```json
address: 0x6c468cf8c9879006e22ec4029696e005c2319c9d
coins:
- denom: matic
amount:
    i: "1000000000000000000000"
accountnumber: 0
sequence: 0
```

### Claim rewards as a validator

Once you are set up and staked as a validator, you will earn rewards for performing validator duties. When you perform validator duties dutifully, you get rewarded.

To claim rewards you can go to your [validator dashboard](https://wallet.polygon.technology/staking/my-account).

You will see two buttons on your profile:

* Withdraw Reward
* Restake Reward

#### Withdraw Reward

As a validator, you earn rewards as long as you are performing your validator duties correctly.

Clicking **Withdraw Reward** will get your rewards back to your wallet.

The dashboard will update after 12 block confirmations.

#### Restake Reward

Restaking your rewards is an easy way to increase your stake as a validator.

Clicking **Restake Reward** will restake your reward and increase your stake.

The dashboard will update after 12 block confirmations.
