---
id: withdraw-exit-many
title: withdrawExitMany
keywords:
  - 'pos client, erc721, withdrawExitMany, polygon, sdk'
description: 'Get started with maticjs'
---

`withdrawExitMany` method can be used to exit the withdraw process by using the txHash from `withdrawStartMany` method.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const result = await erc721RootToken.withdrawExitMany(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
