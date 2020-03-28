---
title: ID로 조회 가져오기
description: 해당 ID별 조회를 가져옵니다.
ms.date: 05/21/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: 39ee62de740f3aa73c3fe8ca86d0da47ebb96334
ms.sourcegitcommit: 0508b7302a3965fd5537b05c1f0397a1da014257
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342263"
---
# <a name="get-a-referral-by-id"></a>ID로 조회 가져오기

적용 대상:

- 파트너 API

이 항목에서는 ID를 기준으로 조회를 가져오는 방법을 설명합니다.

## <a name="prerequisites"></a>필수 조건

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오는 앱+사용자 자격 증명을 통한 인증을 지원합니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 메서드   | 요청 URI                                                                                                 |
|----------|-------------------------------------------------------------------------------------------------------------|
| **GET** | <https://api.partner.microsoft.com/v1.0/engagements/referrals/{Id}>                                     |

### <a name="uri-parameter"></a>URI 매개 변수

URL에 다음 ID를 사용합니다.

| 이름                   | 형식     | 필수 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|Id                      | string   | 예       | 조회 ID       |

### <a name="request-headers"></a>요청 헤더입니다.

- 자세한 내용은 [파트너 REST 헤더](headers.md)를 참조하세요.

### <a name="request-body"></a>요청 본문

다음 표에서는 요청 본문의 [조회](referral-resources.md) 속성을 설명합니다.

### <a name="request-example"></a>요청 예제

```http
GET https://api.partner.microsoft.com/v1.0/engagements/referrals/0d43414c-fb9f-4ca0-9b8d-29deb70364cf HTTP/1.1
Authorization: Bearer <token>
Host: api.partner.microsoft.com
Content-Type: application/json

```

## <a name="rest-response"></a>REST 응답

성공하는 경우, 이 메서드는 응답 본문에 채워진 [조회](referral-resources.md) 리소스를 반환합니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 HTTP 상태 코드가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 이 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다. 전체 목록은 [오류 코드](error-codes.md)를 참조하세요.

### <a name="response-example"></a>응답 예제

``` http
{
    "@odata.context": "https://api.partner.microsoft.com/v1.0/engagments/referrals/$metadata#Referrals/$entity",
    "id": "61c65491-2f2c-461a-84b4-3654499bc1d9",
    "engagementId": "b1c40bb4-6d36-4eca-baa3-e1460cf2a454",
    "organizationId": "7d23e5ca-19dc-4eaa-aac8-5e6b559f0d1d",
    "organizationName": "Contoso Company",
    "createdDateTime": "2018-10-29T21:24:52.040469Z",
    "updatedDateTime": "2018-10-29T21:24:52.040469Z",
    "expirationDateTime": "2018-11-06T00:00:00Z",
    "status": "New",
    "substatus": "Pending",
    "statusReason": null,
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
            "region": "Washington"
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
        "ids": [
                {
                "profileType": "Duns",
                "id": "795986822"
                }
            ]
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
        "notes": "Hi ABC Partner, Can you help this customer? Thanks, John @ Microsoft",
        "invitedBy": {
            "organizationId": "msft"
        }
    },
    "links": {
        "relatedReferrals": {
            "uri": "https://api.partner.microsoft.com/v1.0/engagments/referrals?$filter=engagementId eq 'b1c40bb4-6d36-4eca-baa3-e1460cf2a454'",
            "method": "GET"
        },
        "self": {
            "uri": "https://api.partner.microsoft.com/v1.0/engagments/referrals/61c65491-2f2c-461a-84b4-3654499bc1d9",
            "method": "GET"
        }
    },
    "eTag": "\"2500ec5a-0000-0000-0000-5bf4967d0000\""
}
```