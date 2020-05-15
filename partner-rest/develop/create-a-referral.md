---
title: 조회 만들기
description: 파트너 API에서 독립 또는 공유 조회를 만듭니다.
ms.date: 05/17/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: 6aab4b5f45030c3c16294b2929b1a6d3086fb951
ms.sourcegitcommit: 3c165938f544ff226cbe11ca21ed5aa00448d9b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80342292"
---
# <a name="create-a-referral"></a>조회 만들기

적용 대상:

- 파트너 API

이 항목에서는 조회를 만드는 방법을 설명합니다. [ReferralType](referral-resources.md#referraltype)에는 두 가지 유형이 있습니다.

1. 독립: 이 경우, 한 파트너에 조회가 표시됩니다.
2. 공유: 이 경우, 함께 작업하는 두 파티에 조회가 표시됩니다. 예를 들어 Microsoft와 파트너가 공동 판매 거래에서 협력하는 경우, 두 파티 사이에 조회를 공유 할 수 있습니다. 자세한 내용은 [공유 조회 만들기](#create-a-shared-referral) 섹션을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오는 앱+사용자 자격 증명을 통한 인증을 지원합니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 방법  | 요청 URI                                                  |
|---------|--------------------------------------------------------------|
| **POST** | <https://api.partner.microsoft.com/v1.0/engagements/referrals> |

### <a name="request-headers"></a>요청 헤더

- 자세한 내용은 [파트너 API REST 헤더](headers.md)를 참조하세요.

### <a name="request-body"></a>요청 본문

다음 표에서는 새로운 참조에 대한 요청 본문의 [조회](referral-resources.md) 속성을 설명합니다.

| 속성            | 유형                                                                 | 설명                                                                                                          |
|---------------------|----------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 이름                | string                                                               | 조회의 이름입니다.                                                                                            |
| ExternalReferenceId | string                                                               | 조회에 대한 외부 식별자입니다. 예를 들어, 사용자의 Dynamics 365 잠재 고객 또는 기회 ID입니다.                   |
| 상태              | [ReferralStatus](referral-resources.md#referralstatus)               | 조회 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.          |
| Substatus           | [ReferralSubstatus](referral-resources.md#referralsubstatus)         | 조회 하위 상태를 나타내는 값이 있는 [열거형](https://docs.microsoft.com/dotnet/api/system.enum)입니다.       |
| StatusReason        | string                                                               | 상태에 대한 설명 메시지입니다. 예를 들어 조회가 실패한 이유를 설명합니다.                            |
| ReferralType        | [ReferralType](referral-resources.md#referraltype)                   | 조회 유형을 나타냅니다. **필수 사항입니다.**                                                                                        |
| Qualification       | [ReferralQualification](referral-resources.md#referralqualification) | 조회 품질을 나타냅니다.                                                                              |
| CustomerProfile     | [CustomerProfile](referral-resources.md#customerprofile)             | 고객 연락처 정보입니다.  **필수 사항입니다.**                                                                                      |
| Consent             | [Consent](referral-resources.md#consent)                             | Consent는 다른 조직과 정보를 공유하고 사용자에게 연락할 수 있도록 플래그를 지정합니다. **필수 사항입니다.**               |
| 세부 정보             | [ReferralDetails](referral-resources.md#referraldetails)             | 고객 세부 정보, 메모, 거래 가격, 통화 마감일입니다. **필수 사항입니다.**                                                           |
| 팀                | [Member](referral-resources.md#member)                               | 파트너 참여와 관련된 조직의 사용자를 나타냅니다.                                   |
| InviteContext       | [InviteContext](referral-resources.md#invitecontext)                 | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다. |
| 대상         | [ReferralTarget](referral-resources.md#target)        | 다른 조직을 파트너 참여에 초대할 때 사용자가 제공할 수 있는 추가 정보를 나타냅니다.  |

#### <a name="status--substatus-transition-states"></a>상태 및 하위 상태 전환 상태

| 상태 | 허용되는 상태 전환 | 허용되는 하위 상태            |
|--------|---------------------------|------------------------------|
| 새로 만들기    | 새, 활성, 닫힘       | 보류 중, 받음            |
| 활성 | 활성, 닫힘            | Accepted                     |
| 종결 | 종결                    | 성공, 실패, 거부됨, 만료됨 |

### <a name="request-example"></a>요청 예제

```http
POST https://api.partner.microsoft.com/v1.0/engagements/referrals HTTP/1.1
Authorization: Bearer <token>
Host: api.partner.microsoft.com
Content-Type: application/json

 {
    "name": "Test Cosell Invite_20",
    "status": "New",
    "substatus": "Pending",
    "statusReason": "Customer engagement was a success!",
    "qualification": "SalesQualified",
    "type": "Shared",
    "target": [
        {
            "type": "SolutionProfile",
            "id": "SOL-34104-EBB"
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
    }
}
```

## <a name="rest-response"></a>REST 응답

성공하는 경우, 이 메서드는 응답 본문에 채워진 [조회](referral-resources.md) 리소스를 반환합니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 HTTP 상태 코드가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 이 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다. 전체 목록은 [오류 코드](error-codes.md)를 참조하세요.

### <a name="response-example"></a>응답 예제

``` http
{
    "id": "4111fffc-f9ee-4d53-bba6-569135228642",
    "engagementId": "37ef26aa-1d15-4533-9f93-a69bd33ab1e5",
    "organizationId": "7d23e5ca-19dc-4eaa-aac8-5e6b559f0d1d",
    "organizationName": "Contoso Company",
    "name": "Test Cosell Invite_20",
    "externalReferenceId": null,
    "createdDateTime": "2019-02-23T02:05:23.2931817Z",
    "updatedDateTime": "2019-02-23T02:05:23.2931817Z",
    "expirationDateTime": null,
    "status": "Active",
    "substatus": "Accepted",
    "statusReason": "Customer engagement was a success!",
    "qualification": "SalesQualified",
    "type": "Shared",
    "eTag": "\"00006d10-0000-0000-0000-5c70aa630000\"",
    "target": [
        {
            "type": "SolutionProfile",
            "id": "SOL-34104-EBB"
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

## <a name="create-a-shared-referral"></a>공유 조회 만들기

**공유** [조회 유형](referral-resources.md#referraltype)의 조회를 만드는 단계는 두 가지입니다.

1. [공유 조회 만들기](#create-your-referral)
2. [두 번째 파티의 연결된 조회 만들기](#create-a-connected-referral)

다음 순서도는 공유 조회를 만드는 두 가지 단계를 보여줍니다.

![Microsoft 파트너 API를 통해 연결된 2개의 조회가 있는 공유 조회를 보여주는 순서도](../images/SharedReferral.png)

### <a name="create-your-referral"></a>조회 만들기

1. [ReferralType](referral-resources.md#referraltype)을 공유로 설정하여 조회를 만듭니다.
2. 반환 응답에서 **engagementId**를 복사합니다.

조회의 [ReferralTarget](referral-resources.md#target) 샘플

```json
"target": [
        {
            "type": "SolutionProfile",
            "id": "SOL-ABC-DEF"
        }
    ]
```

### <a name="create-a-connected-referral"></a>연결 된 조회 만들기

1. Microsoft에 대한 다른 조회를 만듭니다.
2. 서로 연결될 수 있도록 조회의 **enagementId**를 포함합니다.

Microsoft 조회의 [ReferralTarget](referral-resources.md#target) 샘플

```json
"target": [
        {
            "type": "BusinessProfileLocation",
            "id": "msft"
        }
    ]
```