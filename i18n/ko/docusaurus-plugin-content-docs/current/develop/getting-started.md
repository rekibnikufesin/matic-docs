---
id: getting-started
title: 폴리곤 PoS에서 개발하기
sidebar_label: Quick Start
description: 폴리곤에서 다음 블록체인 앱을 만듭니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

::::주의 개발문서는 업데이트중임

개발문서는 업데이트 및 개선되고 향상되고 있으며, 변경될 수 있습니다. 궁금한 내용이나 제안 사항이 있으면 언제든지 문의해주시기 바랍니다.

:::

블록체인 애플리케이션을 개발할 수 있는 가장 혁신적이고 흥미로운 플랫폼인 **폴리곤 블록체인**에 오신 것을 환영합니다. 블록체인 기술은 디지털 세상이 데이터를 처리하고 비즈니스를 수행하는 방식을 혁신할 태세를 갖추고 있습니다. 이 혁신의 일부가 되어 폴리곤에서 탈중앙화 애플리케이션(dApp) 개발을 시작하는데 앞장설 수 있습니다.

이 페이지는 **폴리곤 생태계**에 대한 가이드 역할을 합니다. 특히 폴리곤 뿐만 아니라 전체 블록체인 기반으로 구축을 시작하는 방법을 빠르게 알려줄 유용한 리소스 및 웹사이트에 대한 링크를 찾을 수 있습니다. Telegram/Discord로 언제든지 문의해 주세요.

## **개발자 Quick Start**

이더리움 개발자라면 이미 폴리곤 개발자입니다. 이더리움 블록체인에서 익숙한 모든 도구는 즉시 폴리곤에서 지원됩니다. 예를 들어 Truffle, Remix 및 Web3js가 있습니다. Polygon RPC로 전환하고 시작하십시오!

**뭄바이**라고 하는 폴리곤의 테스트 네트워크는 **이더리움의 Goërli 테스트넷**과 연결됩니다. 모든 네트워크 관련 세부정보는 [네트워크 문서](/docs/develop/network-details/network)에서 찾을 수 있습니다.

- [메타마스크 지갑](/docs/develop/metamask/overview) 또는 [Arkane 지갑](/docs/develop/wallets/arkane/intro) 설정하기
- 폴리곤에 컨트랙트 배포하기
    - [Alchemy 이용하기](/docs/develop/alchemy)
    - [Chainstack 이용하기](/docs/develop/chainstack)
    - [QuickNode 이용하기](/docs/develop/quicknode)
    - [Remix 이용하기](/docs/develop/remix)
    - [Truffle 이용하기](/docs/develop/truffle)
    - [Hardhat 이용하기](/docs/develop/hardhat)
    - [Replit 이용하기](/docs/develop/replit)
- 메타마스크에 폴리곤을 추가하거나 [Arkane](/docs/develop/wallets/arkane/network)을 통해 직접 [RPC](https://docs.polygon.technology/docs/develop/metamask/config-polygon-on-metamask)로 폴리곤에 연결하기

::::참고
web3.js로 같은 RPC를 이용할 수 있습니다.
:::

```jsx
// Javascript
const Web3 = require('Web3')

// Sign up for a free dedicated RPC URL at https://rpc.maticvigil.com/ or other hosted node providers.
const web3 = new Web3('https://rpc-mumbai.matic.today')

// Sign up for a free dedicated RPC URL at https://www.alchemy.com or other hosted node providers.
const web3 = new Web3('https://polygon-mainnet.g.alchemy.com/v2/<your-api-key>')

// web3 object is now connected with Polygon's node
```

---

### **이미 dApp이 있습니까?**

- **이더리움 체인(또는 EVM 기반 체인에서 그 문제를 위해) 마이그레이션하기**

    모든 스마트 계약을 폴리곤 체인에 직접 배포합니다. EVM과 호환되는 한 기본 아키텍처에 대해 걱정할 필요가 없습니다!

    [폴리곤에 dApp 배포하기](https://docs.polygon.technology/docs/integrate/quickstart)

- **폴리곤을 더 빠른 트랜잭션 레이어로 사용하기**

    메인넷에 배포된 dApp의 트랜잭션 레이어로 폴리곤을 사용하면  토큰 매핑을 시작할 수 있습니다.

    폴리곤에서 매핑된 토큰 얻기: 👋🏼 [http://bit.ly/matic-technical-group](http://bit.ly/matic-technical-group)에서 핑합니다.

### **폴리곤에서 새로운 dApp을 구축하시겠습니까?**

**구축 시작하기!**

- [풀 스택 dApp: 튜토리얼 시리즈](https://kauri.io/full-stack-dapp-tutorial-series/5b8e401ee727370001c942e3/c)
- 도구 알아보기:

    - [Web3js](https://www.dappuniversity.com/articles/web3-js-intro), [Ethers.js](https://docs.ethers.io/v5/),[Remix](https://docs.polygon.technology/docs/develop/remix/), [Truffle](https://docs.polygon.technology/docs/develop/truffle), [Metamask](/docs/develop/metamask/overview), [Arkane](/docs/develop/wallets/arkane/intro)
- [Fauna, Polygon 및 React를 사용하여 dApp 개발하기](/docs/develop/dapp-fauna-polygon-react)
- [웹훅 통합하기](https://docs.alchemy.com/alchemy/enhanced-apis/notify-api/using-notify)
- [디버그 네임스페이스가 있는 아카이브 노드](https://www.quicknode.com/chains/matic?utm_source=polygon_docs&utm_campaign=ploygon_docs_contract_guide)

**고급**

- [사이드체인과 플라즈마](https://docs.polygon.technology/docs/home/blockchain-basics/sidechain)
- [폴리곤의 아키텍처 및 보안](https://docs.polygon.technology/docs/home/architecture/security-models)
- [플라즈마를 사용하는 경우](https://docs.polygon.technology/docs/home/architecture/security-models)
- [메인체인에서 폴리곤 체인으로 자산 이동하기: 플라즈마 방식](/docs/develop/ethereum-polygon/matic-js/get-started.md)
- [플라즈마 자산 스왑하기](https://docs.polygon.technology/docs/develop/advanced/swap-assets)

**기타 링크**

- [동영상 자습서 라이브러리](https://www.notion.so/Video-Tutorials-Library-f16cbb8c3d9d47d8bc809e06519f110c)
- [팀에 의한 글들](https://www.notion.so/Writings-by-the-Team-c979819406894abb964cb50ae197f376)
- [매틱 도구들](https://www.notion.so/f5739c3ed3cc40e3ae71d5935a72143d)
- [FAQs](https://docs.polygon.technology/docs/faq/technical-faqs)

### **개발자 도구들 배우기**

- [CryptoZombies](https://cryptozombies.io/)
- [풀 스택 dApp 튜토리얼 시리즈](https://kauri.io/#collections/Full%20Stack%20dApp%20Tutorial%20Series/full-stack-dapp-tutorial-series-intro/)
- [Alchemy (블록체인 APIs and 개발자 도구들)](https://alchemy.com/?a=polygon-docs)
- [QuickNode(다중 체인 블록체인 인프라)](https://www.quicknode.com/docs/polygon?utm_source=polygon_docs&utm_campaign=ploygon_docs_contract_guide)
- [Infura Docs](https://infura.io/docs)
- [Infura 및 폴리곤 시작하기](https://docs.infura.io/infura/networks/polygon-pos/tutorials/send-a-transaction)
- [Truffle Suite 문서](https://www.trufflesuite.com/docs)(권장됨)
- [Truffle 자습서](https://www.trufflesuite.com/tutorial) (권장됨)
- [Parity Wiki](https://openethereum.github.io/)
- [Geth docs](https://geth.ethereum.org/)
- [Remix](https://remix.ethereum.org/)
- [OpenZeppelin Docs](https://docs.openzeppelin.com/)
- [Ethernaut](https://ethernaut.openzeppelin.com/)
    - 보안을 가르치는 게임
- [이더 캡쳐하기](https://capturetheether.com/)
    - 보안을 가르치는 게임

### **개발의 기초 배우기**

- [풀 스택 dapp 튜토리얼 시리즈](https://kauri.io/#collections/Full%20Stack%20dApp%20Tutorial%20Series/full-stack-dapp-tutorial-series-intro/)
- [Web3 개발자 스택](https://www.quicknode.com/guides/web3-sdks/the-web3-developer-stack)
- [REMIX IDE를 사용하여 스마트 컨트랙트 배포](https://www.quicknode.com/guides/solidity/how-to-deploy-a-smart-contract-on-matic-polygon)
- [토큰 생성 방법(ERC20)](https://www.quicknode.com/guides/solidity/how-to-create-and-deploy-an-erc20-token)
- [IPFS와 이더리움 통합](https://www.quicknode.com/guides/web3-sdks/how-to-integrate-ipfs-with-ethereum)
- [Hello World 스마트 컨트랙트](https://docs.alchemy.com/alchemy/tutorials/hello-world-smart-contract)
- [NFT를 만드는 방법](https://docs.alchemy.com/alchemy/tutorials/how-to-create-an-nft)
- [Truffle tutorial](https://www.trufflesuite.com/tutorial)
- [Dapp University](https://www.youtube.com/channel/UCY0xL8V6NzzFcwzHCgB8orQ)
- [ConsenSys Academy 개발자 프로그램 주문형 과정](https://consensys.net/academy/ondemand/)
- [이더리움이란?](https://blockgeeks.com/guides/ethereum/)
- [이더리움 마스터링 읽기](https://github.com/ethereumbook/ethereumbook)
- [OpenZeppelin 학습 문서](https://docs.openzeppelin.com/learn/)

### **해커톤에 참여하기**

해커톤은 전 세계의 블록체인 컨퍼런스와 밋업에서 열립니다. 순식간에 비행기를 탈 수는 없지만 일부 회의나 프로젝트에서는 인터넷에 연결된 모든 사람이 참여할 수 있는 가상 해커톤을 주최합니다. [폴리곤의 해커톤 디스코드 채널](https://discord.com/invite/0xPolygon) 확인하기

# **개발에 발 맞추기**

### **소셜 미디어**

Dapp 개발은 네트워크 탈중앙화를 장려하며 이를 구현하기도 합니다. 개발자는 전 세계 어디에나 있습니다! 라서 소셜 미디어는 사람들이 시간대에 관계없이 연락을 유지하는 데 중요해졌습니다. 인기있는 플랫폼 외에도 Telegram, Discord 및 Gitter와 같은 플랫폼에 익숙하지 않을 수 있습니다.

Reddit:

- https://reddit.com/r/0xPolygon
- https://reddit.com/r/ethereum
- https://reddit.com/r/ethdev
- https://reddit.com/r/ethereumnoobies

Twitter:

- [Bankless: How to Use Crypto Twitter](https://bankless.substack.com/p/how-to-use-crypto-twitter-to-level-77c)
- [EthHub](https://twitter.com/ethhub_io)
- [QuickNode](https://twitter.com/QuickNode)
- [Alchemy](https://twitter.com/AlchemyPlatform)
- [CodeFi](https://twitter.com/ConsenSysCodefi)
- [ConsenSys Labs](https://twitter.com/ConsenSys)
- [Universal Login](https://twitter.com/unilogin)
- [MetaCartle](https://twitter.com/meta_cartel)
- [Ethereum Foundation](https://twitter.com/ethereum)
- [DAI Dao](https://twitter.com/rDAI_dao)
- [ETHGlobal](https://twitter.com/ETHGlobal)
- [MakerDao](https://twitter.com/MakerDAO)
- [DeFi Pulse](https://twitter.com/defipulse)
- [DeFi Prime](https://twitter.com/defiprime)
- [Uniswap](https://twitter.com/UniswapExchange)
- [Compound](https://twitter.com/compoundfinance)
- [Gnosis](https://twitter.com/gnosisPM)
- [Nexus Mutual](https://twitter.com/NexusMutual)
- [Argent](https://twitter.com/argentHQ)
- [The Token Analyst](https://twitter.com/thetokenanalyst)
- [EF Devcon account](https://twitter.com/EFDevcon)
- [Status](https://twitter.com/ethstatus?lang=en)
- [OpenZeppelin](https://twitter.com/openzeppelin)

### **Newsletters**

- [Week In Ethereum](https://weekinethereumnews.com/)
- [QuickNode: #Web3Vibes](https://www.getrevue.co/profile/quiknode)
- [Alchemy: Supercharged](https://www.alchemy.com/newsletter)
- [EthHub](https://ethhub.io/)
- [Chain Letter](https://forms.technologyreview.com/chain-letter/)
- [ConsenSys Newsletter Digest](https://share.hsforms.com/1HiFwsb55S5GUf-EOe0KP8Q2urwb?email=)

### **Podcasts**

- [Zero Knowledge](https://www.zeroknowledge.fm/)
- [Into the Ether](https://ethhub.substack.com/)
- [Unconfirmed](https://unconfirmed.libsyn.com/)
- [Epicenter](https://epicenter.tv/)
- [11:FS Blockchain Insider](https://bi.11fs.com/)

## 포인터

이것이 압도적이라면 괜찮습니다! 바로 불 속으로 뛰어들어 해킹을 시작할 수 있습니다. 다음은 리소스, 리포지토리 및 문서에 대해 자세히 알아보기 전에 몇 가지 지침입니다.

1. **최첨단에 있는 비용을 주의하십시오**: 일반적인 틈새 프로그래밍과 마찬가지로 dApp 및 블록체인 개발은 매우 빠르게 움직입니다. 조사하는 동안 당신은 문서 사이트에서 복잡한 코드 저장소, 404들을 찾을 수도 어떠한 문서도 찾지 못할 수도 있습니다. 이것을 걸림돌로 보지 말고, **기회**로의 초대로 보십시오. 개발자 채널에서 Ping을 실행하고 Discord / Gitter / Telegram 채널을 찾고 Stack Overflow 또는 Reddit에 게시하십시오. 커뮤니티의 응답 속도와 개방성에 놀랄 수 있습니다.
2. **학습 곡선은 험난할 수 있지만 진입 장벽은 낮습니다**. 물론 모든 커뮤니티에는 불만이 있지만 일을 하고 노력하면 알게 될 것입니다. 프로젝트는 외부인의 끌어오기 요청을 환영하며 다른 모든 리소스를 소진한 경우 지원이 있을 것입니다. 우리는 더 나은 세상을 만들기 위해 노력하고 있으며 얻을 수 있는 모든 도움을 사용할 수 있습니다. 당신이 여기 있어서 기쁩니다.
