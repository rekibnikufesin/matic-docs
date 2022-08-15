---
id: gas
title: Gas FAQ
description: "Gas FAQ for Polygon Edge"
keywords:
  - docs
  - polygon
  - edge
  - FAQ
  - validators
  
---

## How to enforce a minimum gas price?
You can use the `--price-limit` flag provided on the server command. This will enforce your node to only accept transactions that have the gas higher or equal to the price limit you set. To make sure it is enforced on the entire network, you need to make sure all the nodes have the same price limit.


## Can you have transactions with 0 gas fees?
Yes, you can. The default price limit that nodes enforce is `0`, meaning the nodes will accept transactions that have a gas price set to `0`.

## How to set the gas(native) token total supply?

You can set a premined balance to the accounts (addresses) by using the `--premine flag`. Please note that this is a configuration from the genesis file, and it cannot be changed later.

Example on how to use the `--premine flag`:

`--premine=0x3956E90e632AEbBF34DEB49b71c28A83Bc029862:1000000000000000000000`

This sets a premined balance of 1000 ETH to 0x3956E90e632AEbBF34DEB49b71c28A83Bc029862 (the amount from the argument is in wei). 

The premined amount of the gas token will be the total supply. No other amount of the native currency (gas token) can be minted later.