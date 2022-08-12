---
id: deposit
title: deposit
keywords:
  - 'pos client, erc20, approveMax, polygon, sdk'
description: 'Get started with maticjs'
---

# deposit

`deposit` method can be used to deposit required amount from root token to child token.

```
const erc20RootToken = plasmaClient.erc20(<root token address>,true);

//deposit 100 to user address
const result = await erc20Token.deposit(100, <user address>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
