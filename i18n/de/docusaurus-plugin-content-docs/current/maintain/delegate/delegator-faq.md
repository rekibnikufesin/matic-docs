---
id: delegator-faq
title: Delegator FAQ
sidebar_label: Delegator FAQ
description: Build your next blockchain app on Polygon.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

### What is the Staking Dashboard URL?

The staking dashboard URL is https://wallet.polygon.technology/staking/.

### What is the Minimum stake amount?

There is no minimum stake amount to delegate. However, you can always start with 1 Matic token

### How many rewards will I get if I delegate?

Please use the Staking Rewards Calculator to determine your estimates. https://wallet.polygon.technology/staking-rewards

### Why does my transaction take so long?

All staking transactions of Polygon happen on Ethereum for security reasons.

The time taken to complete a transaction depends on the gas fees that you have allowed and also the network congestion of Ethereum mainnet at that point in time. You can always use the “Speed Up” option to increase the gas fees so that your transaction can be completed soon

### Which wallets are currently supported?

Currently, only the Metamask extension on the desktop browser and Coinbase Wallet are supported. Additionally you can use WalletConnect and Walletlink from supported mobile wallets to interact with the Staking UI dashboard on desktop/laptop. We will be gradually adding support for other wallets soon.

### Are hardware wallets supported?

Yes, hardware wallets are supported. You can use the "Connect Hardware Wallet" option on Metamask and connect your Hardware wallet and then continue the delegation process.

### Why can’t I stake directly from Binance?

Staking through Binance is not yet supported. There will be an announcement if and when Binance starts supporting it.

### I have completed my delegation, where can I check details?

Once you have completed your delegation, wait for 12 block confirmations on Ethereum (~3-5 minutes), then on the Dashboard,you can click on the “My Delegator Details” option on the left-hand side. Or you could also click on “Show Delegator Profile” card

### Where can I check my rewards?

On the Dashboard, you can click on the “My Delegator Details” option on the left-hand side. Or you could also click on “Show Delegator Profile” card.

Check the `New Rewards` card on the right. Once you accrue rewards, you can click on the `Details` link to check rewards in detail.

### Do I need ETH to pay for Gas fees?

Yes. You should provision for ~0.05-0.1 ETH to be safe.

### Do I need to deposit Matic tokens to the Polygon Mainnet network for staking?

No. All your funds need to be on the Main Ethereum Network.

### When I try to do the transaction my Confirm button is disabled, why so?

Please check if you have enough ETH for the gas fees.

### When does reward get distributed?

The rewards are distributed whenever a checkpoint is submitted.

Currently 20188 Matic tokens are distributed proportionately on each successful checkpoint submission to each delegator based on their stake relative to the overall staking pool of all validators and delegators. Also, the percentage for the reward distributed to each delegator will vary with each checkpoint depending on the relative stake of the delegator, validator and the overall stake.

(Note that there is a 10% proposer bonus that accrues to the validator who submits the checkpoint, but over time, the effect of the extra bonus is nullified over multiple checkpoints by different validators.)

The checkpoint submission is done by one of the validators approximately every 34 minutes. This time is approximate and may vary based on validator consensus on the Polygon Heimdall layer. This may also vary based on Ethereum Network. Higher congestion in the network may result in delayed checkpoints.

You can track checkpoints on the staking contract here: https://etherscan.io/address/0x86e4dc95c7fbdbf52e33d563bbdb00823894c287

### Why does reward keep getting decreased every checkpoint?

Actual rewards earned will depend on the actual total locked supply in the network at each checkpoint. This is expected to vary significantly as more MATIC tokens get locked in the staking contracts.

Rewards will be higher, to begin with, and will keep decreasing as the locked supply % goes up. This change in locked supply is captured at every checkpoint, and rewards are calculated based on this.

### How can I claim my rewards?

You can claim your rewards instantly by clicking on the “New Rewards” card and then clicking on the Withdraw rewards button. This will transfer the rewards accumulated to your delegated account on Metamask.

### What is the Unbonding period?

The unbonding period on Polygon is approximately 9 days now. It was 19 days previously. This period applies to the originally delegated amount and re-delegated amounts - it does not apply to any rewards that were nor re-delegated.

### Will I keep receiving rewards after I unbond?

No. Once you unbond you stop receiving rewards.

### How many transactions does the delegation require?

Delegation requires 2 transactions one after the other - one Approve and another Deposit.

### What does Redelegate Rewards mean?

Redelegating your rewards simply means, that you want to increase your stake by restaking the rewards you have accumulated.

### Can I stake to any validator?

Yes. All validators are Polygon Foundation nodes currently.

We are doing a phased rollout of the Polygon mainnet. Later on, external validators will be onboarded gradually. Please see https://blog.matic.network/mainnet-is-going-live-announcing-the-launch-sequence/ for more details.

### Which browser is compatible with Staking Dashboard?

Chrome, Firefox, and Brave

### My Metamask is stuck at confirming after login, what do I do? Or nothing happens when I try to login?

Check for the following:

- If you’re using Brave, please turn off the option for “Use Crypto Wallets” in the settings panel.
- Check if you are logged into Metamask
- Check if you are logged into Metamask with Trezor/Ledger. You need to additionally turn on permission to call contracts on your Ledger device, if not enabled already.
- Check your system timestamp. If the system time is not correct, you will need to correct it.

### How do I send funds from Binance or other exchanges to Polygon wallet?

Technically, the Polygon web wallet/Staking interface is just a web application. Currently it supports the following wallets - Metamask. WalletConnect and WalletLink

So first you must withdraw your funds from Binance or any other exchange to your Ethereum address on Metamask. If you don't know how to use Metamask, google it a bit. There are plenty of videos and blogs to get started with it.

### When will be possible to become a validator and how many tokens do I need to have?

A user can earn a validator spot only by if the below conditions come into play:
1. When a validator decides to unstake from the network or
2. Wait for the auction mechanism and replace the inactive validator.

The minimum stake depends on the auction process where one user outbids another.

### If I have earned rewards while delegating, and if I add additional funds to the same validator node, what happens?

If you have not re-delegated your rewards before delegating additional funds to the same validator node, your rewards will be withdrawn automatically.

In case you dont want that to happen, re-delegate your rewards before delegating additional funds.

### I have delegated my tokens via Metamask on the Staking dashboard. Do I need to keep my system or device on?

No. Once your Delegation transactions are confirmed, and you can see your tokens reflected in the Total Stake and New Reward cards/sections, then you are done. There is no need to keep your system or device on.

### I have unbonded, how long will it take to Unbond

The unbonding period is currently set to 82 checkpoints. This is approximately 9 days. Every checkpoint takes approximately 34 minutes. However, some checkpoints could be delayed upto ~1 hour due to congestion on Ethereum.

### I have unbonded, and I now see the Claim Stake button, but it is disabled, why is that

The Claim stake button will only be enabled when your unbonding period is complete. The unbonding period is currently set at 82 checkpoints.

### Do I know when will the Claim Stake button be enabled?

Yes, under the Claim Stake button you would see a note on how many checkpoints are pending before the Claim Stake button would be enabled. Every checkpoint takes approximately 30 minutes. However, some checkpoints could be delayed upto ~1 hour due to congestion on Ethereum.

### How do I switch my delegation from Foundation Nodes to External nodes?

You can switch your Delegation using the **Move Stake** option on the Staking UI. This will switch your Delegation from the Foundation node to any other external node of your choice.

### Will there be any ubonding period when I switch Delegation from Foundation nodes to external nodes?

There will be no Unbonding period when you switch Delegation from foundation nodes to external nodes. It will be a direct switch without any delays. However, if you are unbonding from a Foundation Node or an External node there will be an Unbonding period for that.

### Are they any specifics to choose an external node during switch delegation?

No. You can choose any node of your choice.

### What happens to my rewards that are accumalated if I switch delegation from Foundation to External node?

If you haven't already claimed your rewards before switching delegation, then upon successful switch of your delegation from Foundation to External the Rewards that were accumalated till then will be transferred back to your account.

### Will delegation on the External Nodes work the same as Foundation Nodes?

Yes, it will work the same as Foundation nodes

### Will I still get rewards after delegating to an External Node?

Yes, rewards will be distributed the same as earlier with the Foundation nodes. Every successful submission of a checkpoint will yield in rewards. Rewards will be distributed and calculated at every checkpoint relative to the stake ratio, as currently implemented.

### Will there be any unbonding period if I unbond from an External Node?

Yes, the unbonding period will stay the same as currently implemented. 82 Checkpoints.

### Will there be any locking period after I switch my Delegation from Foundation to External node?

No. There won't be any locking period after you switch your delegation.

### Can I partially switch my delegation from Foundation to External nodes?

Yes, you will have the option to partially move your stake from Foundation node to an external node. The remaining partial stake will remain on the Foundation node. You can then move that to another node of your choice or the same node.

### Can I switch delegation from an external node to another external node?

No, the **Move Stake** option is only available on the Foundation Nodes. If you want to switch your delegation from an external node to another external node, you will have to unbond first and then delegate to another external node.

### When will the Foundations node be turned off?

The foundation nodes will be turned by the end of January, 2021

### Will there be any Foundation nodes in the future?

No, there won't be any foundation nodes in the future.


### How many transactions do I need to pay for Gas when I do a Move Stake?

The Move Stake is a single transaction only. All transactions would be on the Ethereum Blockchain so you would need to spend some ETH while doing the Move Stake transaction.
