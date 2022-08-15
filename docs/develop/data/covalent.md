---
id: covalent
title: Using Covalent
sidebar_label: Covalent
description: Learn how to use Covalent's unified API for data 
keywords:
  - docs
  - matic
  - polygon
  - covalent
  - data
  - analytics
  - index
  - indexing
  - query
image: https://matic.network/banners/matic-network-16x9.png 
---

## Introduction

Polygon brings massive scale to Ethereum using an adapted version of Plasma 
with PoS based side chains that provides a solution for faster and extremely 
low-cost transactions with finality on the main chain. The Polygon network ensures 
liveliness using PoS checkpoints which are pushed to the Ethereum mainchain. 
This enables a single Polygon sidechain to theoretically achieve `2^16` transactions 
per block, and possibly millions of transactions on multiple chains in the future.

### Quick facts

<TableWrap>

|Property|Value|
|---|---|
|Polygon Mainnet chainId|`137`|
|Polygon Mumbai Testnet chainId|`80001`|
|Polygon Blockchain Explorer|https://polygonscan.com/|
|Block time|~3 seconds|
|Data refresh latency|~6 seconds or 2 Blocks|

</TableWrap>

:::tip Quickstart

Check out **[this introduction video](https://www.youtube.com/watch?v=qhibXxKANWE)** 
to get started.

:::

## Supported endpoints

All [__Class A__](https://www.covalenthq.com/docs/api/#tag--Class-A) endpoints are supported for the Matic mainnet and the Mumbai testnet. You can query either network via the unified API by changing the `chainId`.

:::info Endpoints

A full list of all the requests you are able to do on the Polygon network using Covalent
are available on the [Covalent API documentation](https://www.covalenthq.com/docs/api/).

:::

--- 

## Appendix

### Matic Gas token

To interact with the Matic network, MATIC tokens are required to pay as gas fees. Covalent's 
responses automatically returns `gas_*` fields in the MATIC units.

### Token mapping

Covalent maintains an on-chain real-time mapping of token addresses between Ethereum mainnet and the Matic chain. These addresses are used to reverse-lookup prices on Matic and also to return the right token logo urls.

Some example of mapped tokens:

|Token|Ethereum mainnet|Matic mainnet|
|---|---|---|
|USDT|0xdac17f958d2ee523a2206206994597c13d831ec7|0xc2132d05d31c914a87c6611c10748aeb04b58e8f|
|Uniswap UNI|0x1f9840a85d5af5bf1d1762f925bdaddc4201f984|0xb33eaad8d922b1083446dc23f610c2567fb5180f|

### Token prices

For tokens that have a mapping back to Ethereum mainnet, Covalent is able to return the mapped prices.

### Infrastructure Providers

The following provide infrastructure for this blockchain network:
* [Chainstack](../../service-providers/chainstack)
* [Ankr](../../service-providers/ankr)
* [GetBlock](../../service-providers/getblock)

