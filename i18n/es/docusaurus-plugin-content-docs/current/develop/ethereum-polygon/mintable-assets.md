---
id: mintable-assets
title: Polygon Mintable Assets
description: Build your next blockchain app on Polygon.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

## What are Polygon mintable tokens?

Assets can be transferred to and from across the Ethereum and Polygon chain using the PoS bridge. These assets include ERC20, ERC721, ERC1155 and many other token standards. Most of the assets are pre-existing on Ethereum chain. But new assets can be created on the Polygon chain as well and moved back to Ethereum chain as and when required. This can save lots of gas and time that is spent on token minting on Ethereum. Creation of assets on the Polygon chain is much easier and a more recommended approach. These assets can be moved to Ethereum chain when required. Such type of assets are called Polygon mintable assets.

In the case of Polygon Mintable tokens, assets are created on Polygon. When a Polygon minted asset has to be moved to Ethereum, the asset has to be burned first and then a proof of this burn transaction has to be submitted on the Ethereum chain. The RootChainManager contract calls a special predicate contract internally. This predicate contract directly calls the mint function of the asset contract on Ethereum and the tokens are minted to the users address. This special predicate is called the MintableAssetPredicate.

## What are the requirements to be satisfied?

There are a few conditions that have to be strictly followed when we have to create an asset on Polygon and then move it back to Ethereum.

### Contract to be deployed on Polygon chain
You can either deploy

- A mintable token contract on the Polygon chain or
- Submit a mapping request and the mintable token contract can be autodeployed for you on the Polygon chain via the Mapper tool. You just need to submit a mapping request at [https://mapper.polygon.technology/](https://mapper.polygon.technology/) and leave the child contract field blank in the form. Also, do remember to choose the Mintable option in the form.

Please visit this [link](/docs/develop/ethereum-polygon/submit-mapping-request) to understand how to create a new mapping request.

- If you want to deploy the contract by yourself, then the child contract should look like the following. You are free to make custom changes to this contract, but ensure that the `deposit`, `withdraw` and `mint` functions are present.

    - ChildMintableERC20 -  [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC20.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC20.sol)
    - ChildMintableERC721 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC721.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC721.sol)
    - ChildMintableERC1155 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC1155.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/ChildMintableERC1155.sol)

- Most importantly, the child manager contract on Polygon should be given the depositor role in the asset contract deployed on Polygon. Only this child manager proxy address should have the rights to deposit tokens on Polygon.
- Be sure to verify both contracts on Polygonscan and Etherscan accordingly, before submitting mapping request.

Child Manager contract addresses:

```
Mumbai: 0xb5505a6d998549090530911180f38aC5130101c6
Mainnet: 0xA6FA4fB5f76172d178d61B04b0ecd319C5d1C0aa
```

Please do mention the contract address of the deployed child token when you submit the mapping request.

> Note that the Ethereum contract needs to be deployed as shown in the next step - no minting needs to be done on Ethereum though. It is required so that tokens can be withdrawn to Ethereum if need be.

### Contract to be deployed on Ethereum

- A token contract has to be deployed on the Ethereum chain and it should look like this.
    - MintableERC20 -  [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC20.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC20.sol)
    - MintableERC721 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC721.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC721.sol)
    - MintableERC1155 - [https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC1155.sol](https://github.com/maticnetwork/pos-portal/blob/master/flat/DummyMintableERC1155.sol)

- Most importantly, The `MintableAssetProxy` contract deployed on Ethereum should be given the minter role in the asset contract deployed on Ethereum. Only this predicate proxy address should have the rights to mint tokens on Ethereum.

- This role can be granted by calling the grantRole() function in the token contracts on the root chain. The first parameter is the value of PREDICATE_ROLE constant which is **0x12ff340d0cd9c652c747ca35727e68c547d0f0bfa7758d2e77f75acef481b4f2** and the second parameter is the token predicate proxy address which is given below,


    ```jsx
    Ethereum Mainnet
    "MintableERC20PredicateProxy"  : "0x9923263fA127b3d1484cFD649df8f1831c2A74e4",
    "MintableERC721PredicateProxy" : "0x932532aA4c0174b8453839A6E44eE09Cc615F2b7",
    "MintableERC1155PredicateProxy": "0x2d641867411650cd05dB93B59964536b1ED5b1B7",
    ```

    ```jsx
    Goerli Testnet
    "MintableERC20PredicateProxy"  : "0x37c3bfC05d5ebF9EBb3FF80ce0bd0133Bf221BC8",
    "MintableERC721PredicateProxy" : "0x56E14C4C1748a818a5564D33cF774c59EB3eDF59",
    "MintableERC1155PredicateProxy": "0x72d6066F486bd0052eefB9114B66ae40e0A6031a",
    ```

