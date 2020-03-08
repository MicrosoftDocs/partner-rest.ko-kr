---
title: 조회 리소스
description: 조회 리소스는 고객, Microsoft 또는 다른 파트너의 잠재 고객을 나타냅니다.
ms.date: 05/17/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-csp
ms.localizationpriority: medium
ms.openlocfilehash: 147afef315bf5d9a8e3b5d045259ed2c94bf6acd
ms.sourcegitcommit: 50d18c96d24755174beb4fcb694223325a7fe450
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "76542048"
---
# <a name="referral-resources"></a>조회 리소스

적용 대상:

- 파트너 센터

이러한 리소스는 고객, Microsoft 또는 다른 파트너의 잠재 고객을 나타냅니다.

## <a name="referral"></a>조회

조회를 나타냅니다.

| 속성              | 형식                                              | 설명                                                                                                       |
|-----------------------|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Id                    | string                                            | 이 조회의 ID입니다.                                                                                         |
| EngagementId          | string                                            | 이 조회의 EngagementID입니다. 단일 EngagementID에 여러 조회를 연결할 수 있습니다.                 |
| 이름                  | string                                            | 조회의 이름입니다.                 |
| ExternalReferenceId   | string                                            | 조회에 대한 외부 식별자입니다. 예: 고유한 Dynamics 365 리드/기회 ID 저장                    |
| CreatedDateTime       | UTC 날짜/시간 형식의 문자열                    | 조회가 만들어진 날짜입니다.                                                                                |
| UpdatedDateTime       | UTC 날짜/시간 형식의 문자열                    | 조회가 마지막으로 업데이트된 날짜입니다.                                                                           |
| ExpirationDateTime    | UTC 날짜/시간 형식의 문자열                    | 조회가 만료되는 날짜입니다.                                                                                |
| 상태                | [ReferralStatus](referral-resources.md#referralstatus)      | 조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다. |
| Substatus          | [ReferralSubstatus](referral-resources.md#referralsubstatus)      | 조회 하위 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다. |
| StatusReason          | string                                            | 상태에 대한 설명 메시지입니다. 예: 조회가 손실 된 이유는 무엇 인가요? |
| ReferralType          | [ReferralType](referral-resources.md#referraltype)          | 조회 유형을 나타냅니다.                                                                                     |
| Qualification         | [ReferralQualification](referral-resources.md#referralqualification)| 조회 품질을 나타냅니다.                                                                           |
| CustomerProfile       | [CustomerProfile](referral-resources.md#customerprofile)    | 고객에 대한 정보입니다.                                                                                     |
| Consent               | [Consent](referral-resources.md#consent)                    | Consent는 다른 조직과 정보를 공유하고 사용자에게 연락할 수 있도록 플래그를 지정합니다.         |
| 세부 정보               | [ReferralDetails](referral-resources.md#referraldetails)    | 고객 세부 정보, 메모, 거래 가격, 통화 마감일입니다.                                                                |
| 팀                  | [Member](referral-resources.md#member)                      | 관련된 조직의 사용자를 나타냅니다.                                |
| InviteContext         | [InviteContext](referral-resources.md#invitecontext)        | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다.  |
| ETag                  | string                                            | ETag는 리소스를 업데이트할 때 동시성 검사를 하는 과정에서 필수적으로 사용됩니다. |
| 대상         | [ReferralTarget](referral-resources.md#target)        | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다.  |

## <a name="referralstatus"></a>ReferralStatus

조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 값           | 설명                                                                                |
|-----------------|---------------------------------------------------------------------------------------------|
| 없음            |                                                                                             |
| 새로 만들기             | 새 조회를 나타냅니다.                                                                 |
| 활성          | 활성 조회를 나타냅니다.                                                             |
| 종결          | 닫힌 조회를 나타냅니다.                                                              |

## <a name="referralsubstatus"></a>ReferralSubstatus

조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 값           | 설명                                                                                |
|-----------------|--------------------------------------------------------------------------------------------|
| 없음            |                                                                                            |
| 보류 중         | 보류 중인 새 조회를 나타냅니다.                                                 |
| 받은 상태 메시지 수        | 받은 새 조회를 나타냅니다.                   |
| 수락됨        | 수락된 활성 조회를 나타냅니다.                                                    |
| 성공             | 성공하여 닫힌 조회를 나타냅니다.                                            |
| 실패            | 실패하여 닫힌 조회를 나타냅니다.                                           |
| 거부됨        | 거부되어 닫힌 조회를 나타냅니다.                                       |
| 만료됨         | 만료되어 닫힌 조회를 나타냅니다.                                             |

### <a name="status--substatus-transition-states"></a>상태 및 하위 상태 전환 상태

| 상태                | 허용되는 상태 전환     | 허용되는 하위 상태                |
|-----------------------|-------------------------------|---------------------------------------|
| 새로 만들기                   | 새, 활성, 닫힘           | 보류 중, 받음                     |
| 활성                | 활성, 닫힘                | 수락됨                              |
| 종결                | 종결                        | 성공, 실패, 거부됨, 만료됨          |

## <a name="referraltype"></a>ReferralType

조회 유형을 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 속성              | 설명                                                                     |
|-----------------------|---------------------------------------------------------------------------------|
| 공유                | 관련된 모든 파티가 협력하여 닫는 조회를 나타냅니다.  |
| 독립           | 두 파티가 협력하여 닫는 조회를 나타냅니다.           |

## <a name="referralqualification"></a>ReferralQualification

조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 값                | 설명                                                                                 |
|----------------------|---------------------------------------------------------------------------------------------|
| 없음                 | 품질 측정 방법이 연결되지 않은 조회를 나타냅니다.                               |
| 직접               | 고객이 직접 만든 조회를 나타냅니다.                         |
| MarketingQualified   | Microsoft 마케팅 자동화 시스템을 통해 생성된 조회를 나타냅니다.   |
| SalesQualified       | Microsoft 영업 에이전트의 조회를 나타냅니다.                                         |

## <a name="customerprofile"></a>CustomerProfile

고객 연락처 정보를 포함합니다.

| 속성 | 형식                                                   | 설명                                            |
|----------|--------------------------------------------------------|--------------------------------------------------------|
| 이름     | string                                                 | 고객 조직 이름입니다.                        |
| Address  | [Address](referral-resources.md#address)                         | 고객의 주소입니다.                           |
| 크기     | string                                                 | 고객 조직의 직원 수입니다. |
| 팀     | [Member](referral-resources.md#member)                           | 고객 조직의 연락처입니다.            |
| Ids      | [CustomerProfileType](referral-resources.md#customerprofiletype) | 고객의 외부 ID를 나타내는 값의 [배열](https://docs.microsoft.com/dotnet/api/system.array)입니다.                        |

## <a name="customerprofiletype"></a>CustomerProfileType

고객의 외부 ID를 포함합니다.

| 속성 | 형식   | 설명                                                                           |
|----------|--------|---------------------------------------------------------------------------------------|
| Duns     | string | 고객의 [Dun & Bradstreet 번호](https://www.dnb.com/duns-number.html)입니다. |
| 외부 | string | 조직 고유의 고객 ID입니다.                                            |

## <a name="address"></a>Address

고객에 대해 사용할 주소입니다.

| 속성     | 형식   | 설명                                                |
|--------------|--------|------------------------------------------------------------|
| AddressLine1 | string | 주소의 첫 번째 줄입니다.                             |
| AddressLine2 | string | 주소의 두 번째 줄입니다. 이 속성은 선택 사항입니다. |
| 구/군/시         | string | 구/군/시입니다.                                                  |
| 상태        | string | 상태입니다.                                                 |
| PostalCode   | string | 우편 번호입니다.                                |
| Country      | string | [ISO 국가 코드 형식](https://docs.microsoft.com/dotnet/api/system.globalization.regioninfo.threeletterisoregionname?view=netframework-4.7.2)의 국가/지역입니다.             |
| Region       | string | 지역입니다.                                                |

## <a name="member"></a>멤버

특정 개인에 대한 연락처 정보를 설명합니다.

| 속성    | 형식   | 설명                  |
|-------------|--------|------------------------------|
| FirstName   | string | 연락처의 이름입니다.    |
| LastName    | string | 연락처의 성입니다.     |
| PhoneNumber | string | 연락처의 전화 번호입니다.  |
| 메일       | string | 연락처의 이메일 주소입니다. |
| ContactPreference       | [ContactPreference](referral-resources.md#contactpreference) | 이메일 알림을 받을 연락처의 기본 설정입니다. |

## <a name="contactpreference"></a>ContactPreference

이메일 알림을 받을 연락처의 기본 설정입니다.

| 속성    | 형식   | 설명                  |
|-------------|--------|------------------------------|
| 로캘   | string | 이메일 알림의 로캘입니다. [AllCultures](https://docs.microsoft.com/dotnet/api/system.globalization.culturetypes?view=netframework-4.7.2#System_Globalization_CultureTypes_AllCultures), [NeutralCultures](https://docs.microsoft.com/dotnet/api/system.globalization.culturetypes?view=netframework-4.7.2#System_Globalization_CultureTypes_NeutralCultures), [SpecificCultures](https://docs.microsoft.com/dotnet/api/system.globalization.culturetypes?view=netframework-4.7.2#System_Globalization_CultureTypes_SpecificCultures)가 지원됩니다.  |
| DisableNotifications    | bool | 사용자에 대한 이메일 알림을 비활성화합니다.     |

## <a name="consent"></a>Consent

Consent는 다른 조직과 정보를 공유하고 사용자에게 연락할 수 있도록 플래그를 지정합니다.

| 속성                                         | 형식      | 설명                                                                                |
|--------------------------------------------------|-----------|--------------------------------------------------------------------------------------------|
| ConsentToToShareInfoWithOthers                   | boolean   | PII(개인 식별 정보)를 다른 사람과 공유한다는 것을 나타냅니다.             |
| ConsentToContact                                 | boolean   | 사용자에게 연락을 동의한다는 것을 나타냅니다.  |

## <a name="invitecontext"></a>InviteContext

다른 조직을 초대할 때 공유할 수 있는 추가 정보입니다.

| 속성              | 형식                                                       | 설명                                                                   |
|-----------------------|------------------------------------------------------------|-------------------------------------------------------------------------------|
| 참고                 | string                                                     | 수신 조직에 대한 추가 참고 사항입니다.                |
| InvitedBy | [InvitedBy](referral-resources.md#invitedby)                                     | 조회를 보낸 조직 ID입니다.                                   |

## <a name="invitedby"></a>InvitedBy

다른 조직을 초대할 때 공유할 수 있는 추가 정보입니다.

| 속성              | 형식                                                       | 설명                                                                   |
|-----------------------|------------------------------------------------------------|-------------------------------------------------------------------------------|
| OrganizationId        | string                                                     | 조회를 보낸 조직 ID입니다.                |
| 조직 이름      | string                                                     | 조회를 보낸 조직 이름입니다.                                   |

## <a name="referraldetails"></a>ReferralDetails

조회 세부 정보를 나타냅니다.

| 속성              | 형식                                                       | 설명                                                                   |
|-----------------------|------------------------------------------------------------|-------------------------------------------------------------------------------|
| 참고                 | string                                                     | 수신 조직에 대한 추가 참고 사항입니다.                |
| DealValue             | decimal                                                    | 조회의 값입니다.                                    |
| Currency              | string                                                    | [ISO 4217 통화 기호](https://docs.microsoft.com/dotnet/api/system.globalization.regioninfo.isocurrencysymbol?view=netframework-4.7.2)                                   |
| ClosingDateTime       | UTC 날짜/시간 형식의 문자열                         | 고객이 마감하려는 날짜입니다.                           |
| 요구 사항          | [ReferralRequirements](referral-resources.md#referralrequirements)   | 고객이 관심을 가지고 있는 산업, 제품, 서비스 유형 및 솔루션입니다.|

## <a name="referralrequirements"></a>ReferralRequirements

고객 요구 사항을 포함합니다.

| 속성        | 형식                                                         | 설명                                          |
|-----------------|--------------------------------------------------------------|------------------------------------------------------|
| 산업      | [Tag](referral-resources.md#tag)                                       | 고객이 관심이 갖는 산업입니다.        |
| 제품        | [Tag](referral-resources.md#tag)                                       | 고객이 관심이 갖는 제품입니다.          |
| 서비스        | [Tag](referral-resources.md#tag)                                       | 고객이 관심이 갖는 서비스입니다.          |
| 솔루션       | [SolutionTag](referral-resources.md#solutiontag)                       | 고객이 관심이 갖는 솔루션입니다.                             |

## <a name="solutiontag"></a>SolutionTag

솔루션 세부 정보를 포함합니다.

| 속성        | 형식                                         | 설명                                          |
|-----------------|----------------------------------------------|------------------------------------------------------|
| Id              | string                                       | 솔루션의 ID입니다.        |
| 이름            | string                                       | 솔루션의 이름입니다.          |
| SolutionType    | [SolutionType](referral-resources.md#solutiontype)     | 솔루션의 유형입니다.          |

## <a name="solutiontype"></a>SolutionType

솔루션 유형을 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 속성        | 설명                                                     |
|-----------------|-----------------------------------------------------------------|
| 없음            |                                                                  |
| 범주        |  미리 정의된 솔루션 이름을 활용합니다.                            |
| 이름            |  Microsoft 카탈로그에서 솔루션을 참조할 수 있습니다. |

## <a name="target"></a>대상

조회 대상을 설명합니다.

| 속성                  | 형식                                                  | 설명                                                   |
|---------------------------|-------------------------------------------------------|---------------------------------------------------------------|
| Id                        | string                                                | 조회 대상의 ID입니다. |
| 형식                      | [ReferralTargetType](referral-resources.md#targettype) | 조회 대상 유형 |

## <a name="targettype"></a>TargetType

솔루션 유형을 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.

| 속성        | 설명                                                     |
|-----------------|-----------------------------------------------------------------|
| 없음            |                                                                  |
| BusinessProfileLocation         |  파트너 비즈니스 프로필의 프로필 위치입니다.                            |
| SolutionProfile            |  파트너의 솔루션 프로필입니다. |

## <a name="tag"></a>Tag

태그를 설명합니다.

| 속성                  | 형식                                                  | 설명                                                   |
|---------------------------|-------------------------------------------------------|---------------------------------------------------------------|
| Id                        | string                                                | 이 태그의 ID입니다.                                          |

### <a name="products"></a>제품

| 값        |
|-----------------|
|Azure|
|EnterpriseMobilityAndSecurity|
|Exchange|
|DeveloperTools|
|Dynamics365Business|
|Dynamics365Enterprise|
|DynamicsAX,GP,NAV,SL|
|Microsoft365|
|Office|
|PowerBI|
|프로젝트|
|SharePoint|
|SkypeForBusiness|
|Surface|
|SurfaceHub|
|SQL|
|팀|
|Visio|
|Windows|
|Yammer|

### <a name="services"></a>서비스

| 값        |
|-----------------|
|ConsultingAndProfessional|
|CustomSolution(ISV)|
|DeploymentOrMigration|
|하드웨어|
|Integration|
|IPServices(ISV)|
|LearningAndCertification|
|라이선싱|
|ManagedServices|
|ProjectServices|

### <a name="industries"></a>산업

| 값        |
|-----------------|
|농업, 임업 및 어업|
|통신 및 미디어|
|교육|
|금융 서비스|
|정부|
|의료|
|숙박|
|제조업|
|전기 및 공공 시설|
|공공 안전 및 국가 안보|
|소매 및 소비재|
|서비스|
|여행 및 교통|
|도매업 및 유통|

### <a name="solutions"></a>솔루션

| 값        |
|-----------------|
|AdvancedAnalytics|
|ApplicationIntegration|
|ArtificialIntelligence|
|AzureSecurityOperationManagement|
    |AzureStack|
    |BackupDisasterRecovery|
    |BigData|
    |Blockchain|
    |Chatbot|
    |CloudDatabaseMigration|
    |CloudMigration|
    |CloudVoice|
    |CognitiveServices|
    |CompetitiveDatabaseMigration|
    |컨테이너|
    |DataWarehouse|
    |DatabaseonLinux|
    |DevelopmentandTest|
    |DevOps|
    |DigitalMedia|
    |Dynamics365forCustomerService|
    |Dynamics365forFieldService|
    |Dynamics365forFinanceOperations|
    |Dynamics365forRetail|
    |Dynamics365forSales|
    |Dynamics365forTalent|
    |DynamicsonAzure|
    |EnterpriseBusinessIntelligence|
    |게임|
    |HighPerformanceComputing|
    |HybridStorage|
    |IdentityandAccessManagement|
    |InformationManagement|
    |InternetofThings|
    |MachineLearning|
    |Microserviceapplications|
    |MobileApplications|
    |MySQLPostgresMigrationtoAzure|
    |네트워킹|
    |NoSQLMigration|
    |RedhatonAzure|
    |RegulatoryComplianceGDPR|
    |SAPonAzure|
    |ServerlessComputing|
    |SharepointonAzure|
    |SQLServerUpgrade|
    |ThreatProtection|
    |WebDevelopment|

### <a name="customer-size"></a>고객 규모

| 값        |
|-----------------|
|    1to50employees|
|    51to500employees|
|    Morethan500employees|
|    1to9employees|
|    10to50employees|
|    51to250employees|
|    251to1000employees|
|    1001to5000employees|
|    5001to10000employees|
|    10001to20000employees|
|    Morethan20000employees|
