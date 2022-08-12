---
id: withdraw-start
title: withdrawStart
keywords:
  - 'pos client, erc721, withdrawStart, polygon, sdk'
description: 'Get started with maticjs'
---

`withdrawStart` method can be used to initiate the withdraw process which will burn the specified token on polygon chain.

```
const erc721Token = posClient.erc721(<child token address>);

const result = await erc721Token.withdrawStart(<token id>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
