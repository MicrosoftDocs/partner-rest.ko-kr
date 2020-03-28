---
title: 조회 업데이트
description: 파트너 API를 사용하여 조회를 업데이트합니다.
ms.date: 05/17/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: 9b77028541e58006f98a25800aecf21edc93fac8
ms.sourcegitcommit: 0508b7302a3965fd5537b05c1f0397a1da014257
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342073"
---
# <a name="update-a-referral"></a>조회 업데이트

적용 대상:

- 파트너 API

이 항목에서는 조회를 업데이트하는 방법을 설명합니다.

## <a name="prerequisites"></a>필수 조건

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오는 앱+사용자 자격 증명을 통한 인증을 지원합니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 메서드  | 요청 URI                                                       |
|---------|-------------------------------------------------------------------|
| **PUT** | <https://api.partner.microsoft.com/v1.0/engagements/referrals/{Id}> |

### <a name="request-headers"></a>요청 헤더입니다.

- 자세한 내용은 [파트너 API REST 헤더](headers.md)를 참조하세요.

> [!IMPORTANT]
> **If-Match** 헤더를 설정해야 합니다.

### <a name="request-body"></a>요청 본문

다음 표에서는 요청 본문의 [조회](referral-resources.md) 속성을 설명합니다.

| 속성            | 형식                                                                 | 설명                                                                                                          |
|---------------------|----------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Id                  | string                                                               | 이 조회의 ID입니다.                                                                                            |
| EngagementId        | string                                                               | 이 조회의 EngagementID입니다. 단일 EngagementID에 여러 조회를 연결할 수 있습니다.                    |
| 이름                | string                                                               | 조회의 이름입니다.                                                                                            |
| ExternalReferenceId | string                                                               | 조회에 대한 외부 식별자입니다. 예: 고유한 Dynamics 365 리드/기회 ID 저장                    |
| CreatedDateTime     | UTC 날짜/시간 형식의 문자열                                       | 조회가 만들어진 날짜입니다.                                                                                   |
| UpdatedDateTime     | UTC 날짜/시간 형식의 문자열                                       | 조회가 마지막으로 업데이트된 날짜입니다.                                                                              |
| ExpirationDateTime  | UTC 날짜/시간 형식의 문자열                                       | 조회가 만료되는 날짜입니다.                                                                                   |
| 상태              | [ReferralStatus](referral-resources.md#referralstatus)               | 조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.          |
| Substatus           | [ReferralSubstatus](referral-resources.md#referralsubstatus)         | 조회 하위 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.      |
| StatusReason        | string                                                               | 상태에 대한 설명 메시지입니다. 예를 들어 조회가 실패한 이유를 설명합니다.                              |
| ReferralType        | [ReferralType](referral-resources.md#referraltype)                   | 조회 유형을 나타냅니다.                                                                                        |
| Qualification       | [ReferralQualification](referral-resources.md#referralqualification) | 조회 품질을 나타냅니다.                                                                              |
| CustomerProfile     | [CustomerProfile](referral-resources.md#customerprofile)             | 고객 연락처 정보입니다.                                                                                        |
| Consent             | [Consent](referral-resources.md#consent)                             | Consent는 다른 조직과 정보를 공유하고 사용자에게 연락할 수 있도록 플래그를 지정합니다.                |
| 세부 정보             | [ReferralDetails](referral-resources.md#referraldetails)             | 고객 세부 정보, 메모, 거래 가격, 통화 마감일입니다.                                                          |
| 팀                | [Member](referral-resources.md#member)                               | 파트너 참여와 관련된 조직의 사용자를 나타냅니다.                                   |
| InviteContext       | [InviteContext](referral-resources.md#invitecontext)                 | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다. |
| 대상         | [ReferralTarget](referral-resources.md#target)        | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다.  |

### <a name="status-and-substatus-transition-states"></a>상태 및 하위 상태 전환 상태

| 상태 | 허용되는 상태 전환 | 허용되는 하위 상태            |
|--------|---------------------------|------------------------------|
| 새로 만들기    | 새, 활성, 닫힘       | 보류 중, 받음            |
| 활성 | 활성, 닫힘            | 수락됨                     |
| 종결 | 종결                    | 성공, 실패, 거부됨, 만료됨 |

### <a name="request-example"></a>요청 예제

```http
PUT https://api.partner.microsoft.com/v1.0/engagements/referrals/49d90c72-3326-4f61-aacc-2cb57970448c HTTP/1.1
Authorization: Bearer <token>
Host: api.partner.microsoft.com
Content-Type: application/json

 {
    "id": "49d90c72-3326-4f61-aacc-2cb57970448c",
    "engagementId": "37ef26aa-1d15-4533-9f93-a69bd33ab1e5",
    "createdDateTime": "2018-11-06T18:40:42.6178337Z",
    "updatedDateTime": "2018-11-06T18:40:42.6178337Z",
    "expirationDateTime": "2018-11-14T00:00:00Z",
    "status": "Closed",
    "substatus": "Won",
    "statusReason": "Customer engagement was a success!",
    "qualification": "SalesQualified",
    "type": "Independent",
    "target": [
        {
            "type": "BusinessProfileLocation",
            "id": "01e2abcd-52b0-4af3-a3ae-1fd1530b3563"
        }
    ],
    "customerProfile": {
        "name": "Contoso Customer Inc",
        "address": {
            "addressLine1": "One Microsoft Way",
            "addressLine2": "34",
            "city": "Redmond",
            "state": "WA",
            "postalCode": "98052",
            "country": "US"
        },
        "size": "10to50employees",
        "team": [
            {
                "contactPreference": {
                    "locale": "en-us",
                    "disableNotifications": false
                },
                "firstName": "Sue",
                "lastName": "Smith",
                "phoneNumber": "1234567890",
                "email": "sue.smith@contoso.com"
            },
            {
                "contactPreference": {
                    "locale": "en-us",
                    "disableNotifications": false
                },
                "firstName": "Joe",
                "lastName": "Hansen",
                "phoneNumber": "4035698759",
                "email": "joe.hansen@contoso.com"
            }
        ],
        "ids": []
    },
    "consent": {
        "consentToToShareInfoWithOthers": true,
        "consentToContact": true
    },
    "details": {
        "notes": "Customer is looking to leverage Dynamics 365 to manage their supply chain. There is also a need to leverage a set of custom apps to enable their business processes.",
        "dealValue": 50000,
        "currency": "USD",
        "closingDateTime": "2018-11-14T00:00:00Z",
        "requirements": {
            "industries": [
                {
                    "id": "Manufacturing"
                }
            ],
            "products": [
                {
                    "id": "Dynamics365Enterprise"
                }
            ],
            "services": [
                {
                    "id": "DeploymentOrMigration"
                }
            ],
            "solutions": [
                {
                    "name": "Dynamics 365 for Field Service",
                    "type": "Category",
                    "id": "Dynamics365forFieldService"
                }
            ]
        }
    },
    "team": [
        {
            "contactPreference": {
                "locale": "en-us",
                "disableNotifications": false
            },
            "firstName": "John",
            "lastName": "Doe",
            "phoneNumber": "1231231234",
            "email": "john.doe@microsoft.com"
        }
    ],
    "inviteContext": {
        "notes": "Hi ABC Partner, hoping you can help this customer. Thanks, John @ Microsoft",
        "invitedBy": {
            "organizationId": "msft"
        }
    },
    "eTag": "\"2500ec5a-0000-0000-0000-5bf4967d0000\"",

}
```

> [!IMPORTANT]
> PUT 리소스에서 `"links": { }` 개체를 제거합니다.

## <a name="rest-response"></a>REST 응답

성공하는 경우, 이 메서드는 응답 본문에 채워진 [조회](referral-resources.md) 리소스를 반환합니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 HTTP 상태 코드가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 이 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다. 전체 목록은 [오류 코드](error-codes.md)를 참조하세요.

### <a name="response-example"></a>응답 예제

``` http
{
    "id": "4111fffc-f9ee-4d53-bba6-569135228642",
    "organizationId": "7d23e5ca-19dc-4eaa-aac8-5e6b559f0d1d",
    "organizationName": "Contoso Company",
    "engagementId": "37ef26aa-1d15-4533-9f93-a69bd33ab1e5",
    "createdDateTime": "2018-11-06T18:40:42.6178337Z",
    "updatedDateTime": "2018-11-06T18:43:38.9948636Z",
    "expirationDateTime": "2018-11-14T00:00:00Z",
    "status": "Closed",
    "substatus": "Won",
    "statusReason": "Customer engagement was a success!",
    "qualification": "SalesQualified",
    "type": "Independent",
    "target": [
        {
            "type": "BusinessProfileLocation",
            "id": "01e2abcd-52b0-4af3-a3ae-1fd1530b3563"
        }
    ],
    "customerProfile": {
        "name": "Contoso Customer Inc",
        "address": {
            "addressLine1": "One Microsoft Way",
            "addressLine2": "34",
            "city": "Redmond",
            "state": "WA",
            "postalCode": "98052",
            "country": "US"
        },
        "size": "10to50employees",
        "team": [
            {
                "contactPreference": {
                    "locale": "en-us",
                    "disableNotifications": false
                },
                "firstName": "Sue",
                "lastName": "Smith",
                "phoneNumber": "1234567890",
                "email": "sue.smith@contoso.com"
            },
            {
                "contactPreference": {
                    "locale": "en-us",
                    "disableNotifications": false
                },
                "firstName": "Joe",
                "lastName": "Hansen",
                "phoneNumber": "4035698759",
                "email": "joe.hansen@contoso.com"
            }
        ],
        "ids": []
    },
    "consent": {
        "consentToToShareInfoWithOthers": true,
        "consentToContact": true
    },
    "details": {
        "notes": "Customer is looking to leverage Dynamics 365 to manage their supply chain. There is also a need to leverage a set of custom apps to enable their business processes.",
        "dealValue": 50000,
        "currency": "USD",
        "requirements": {
            "industries": [
                {
                    "id": "Manufacturing"
                }
            ],
            "products": [
                {
                    "id": "Dynamics365Enterprise"
                }
            ],
            "services": [
                {
                    "id": "DeploymentOrMigration"
                }
            ],
            "solutions": [
                {
                    "name": "Dynamics 365 for Field Service",
                    "type": "Category",
                    "id": "Dynamics365forFieldService"
                }
            ]
        }
    },
    "team": [
        {
            "contactPreference": {
                "locale": "en-us",
                "disableNotifications": false
            },
            "firstName": "John",
            "lastName": "Doe",
            "phoneNumber": "1231231234",
            "email": "john.doe@microsoft.com"
        }
    ],
    "inviteContext": {
        "notes": "Hi ABC Partner, hoping you can help this customer. Thanks, John @ Microsoft",
        "invitedBy": {
            "organizationId": "msft"
        }
    },
    "eTag": "\"2500ec5a-0000-0000-0000-5bf4967d0000\"",
    "links": {
        "relatedReferrals": {
            "uri": "https://api.partner.microsoft.com/v1.0/engagments/referrals?$filter=engagementId eq '37ef26aa-1d15-4533-9f93-a69bd33ab1e5'",
            "method": "GET"
        },
        "self": {
            "uri": "https://api.partner.microsoft.com/v1.0/engagments/referrals/4111fffc-f9ee-4d53-bba6-569135228642",
            "method": "GET"
        }
    }
}
```