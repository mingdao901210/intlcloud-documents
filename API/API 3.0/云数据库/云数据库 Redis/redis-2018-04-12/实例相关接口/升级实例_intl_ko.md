## 1. API 설명

API 요청 도메인 이름: redis.tencentcloudapi.com.

인스턴스 업그레이드

기본적 API 요청 빈도 제한: 20회/초.

주의: 이 API는 금융 지역을 지원합니다. 금융 지역과 비금융 지역은 격리되어 있고 서로 통하지 않아서, 공통 매개변수 Region이 금융 지역 도메인(예: ap-shanghai-fsi)일 경우, 금융 지역의 도메인 이름을 동시에 지정해야 하며, Region의 도메인과 일치시키는 것이 가장 좋습니다. 예: redis.ap-shanghai-fsi.tencentcloudapi.com.



## 2. 입력 매개변수

다음 요청 매개변수 리스트에는 API 요청 매개변수 및 일부 공통 매개변수만 나열되며, 완전한 공통 매개변수 리스트는 [공통 요청 매개변수](/document/api/239/20005)를 참조하십시오.

| 매개변수 이름 | 필수 여부 | 유형 | 설명 |
|---------|---------|---------|---------|
| Action | 예 | String | 공통 매개변수, 이 API 값: UpgradeInstance |
| Version | 예 | String | 공통 매개변수, 이 API 값: 2018-04-12 |
| Region | 예 | String | 공통 매개변수, 자세한 내용은 제품이 지원되는 [지역 리스트](/document/api/239/20005#.E5.9C.B0.E5.9F.9F.E5.88.97.E8.A1.A8)를 참조하십시오. |
| InstanceId | 예 | String | 인스턴스 ID |
| MemSize | 예 | Integer | 샤딩 크기(MB) |
| RedisShardNum | 아니요 | Integer | 샤딩 수량, Redis 2.8 마스터/슬레이브 버전, CKV 마스터/슬레이브 버전과 Redis 2.8 스탠드 얼로운은 입력할 필요가 없습니다. |
| RedisReplicasNum | 아니요 | Integer | 복제본 수량, Redis 2.8 마스터/슬레이브 버전, CKV 마스터/슬레이브 버전과 Redis 2.8 스탠드 얼로운은 입력할 필요가 없습니다. |

## 3. 출력 매개변수

| 매개변수 이름 | 유형 | 설명 |
|---------|---------|---------|
| DealId | String | 주문 ID |
| RequestId | String | 유일한 요청 ID, 매회 요청 시마다 반환됩니다. 문제를 찾을 경우 해당 요청의 RequestId를 제공해야 합니다. |

## 4. 예시

### 예시 1 요청 예시

#### 입력 예시

```
https://redis.tencentcloudapi.com/?Action=UpgradeInstance
&InstanceId=crs-5qlrlhux
&MemSize=4096
&<공통 요청 매개변수>
```

#### 출력 예시

```
{
    "Response": {
        "DealId": "6954227",
        "RequestId": "4daddc97-0005-45d8-b5b8-38514ec1e97c"
    }
}
```


## 5. 개발자 리소스

### API Explorer

**이 도구는 온라인 호출, 서명 인증, SDK 코드 생성 및 빠른 인덱스 API 등의 기능을 제공하여, 클라우드 API의 어려움을 크게 줄일 수 있으므로 사용을 추천합니다.**

* [API 3.0 Explorer](https://console.cloud.tencent.com/api/explorer?Product=redis&Version=2018-04-12&Action=UpgradeInstance)

### SDK

클라우드 API 3.0은 매칭되는 소프트웨어 개발 키트(SDK)를 제공하고 여러 가지 프로그래밍 언어를 지원하여 더욱 편리한 API 호출이 가능합니다.

* [Tencent Cloud SDK 3.0 for Python](https://github.com/TencentCloud/tencentcloud-sdk-python)
* [Tencent Cloud SDK 3.0 for Java](https://github.com/TencentCloud/tencentcloud-sdk-java)
* [Tencent Cloud SDK 3.0 for PHP](https://github.com/TencentCloud/tencentcloud-sdk-php)
* [Tencent Cloud SDK 3.0 for Go](https://github.com/TencentCloud/tencentcloud-sdk-go)
* [Tencent Cloud SDK 3.0 for NodeJS](https://github.com/TencentCloud/tencentcloud-sdk-nodejs)
* [Tencent Cloud SDK 3.0 for .NET](https://github.com/TencentCloud/tencentcloud-sdk-dotnet)

### TCCLI

* [Tencent Cloud CLI 3.0](https://cloud.tencent.com/document/product/440/6176)

## 6. 오류 코드

다음은 API 비즈니스 로직과 관련된 오류 코드만 나열하며 다른 오류 코드는 [공통 오류 코드](/document/api/239/20007#.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81)를 참조하십시오.

| 오류 코드 | 설명 |
|---------|---------|
| InvalidParameterValue.MemSizeNotInRange | 요청 요량은 판매 범위 내에 있지 않습니다. |
| InvalidParameterValue.ReduceCapacityNotAllowed | 요청 용량이 작아서 용량 줄임을 지원하지 않습니다. |
| LimitExceeded.InvalidMemSize | 요청 용량이 판매 사양에 있지 않습니다(memSize는 1024의 정수 배수여야 하며, 단위는 MB입니다). |
| ResourceNotFound.InstanceNotExists | serialId에 따라 해당 Redis를 찾지 못했습니다. |
| ResourceUnavailable.AccountBalanceNotEnough | 요청 주문 번호가 존재하지 않습니다. |
| ResourceUnavailable.InstanceStatusAbnormal | Redis 상태 이상으로 해당 프로세스를 실행할 수 없습니다. |
| UnauthorizedOperation.NoCAMAuthed | cam 권한이 없습니다. |
