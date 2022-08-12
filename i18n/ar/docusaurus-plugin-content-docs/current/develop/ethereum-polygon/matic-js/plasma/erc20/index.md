---
id: index
title: PlasmaClient
keywords:
  - 'plasma client, erc20, contract, polygon, sdk'
description: 'Get started with maticjs'
---

`plasmaClient` provides `erc20` method which helps you to interact with a erc20 token.

## Child token

```
const childERC20Token = plasmaClient.erc20(<child token address>);
```

## Root token

Root token can be initiated by providing second parameter value as `true`.

```
const parentERC20Token = plasmaClient.erc20(<root token address>, true);
```
