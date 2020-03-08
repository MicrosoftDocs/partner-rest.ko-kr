---
title: 조회 목록 가져오기
description: 파트너 API를 사용하여 조회 목록을 가져오는 방법입니다.
ms.date: 05/21/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-csp
ms.localizationpriority: medium
ms.openlocfilehash: 7daa7a1989a5832f58d555b1f8bb3d7681024dba
ms.sourcegitcommit: 50d18c96d24755174beb4fcb694223325a7fe450
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "74556533"
---
# <a name="get-a-list-of-referrals"></a>조회 목록 가져오기

적용 대상:

- 파트너 API

이 항목에서는 조회 목록을 가져오는 방법을 설명합니다.

## <a name="prerequisites"></a>필수 조건

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오는 앱+사용자 자격 증명을 통한 인증을 지원합니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 메서드  | 요청 URI                                                  |
|---------|--------------------------------------------------------------|
| **GET** | <https://api.partner.microsoft.com/v1.0/engagements/referrals> |

#### <a name="supported-odata-operations"></a>지원되는 OData 작업

| 이름     | 설명            | 예제                                                                    |
|:---------|:-----------------------|:---------------------------------------------------------------------------|
| $filter  | 결과 필터링(행) |`/referrals?$filter=engagementId eq '65edc0b5-3485-41b7-a17e-dfa9ef4706e2'` |
| $orderby | 결과 정렬         |`/referrals?$orderby=createdDateTime desc`                                  |

#### <a name="supported-filter-parameters"></a>지원되는 필터 매개 변수

다음 필터 매개 변수를 사용하여 조회 목록을 가져옵니다.

| 이름         | 형식   | 필수 | 설명                                                                             |
|--------------|--------|----------|-----------------------------------------------------------------------------------------|
| engagementId | string | 아니요       | Engagement ID                                                                       |
| 상태       | string | 아니요       | [ReferralStatus](referral-resources.md#referralstatus)를 나타내는 문자열       |
| substatus    | string | 아니요       | [ReferralSubstatus](referral-resources.md#referralsubstatus)를 나타내는 문자열 |
| updatedDateTime     | string | 아니요       | 조회의 UpdatedDatetime |
| 전자 메일     | string | 아니요       | 조회의 팀 연락처 이메일 |

#### <a name="supported-orderby-parameters"></a>지원되는 orderby 매개 변수

다음 orderby 매개 변수를 사용하여 조회 목록을 가져옵니다.

| 이름           | 형식     | 필수 | 설명                        |
|----------------|----------|----------|------------------------------------|
|createdDateTime | DateTime | 예      | 조회를 만든 날짜와 시간 |

### <a name="request-headers"></a>요청 헤더입니다.

- 자세한 내용은 [파트너 REST 헤더](headers.md)를 참조하세요.

### <a name="request-body"></a>요청 본문

None.

### <a name="request-example"></a>요청 예제

```http
GET https://api.partner.microsoft.com/v1.0/engagements/referrals HTTP/1.1
Authorization: Bearer <token>
Host: api.partner.microsoft.com
Content-Type: application/json

```

## <a name="rest-response"></a>REST 응답

요청이 성공적이면 응답 본문에 [조회 리소스](referral-resources.md) 컬렉션이 포함됩니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 [HTTP 상태 코드](error-codes.md)가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다.

### <a name="response-example"></a>응답 예제

``` http
{
    "@odata.context": "http://api.partner.microsoft.com/v1.0/$metadata#Referrals",
    "@odata.count": 100,
    "value": [
        {
            "id": "c5fbb3b6-be74-4795-9fb5-4324c73fed37",  
            "organizationId": "7d23e5ca-19dc-4eaa-aac8-5e6b559f0d1d",
            "organizationName": "Contoso Company",
            "engagementId": "65edc0b5-3485-41b7-a17e-dfa9ef4706e2",
            "externalReferenceId": "",
            "createdDateTime": "2018-10-30T21:03:42.4579542Z",
            "updatedDateTime": "2018-10-30T21:03:42.4579542Z",
            "expirationDateTime": "2018-11-13T00:00:00Z",
            "status": "New",
            "substatus": "Pending",
            "qualification": "Direct",
            "type": "Independent",
            "target": [
                    {
                        "type": "BusinessProfileLocation",
                        "id": "01e2abcd-52b0-4af3-a3ae-1fd1530b3563"
                    }
            ],
            "customerProfile": {
                "name": "Fabrikam Customer Inc",
                "address": {
                    "addressLine1": "One Microsoft Way",
                    "addressLine2": "",
                    "city": "Redmond",
                    "state": "WA",
                    "postalCode": "98052",
                    "country": "US"
                },
                "size": "1to9employees",
                "team": [
                    {
                        "contactPreference": {
                            "locale": "en-US",
                            "disableNotifications": false
                        },
                        "firstName": "Sue",
                        "lastName": "Smith",
                        "phoneNumber": "1234567890",
                        "email": "sue.smith@fabrikam.com"
                    }
                ],
                "ids": {}
            },
            "consent": {
                "consentToContact": true
            },
            "details": {
                "notes": "We are interested in deploying Microsoft 365 and are looking for support in training our employees. Can you help?",
                "requirements": {
                    "industries": [
                        {
                            "id": "Education"
                        }
                    ],
                    "products": [
                        {
                            "id": "Microsoft365"
                        }
                    ],
                    "services": [
                        {
                            "id": "LearningAndCertification"
                        }
                    ]
                }
            }
            "links": {
                "relatedReferrals":
                    {
                       "uri": "https://api.partner.microsoft.com/v1.0/engagements/referrals?$filter=engagementId eq '65edc0b5-3485-41b7-a17e-dfa9ef4706e2'",
                        "method": "GET"
                    },
                "self":
                    {
                       "uri": "https://api.partner.microsoft.com/v1.0/engagements/referrals/c5fbb3b6-be74-4795-9fb5-4324c73fed37",
                        "method": "GET"
                    }
            },
        },
        {
            "id": "61c65491-2f2c-461a-84b4-3654499bc1d9",
            "organizationId": "7d23e5ca-19dc-4eaa-aac8-5e6b559f0d1d",
            "organizationName": "Contoso Company",
            "engagementId": "b1c40bb4-6d36-4eca-baa3-e1460cf2a454",
            "createdDateTime": "2018-10-29T21:24:52.040469Z",
            "updatedDateTime": "2018-10-29T21:24:52.040469Z",
            "expirationDateTime": "2018-11-06T00:00:00Z",
            "status": "New",
            "substatus": "Pending",
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
                            "locale": "en-US",
                            "disableNotifications": false
                        },
                        "firstName": "Sue",
                        "lastName": "Smith",
                        "phoneNumber": "1234567890",
                        "email": "sue.smith@contoso.com"
                    },
                    {
                        "contactPreference": {
                            "locale": "en-US",
                            "disableNotifications": false
                        },
                        "firstName": "Joe",
                        "lastName": "Hansen",
                        "phoneNumber": "4035698759",
                        "email": "joe.hansen@contoso.com"
                    }
                ],
                "ids": {}
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
                        "locale": "en-US",
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
                    "organizationId": "msft",
                    "organizationName": "Microsoft"
                }
            },
            "links": {
                "relatedReferrals":
                    {
                       "uri": "https://api.partner.microsoft.com/v1.0/engagements/referrals?$filter=engagementId eq 'b1c40bb4-6d36-4eca-baa3-e1460cf2a454'",
                        "method": "GET"
                    },
                "self":
                    {
                       "uri": "https://api.partner.microsoft.com/v1.0/engagements/referrals/61c65491-2f2c-461a-84b4-3654499bc1d9",
                        "method": "GET"
                    }
            }
        }
    ],
  
 "@odata.nextLink": "http://api.partner.microsoft.com/v1.0/referrals?$skiptoken=k181pEdP0ykypkieJfcxXIN4ARsee8%2fFUEbbvZaoTUZRLJ3o7TADYve12iEDrlgVevJO5J0ftNkTY5FZepU1sh1m8fRNuUD4YPGazoaywpAqoRAd43JuVJV8fwmcJFTFAH4Wg2%2fQFWkxT6PWujvkj%2biqLMDsU8CI6x3k0CHZWhLiw4inmd3Ib8wVoA4wygjONBhcEegyySiZa85H59pWHQoNFA%2fYP6tyw3%2f0CgcSwa%2fh%2bDf7Ygm0MaOhBZbYmkhRITbgRT1LWDLBGpNzLJN%2fB%2basQXEojDL7tZRLB%2bGlfPJ%2fojPBTctFqEp13TVZta2jdJmC917IhR5fGt91%2blwuxd83Y5PwCjXpg%2fYn8JKna10%2bVe4Devy21pryas%2bu3oGKmbqaXKMBIUCQU8D7cFy59R90Tzcogte05QSSiN6U1KP%2bY1oPchrKmvqt8S6jNoy%2bTaR3Bc9AtzVNpuaEGGkfO9g3SFlLuBv%2bd4aG50iFjmOKCAPIRr76JVUzYsJ76oyUv0AOZ%2bW5lqgtyQQwN%2b%2baEkF3HDebb6%2byTqdagjI86faTAi4YOOn0oVmmGIWZJnp5MuG38P8VHqK4ZdZ5OpKkpMrigLqYrU%2fFliGbBgnTP4Y7Pp%2fai9JzxYF1%2fajIYsE0OdyojAG2GtzNzlyHj1g9e9Tgz2%2fy5BfHrk4asYRcSvQfN%2bdaO1NVWCraMvhxjsr5Twcg9z%2bAETNVqXZMp5qKBsPGxPFcnWGx6OY296kmIzuTnDd2igIoye8wj57xO4WEBIW3%2bFIHrBlzh3bV0ljNHBb%2f5eDF68GK6AMmwusqjleiW3iHmZRPeTiMU2oPuzx5"
}
```

컬렉션에 100개가 넘는 항목이 있으면 `@odata.nextLink`를 사용하여 다음 결과 페이지를 가져옵니다.

```http
GET https://api.partner.microsoft.com/v1.0/engagements/referrals HTTP/1.1
Authorization: Bearer <token>
Host: api.partner.microsoft.com
Content-Type: application/json
```
