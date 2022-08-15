---
id: erc20
title: ERC20 입출금 가이드
sidebar_label: ERC20
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

[ERC20에 대한 최신 Matic.js 문서](https://maticnetwork.github.io/matic.js/docs/pos/erc20/)를 확인하십시오.

이 자습서는 Goerli 네트워크에 매핑된 폴리곤 테스트넷( Mumbai )을 사용하여 두 블록체인 간의 자산 전송을 보여줍니다. 이 자습서를 따라가는 동안 **주목해야 할 중요한 점**은 프록시 주소를 사용할 수 있을 때마다 항상 사용해야 한다는 것입니다. 예를 들어 상호 작용에는 **RootChainManager** 주소 대신 **RootChainManagerProxy** 주소를 사용해야 합니다. **PoS 컨트랙트 주소, ABI, 테스트 토큰 주소** 및 PoS 브리지 컨트랙트의 기타 배포 세부 정보는 [여기](/docs/develop/ethereum-polygon/pos/deployment)에서 찾을 수 있습니다.

애플리케이션에 PoS 브리지를 통합하려면 **자산 매핑**이 필요합니다. [여기](/docs/develop/ethereum-polygon/submit-mapping-request)에서 매핑 요청을 제출할 수 있습니다. 그러나 이 자습서의 목적을 위해, 우리는 이미 **테스트 토큰**을 배포하고 PoS 브리지에 매핑했습니다. 그것은 자습서를 직접 시도하기 위해 필요할 수 있습니다. [faucet](https://faucet.polygon.technology/)에서 원하는 자산을 요청할 수 있습니다. faucet에서 테스트 토큰을 사용할 수 없는 경우 [discord](https://discord.com/invite/0xPolygon)에서 우리 팀에게 문의하십시오.

다음 자습서에서는 몇 가지 코드 조각과 함께 모든 단계를 자세히 설명합니다. 그러나 PoS 브리지의 작동을 통합하고 이해하는 데 도움이 될 수 있는 모든 **예제 소스 코드**가 있는 이 [리포지토리](https://github.com/maticnetwork/matic.js/tree/master/examples/pos)를 항상 참조할 수 있습니다.

## 높은 수준의 작업흐름

ERC20 입금하기

1. 예치해야 하는 토큰을 사용하기 위해 **_ERC20Predicate_** 컨트랙트를 **_승인합니다_**.
2. **_RootChainManager_**에서 **_depositFor_**를 호출합니다.

ERC20 출금하기 -

1. 폴리곤 체인에서 토큰을 **_소각_**합니다.
2. **_RootChainManager_**에서 **_exit_** 함수를 호출하여 소각 트랜잭션의 증명을 제출하십시오. 이 호출은 소각 트랜잭션이 포함된 블록에 대한 **_체크포인트가 제출된 후_**에 만들 수 있습니다.

## 단계 세부정보

---

### 승인하기

이는 **_ERC20Predicate_**가 **_transferFrom_** 함수를 호출할 수 있도록 하는 정상적인 ERC20 승인입니다. 폴리곤 POS 클라이언트는 이 호출을 수행하기 위한 **_approve_** 메소드를 노출합니다.

```jsx
const execute = async () => {
  const client = await getPOSClient();
  const erc20RootToken = posClient.erc20(<root token address>,true);
  const approveResult = await erc20Token.approve(100);
  const txHash = await approveResult.getTransactionHash();
  const txReceipt = await approveResult.getReceipt();
}
```

### 입금하기

토큰은 사전에 매핑되고 전송 승인을 받아야 합니다. 폴리곤 POS 클라이언트는 이 호출을 수행하기 위해 **_deposit_** 메소드를 노출합니다.

```jsx
const execute = async () => {
  const client = await getPOSClient();
  const erc20RootToken = posClient.erc20(<root token address>, true);

  //deposit 100 to user address
  const result = await erc20Token.deposit(100, <user address>);
  const txHash = await result.getTransactionHash();
  const txReceipt = await result.getReceipt();

}
```

> 참고: 이더리움에서 폴리곤으로의 입금은 상태 동기화 메커니즘을 사용하여 발생하며 약 5-7분 정도 걸립니다. 이 시간 간격을 두고 기다린 후 web3.js/matic.js 라이브러리나 메타마스크를 이용하여 잔고를 확인하는 것을 권장합니다. 탐색기는 하위 체인에서 하나 이상의 자산 전송이 발생한 경우에만 잔고를 표시합니다. 이 [링크](/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos)는 입금 이벤트를 추적하는 방법을 설명합니다.

### 소각을 위한 WithdrawStart 메소드

*withdrawStart* 메소드는 폴리곤 체인에서 지정된 수량을 소각할 인출 프로세스를 시작하는 데 사용할 수 있습니다.

```jsx
const execute = async () => {
  const client = await getPOSClient();
  const erc20Token = posClient.erc20(<child token address>);

  // start withdraw process for 100 amount
  const result = await erc20Token.withdrawStart(100);
  const txHash = await result.getTransactionHash();
  const txReceipt = await result.getReceipt();
}
```

이 호출에 대한 트랜잭션 해시를 저장하고 소각 증명을 생성하는 동안 사용합니다.

### 종료하기

소각 트랜잭션이 포함된 블록에 대한 **_체크포인트_**가 **_제출되면_** 사용자는 **_RootChainManager_** 컨트랙트의 **_exit_** 함수를 호출하고 소각 증명을 제출해야 합니다. 유효한 증명 토큰을 제출하면 사용자에게 전송됩니다. Polygon Plasma 클라이언트는 이 호출을 수행하기 위해 **_withdrawExit_** 메소드를 노출합니다. 이 함수는 체크포인트가 메인 체인에 포함된 후에만 호출할 수 있습니다. 이 [가이드](/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos#checkpoint-events)에 따라 체크포인트 포함을 추적할 수 있습니다.

*withdrawExit* 메소드는 *withdrawStart* 메소드에서 txHash를 사용하여 인출 프로세스를 종료하는 데 사용할 수 있습니다.

참고- 인출을 종료하려면 withdrawStart 트랜잭션에 체크포인트가 있어야 합니다.


```jsx
const execute = async () => {
  const client = await getPOSClient();
  const erc20RootToken = posClient.erc20(<root token address>, true);
  const result = await erc20Token.withdrawExit(<burn tx hash>);
  const txHash = await result.getTransactionHash();
  const txReceipt = await result.getReceipt();
}
```
