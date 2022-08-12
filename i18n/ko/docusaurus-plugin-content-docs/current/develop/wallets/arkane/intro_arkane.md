---
id: intro
title: 소개
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

> Arkane, 블록체인 기술에 뛰어들고자 하는 개발자를 위한 것.


**Arkane**을 사용하면 이미 web3와 통합된 앱이 있거나 처음부터 새로운 애플리케이션을 구축하고 있는지 여부에 관계없이 앱을 폴리곤 블록체인과 쉽게 통합할 수 있습니다. Arkane은 웹과 모바일 사용자 모두에게 부드럽고 즐거운 경험을 제공합니다.

Arkane은 폴리곤 블록체인과 상호 작용하고, 블록체인 지갑을 생성하고, 대체 가능(ERC20) 및 대체 불가능 토큰(ERC721 및 ERC1155)과 같은 다양한 자산 유형을 생성하고 스마트 컨트랙트와 상호 작용할 수 있도록 도와줍니다. 우수한 개발자 경험과 함께 사용자에게 사용자 친화적인 인터페이스를 제공할 수 있습니다.

각 응용 프로그램은 고유하고 요구 사항이 다르기 때문에 Arkane과 상호 작용하는 다양한 방법을 제공합니다. Web3를 지원하는 응용 프로그램들은 [Arkane Web3 provider](https://arkane.gitbook.io/widget/web3-provider/getting-started) 를 통합하는 것이 좋습니다. 다른 응용 프로그램은 [Arkane Widget](https://arkane.gitbook.io/widget/widget/introduction)을 사용하는 것이 좋습니다.


## 주요 특징들
- 웹 및 모바일 지원
- 소셜 로그인 제공
- Fiat-on-ramp 제공
- *폴리곤에서 NFT(ERC721 및 ERC1155)를 지원하는 유일한 지갑*
- 폴리곤과 이더리움 모두 지원
- web3를 사용하여 쉽게 통합
- 주류 사용자들을 위한 빌드
- 인앱 고객 지원 제공


## 시작하기 🎉
이미 Web3 기술을 지원하는 경우 기존 Web3 Ethereum JavaScript API를 둘러싼 스마트 래퍼인 Arkane Web3 provider를 통합하여 애플리케이션 내 UX를 개선할 수 있습니다.

Web3 provider를 사용하면 최소한의 노력으로 Arkane의 잠재력을 최대한 활용할 수 있으며 기술에 익숙하지 않은 사용자가 애플리케이션을 떠나거나 타사 플러그인을 다운로드하지 않고도 온보딩할 수 있습니다. 통합은 2단계에 5분이 소요됨




**아직 Web3를 지원하지 않습니까?**
> 📦 [Widget - Arkane Connect](https://arkane.gitbook.io/widget/)에 다뤘으니 걱정할 필요없습니다.




### Step 1: 프로젝트에 라이브러리 추가하기
NPM을 통해 프로젝트에 라이브러리를 다운로드하여 설치합니다.

```
npm i @arkane-network/web3-arkane-provider
```

다음으로 페이지 헤드에 스크립트를 추가합니다.

```
<script src="/node_modules/@arkane-network/web3-arkane-provider/dist/web3-arkane-provider.js"></script>
```

페이지에 자바스크립트 파일을 추가하면 전역 Arkane 개체가 창에 추가됩니다. 이 객체는 web3 래퍼를 생성하기 위한 게이트웨이이며 위젯인 Arkane Connect를 완전히 통합합니다.

### Step 2: web3 provider 초기화하기
프로젝트에 다음 코드 줄을 추가하면 Arkane web3 provider가 로드됩니다.

```
Arkane.createArkaneProviderEngine({clientId: ‘Arketype’}).then(provider => {
    web3 = new Web3(provider);
});
```
web3 인스턴스는 이제 패리티 또는 메타마스크에 의해 주입된 것처럼 작동합니다. 지갑을 가져오고 트랜잭션에 서명하고 메시지를 보낼 수 있습니다.
### 축하합니다. 이제 당신의 dapp이 Arkane을 지원합니다 🎉
> 🧙 Arkanes 프로덕션 환경과 메인넷에 연결하려면, [앱을 등록](https://arkane-network.typeform.com/to/hzbcGJ)하고 [Client ID](https://arkane.gitbook.io/widget/deep-dive/authentication#client-id)를 요청해야 합니다.

Arkane이 제공하는 멋진 세계에 대해 더 알고 싶으시면, [그들의 문서를 확인하십시오](https://arkane.gitbook.io/widget/)

## 쇼케이스 영상
#### 폴리곤의 이메일 주소로 Matic토큰 보내기
[![Polygon Network의 이메일 주소로 Matic 토큰 보내기](https://i.snipboard.io/OzXmrN.jpg)](https://www.youtube.com/watch?v=3gehPvX4DOo&list=PLh3bJA02WlKErlpDexw_cFOlPfMQiU67U&index=1)

#### Matic NFT 전송하기
[![Matic NFT 전송하기](https://i.snipboard.io/dLkM3t.jpg)](https://www.youtube.com/watch?v=SLxAIXRv7ec&list=PLh3bJA02WlKErlpDexw_cFOlPfMQiU67U)


