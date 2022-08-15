---
id: matic-to-ethereum
title: 폴리곤에서 이더리움으로 데이터 전송
description: 계약을 통해 폴리곤에서 이더리움으로 상태나 데이터 전송
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

폴리곤에서 이더리움으로 데이터를 전송하는 메커니즘은 이더리움에서 폴리곤으로 동일한 작업을 수행하는 것과 약간 다릅니다. 이더리움 체인에서 검증자가 생성한 **체크포인트** 트랜잭션은 이를 달성하는 데 사용됩니다. 기본적으로 트랜잭션은 처음에 폴리곤에서 생성됩니다. 이 트랜잭션을 생성하는 동안 **이벤트가 발생하고** **이벤트 로그에** 폴리곤에서 이더리움으로 **전송하려는 데이터가 포함되어 있는지** 확인해야 합니다.

일정 기간(약 10-30분) 동안 이 트랜잭션은 검증자에 의해 이더리움 체인에서 확인됩니다. 체크포인트가 완료되면 폴리곤 체인에서 생성된 트랜잭션의 해시를 이더리움 체인의 **RootChainManager** 컨트랙트에 대한 증거로 제출할 수 있습니다. 이 컨트랙트는 트랜잭션의 유효성을 검사하고 이 트랜잭션이 체크포인트에 포함되어 있는지 확인하고 마지막으로 이 트랜잭션의 이벤트 로그를 디코딩합니다.

이 단계가 끝나면 **디코딩된 이벤트 로그 데이터를** 사용하여 이더리움 체인에 배포된 루트 컨트랙트에 대한 **변경을 수행**할 수 있습니다. 이를 위해 우리는 또한 이더리움의 상태 변경이 안전한 방법으로만 수행되도록 해야 합니다. 따라서 우리는 **RootChainManager** 컨트랙트에 의해서만 트리거될 수 있는 특별한 유형의 컨트랙트인 **Predicate** 컨트랙트를 사용합니다. 이 아키텍처는 폴리곤의 트랜잭션이 **RootChainManager** 컨트랙트에 의해 이더리움 체인에서 확인되고 검증될 때만 이더리움의 상태 변경이 발생하도록 합니다.

# 개요

- 트랜잭션은 폴리곤 체인에 배포된 하위 컨트랙트에서 실행됩니다.
- 이 트랜잭션에서도 이벤트가 발생합니다. 이 **이벤트**의 매개변수에는 폴리곤에서 이더리움으로 **전송되어야 하는 데이터가 포함됩니다**.
- 폴리곤 네트워크의 검증자는 특정 시간 간격(아마도 10-30분)에 이 트랜잭션을 선택하고 유효성을 검사하고 이더리움의 **체크포인트에 추가**합니다.
- **RootChain** 컨트랙트에 체크포인트 트랜잭션이 생성되고 이 [스크립트](https://github.com/rahuldamodar94/matic-learn-pos/blob/transfer-matic-ethereum/script/check-checkpoint.js)를 사용하여 체크포인트 포함을 확인할 수 있습니다.
- 체크포인트 추가가 완료되면 **matic.js** 라이브러리를 사용하여 **RootChainManager** 계약의 **exit** 함수를 호출할 수 있습니다. **exit** 함수는 이 [예제](https://github.com/rahuldamodar94/matic-learn-pos/blob/transfer-matic-ethereum/script/exit.js)와 같이 matic.js 라이브러리를 사용하여 호출할 수 있습니다.

- 스크립트를 실행하면 이더리움 체인에 폴리곤 트랜잭션 해시가 포함되어 있는지 확인한 다음 [조건부](https://github.com/rahuldamodar94/matic-learn-pos/blob/transfer-matic-ethereum/contracts/CustomPredicate.sol) 컨트랙트의 **exitToken** 함수를 차례로 호출합니다.
- 이것은 **루트 체인 컨트랙트의 상태 변경**이 항상 **안전한** 방식으로 그리고 **오직 술어 컨트랙트를 통해서만** 수행되도록 합니다.
- 주목해야 할 중요한 점은 폴리곤에서 **트랜잭션 해시를 확인**하고 **predicate 컨트랙트를 트리거하는 것**이 **단일 트랜잭션**에서 발생하므로 루트 컨트랙트의 모든 상태 변경에 대한 보안을 보장한다는 것입니다.

# 구현

이것은 폴리곤에서 이더리움으로 데이터를 전송하는 방법에 대한 간단한 데모입니다. 이 자습서는 체인을 통해 uint256 값을 전송하는 예를 보여줍니다. 그러나 데이터 유형을 전송할 수 있습니다. 그러나 데이터를 바이트 단위로 인코딩한 다음 하위 컨트랙트에서 내보내야 합니다. 루트 컨트랙트에서 최종적으로 디코딩될 수 있습니다.

1.  먼저 루트 체인과 하위 체인 컨트랙트를 만듭니다. 상태 변경을 수행하는 함수도 이벤트를 내보내는지 확인하십시오. 이 이벤트는 매개변수 중 하나로 전송할 데이터를 포함해야 합니다. 하위 및 루트컨트랙트가 어떻게 생겼는지에 대한 샘플 형식은 아래에 나와 있습니다. 이것은 setData 함수를 사용하여 값이 설정되는 데이터 변수가 있는 매우 간단한 컨트랙트입니다. setData 함수를 호출하면 Data 이벤트가 발생합니다. 컨트랙트의 나머지 부분은 이 튜토리얼의 다음 섹션에서 설명합니다.

A. 하위 컨트랙트

```javascript
contract Child {

    event Data(address indexed from, bytes bytes_data);

    uint256 public data;

    function setData(bytes memory bytes_data) public {
     data = abi.decode(bytes_data,(uint256));
     emit Data(msg.sender,bytes_data);
    }

}
```

B. 루트 컨트랙트

이 `0x1470E07a6dD1D11eAE439Acaa6971C941C9EF48f`를 루트 컨트랙트 생성자의 `_predicate` 값으로 전달합니다.

```javascript
contract Root {

    address public predicate;
    constructor(address _predicate) public{
        predicate=_predicate;
    }

   modifier onlyPredicate() {
        require(msg.sender == predicate);
        _;
    }

    uint256 public data;

    function setData(bytes memory bytes_data) public onlyPredicate{
        data = abi.decode(bytes_data,(uint256));
    }

}
```

2.  자식 및 루트 컨트랙트가 각각 폴리곤 및 이더리움 체인에 배포되면 이러한 컨트랙트는 PoS 브리지를 사용하여 매핑되어야 합니다. 이 매핑은 체인 전체에서 이러한 두 컨트랙트 간의 연결이 유지되도록 합니다. 이 매핑을 수행하기 위해 폴리곤 팀은 [discord](https://discord.com/invite/0xPolygon)를 통해 연락할 수 있습니다.

3.  주목해야 할 한 가지 중요한 점은 루트 계약에 onlyPredicate 수정자가 있다는 것입니다. predicate 컨트랙트만 루트 컨트랙트의 상태를 변경하도록 하기 때문에 항상 이 수정자를 사용하는 것이 좋습니다. predicate 컨트랙트은 폴리곤 체인에서 발생한 트랜잭션이 이더리움 체인의 RootChainManager에 의해 검증될 때만 루트 컨트랙트를 트리거하는 특수 컨트랙트입니다. 이것은 루트 컨트랙트의 안전한 상태 변경을 보장합니다.

위의 구현을 테스트하기 위해 하위 컨트랙트의 **setData** 함수를 호출하여 폴리곤 체인에서 트랜잭션을 만들 수 있습니다. 체크포인트가 완료될 때까지 이 시점에서 기다려야 합니다. 이 [크립트](https://github.com/rahuldamodar94/matic-learn-pos/blob/transfer-matic-ethereum/script/check-checkpoint.js)를 사용하여 체크포인트 포함을 확인할 수 있습니다. 체크포인트가 완료되면 matic.js SDK를 사용하여 RootChainManager의 exit 함수를 호출합니다.

```jsx
const txHash =
  "0xc094de3b7abd29f23a23549d9484e9c6bddb2542e2cc0aa605221cb55548951c";

const logEventSignature =
  "0x93f3e547dcb3ce9c356bb293f12e44f70fc24105d675b782bd639333aab70df7";

const execute = async () => {
  try {
    const tx = await maticPOSClient.posRootChainManager.exit(
      txHash,
      logEventSignature
    );
    console.log(tx.transactionHash); // eslint-disable-line
  } catch (e) {
    console.error(e); // eslint-disable-line
  }
};
```

위 스크린샷과 같이 **txHash**는 폴리곤 체인에 배포된 하위 컨트랙트에서 발생한 트랜잭션의 트랜잭션 해시입니다.

**logEventSignature**는 데이터 이벤트의 kccack-256 해시입니다. 이것은 우리가 Predicate 컨트랙트에 포함시킨 것과 동일한 해시입니다. 이 튜토리얼과 종료 스크립트에 사용된 모든 컨트랙트 코드는 [여기](https://github.com/rahuldamodar94/matic-learn-pos/tree/transfer-matic-ethereum)에서 찾을 수 있습니다.

종료 스크립트가 완료되면 이더리움 체인의 루트 컨트랙트를 조회하여 하위 컨트랙트에 설정된 변수 **데이터**의 값이 루트 컨트랙트의 **데이터** 변수에도 반영되었는지 확인할 수 있습니다.

