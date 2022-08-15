---
id: eth
title: ETH 입출금 가이드
sidebar_label: ETH
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

[ETH에 대한 최신 Matic.js 문서](https://maticnetwork.github.io/matic.js/docs/pos/deposit-ether/)를 확인하십시오.

## 빠른 요약

문서의 이 섹션에서는 폴리곤 네트워크에서 ERC20 토큰을 입금 및 출금하는 방법을 다룹니다. 표준에 맞는 네이밍 및 구현 패턴이 다양한 문서의 ETH, ERC20, ERC721 및 ERC1155 섹션 간에 공통 기능들이 존재합니다. 문서의 이 섹션을 사용하기 위한 가장 중요한 전제 조건은 자산을 매핑하는 것이므로 [여기](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/)에서 매핑 요청을 제출하세요.

## 소개

이 자습서는 Goerli 네트워크에 매핑된 폴리곤 테스트넷( Mumbai )을 사용하여 두 블록체인 간의 자산 전송을 보여줍니다. 이 자습서의 목적을 위해 가능하면 프록시 주소를 사용해야 한다는 점에 유의하는 것이 중요합니다. 이는 구현 컨트랙트 주소가 컨트랙트 코드에 새 업데이트가 추가될 때 변경될 수 있지만 프록시는 변경되지 않고 들어오는 모든 호출을 최신 구현으로 리디렉션하기 때문입니다. 본질적으로 프록시 주소를 사용하면 준비가 되기 전에 구현 컨트랙트에서 발생하는 변경 사항에 대해 걱정할 필요가 없습니다.

예를 들어 **RootChainManager** 주소 대신 상호 작용에 **RootChainManagerProxy** 주소를 사용하십시오. PoS 컨트랙트 주소, ABI, 테스트 토큰 주소와 같은 배포의 세부 정보는 [여기](https://docs.polygon.technology/docs/develop/ethereum-polygon/pos/deployment/)에서 찾을 수 있습니다.

자산 매핑은 애플리케이션에 PoS 브리지를 통합하는 데 필요한 단계이므로 아직 수행하지 않은 경우 [여기](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/)에서 매핑 요청을 제출하세요. 이 자습서의 목적을 위해 팀은 테스트 토큰을 배포하고 이를 PoS 브리지에 매핑했습니다. [faucet](https://faucet.polygon.technology/)에서 사용하려는 자산을 요청하고 테스트 토큰을 사용할 수 없는 경우 [Discord](https://discord.com/invite/0xPolygon)에서  팀에 문의하세요. 즉시 회신해 드리겠습니다.

다음 자습서에서는 몇 가지 코드 조각과 함께 모든 단계를 자세히 설명합니다. 그러나 PoS 브리지의 작동을 통합하고 이해하는 데 도움이 될 수 있는 모든 **예제 소스 코드**가 있는 이 [리포지토리](https://github.com/maticnetwork/matic.js/tree/master/examples)를 항상 참조할 수 있습니다.

## 높은 수준의 작업흐름

ETH 입금하기 -

1. **_RootChainManager_**에서 **_depositEtherFor_**를 호출하고 필요한 이더를 **보냅니다**.

ETH 출금하기 -

1. 폴리곤 체인에서 토큰을 **_소각_**합니다.
2. **_RootChainManager_**에서 **_exit_** 함수를 호출하여 소각 트랜잭션의 증명을 제출하십시오. 이 호출은 소각 트랜잭션이 포함된 블록에 대한 **_체크포인트가 제출된 후_**에 만들 수 있습니다.

## 단계

### 입금하기

ETH는 **RootChainManager** 컨트랙트에서 **depositEtherFor**를 호출하여 폴리곤 체인에 입금할 수 있습니다. 폴리곤 POS 클라이언트는 이 호출을 수행하기 위해 **_depositEther_** 메소드를 노출합니다.

```jsx
const result = await posClient.depositEther(<amount>);
const txHash = await result.getTransactionHash();
const txReceipt = await result.getReceipt();
```

참고: 이더리움에서 폴리곤으로의 입금은 **상태 동기화** 메커니즘을 사용하여 이루어지며 약 5-7분이 소요됩니다. 이 시간 간격을 두고 기다린 후 web3.js/matic.js 라이브러리나 메타마스크를 이용하여 잔고를 확인하는 것을 권장합니다. 탐색기는 하위 체인에서 하나 이상의 자산 전송이 발생한 경우에만 잔고를 표시합니다. 이 [링크](https://docs.polygon.technology/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos/)는 입금 이벤트를 추적하는 방법을 설명합니다.

### 소각하기

**_ETH_**는 폴리곤 체인에 **_ERC20_** 토큰으로써 입금됩니다. 인출은 ERC20 토큰 인출과 동일한 절차를 따릅니다.

토큰을 소각하고 출금 프로세스에 참여하려면 MaticWETH 컨트랙트의 **Withdraw**함수를 호출하십시오. Ether는 폴리곤 체인의 ERC20 토큰이기 때문에 폴리곤 PoS 클라이언트에서 **erc20** 토큰을 시작한 다음 **withdrawStart** 메소드를 호출하여 소각 프로세스를 시작해야 합니다.

```jsx
const erc20Token = posClient.erc20(<token address>);

// start withdraw process for 100 amount
const result = await erc20Token.withdrawStart(100);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

이 호출에 대한 트랜잭션 해시를 저장하고 소각 증명을 생성하는 동안 사용합니다.

### 종료하기


소각 거래가 포함된 블록에 대한 **체크포인트**가 제출되면, 사용자는 `RootChainManager` 컨트랙트의 **exit** 함수를 호출하고 소각 증명을 제출해야 합니다. 유효한 증명 토큰을 제출하면 사용자에게 전송됩니다. 폴리곤 POS 클라이언트 `erc20`은 이 호출을 수행하기 위해 `gatherExit` 메소드를 노출합니다. 이 함수는 체크포인트가 메인 체인에 포함된 후에만 호출할 수 있습니다. 이 [가이드](/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos#checkpoint-events)에 따라 체크포인트 포함을 추적할 수 있습니다.


```jsx
// token address can be null for native tokens like ethereum or matic
const erc20RootToken = posClient.erc20(<token address>, true);

const result = await erc20Token.withdrawExit(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
