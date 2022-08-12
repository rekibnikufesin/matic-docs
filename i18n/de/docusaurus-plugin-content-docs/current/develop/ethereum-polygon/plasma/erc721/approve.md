---
id: approve
title: approve
keywords:
  - 'plasma client, erc721, approve, polygon, sdk'
description: 'Get started with maticjs'
---

# approve

`approve` method can be used to approve required amount on root token.

```
const erc721RootToken = plasmaClient.erc721(<root token address>,true);

const approveResult = await erc721RootToken.approve(<token id>);

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
