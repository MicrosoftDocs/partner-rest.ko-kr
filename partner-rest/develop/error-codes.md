---
title: 파트너 API REST 오류 코드
description: 파트너 REST API는 요청의 성공 또는 실패에 대한 상태 코드를 포함하는 JSON 개체를 반환합니다.
ms.date: 05/21/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-csp
ms.localizationpriority: medium
ms.openlocfilehash: 8b793a06a6f1aedc339b9e6be271a8307926da20
ms.sourcegitcommit: f7918b7775ca8c6192b2a3e61edb74547730672d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "74556603"
---
# <a name="partner-api-rest-error-codes"></a>파트너 API REST 오류 코드

적용 대상:

- 파트너 API

파트너 REST API의 오류는 표준 HTTP 상태 코드와 JSON 오류 응답 개체를 사용하여 반환됩니다.

## <a name="http-status-codes"></a>HTTP 상태 코드

아래 표에는 반환될 수 있는 HTTP 상태 코드와 설명이 나열되어 있습니다.

| 상태 코드 | 상태 메시지                  | 설명                                                                                                                            |
|:------------|:--------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------|
| 400         | 잘못된 요청                     | 요청이 잘못되었거나 형식이 잘못되어 요청을 처리할 수 없습니다.                                                                       |
| 401         | 권한 없음                    | 필요한 인증 정보가 누락되었거나 리소스에 대해 유효하지 않습니다.                                                   |
| 403         | 사용할 수 없음                       | 요청한 리소스에 대한 액세스가 거부되었습니다. 사용자에게 충분한 권한이 없습니다. **중요: 조건부 액세스 정책이 리소스에 적용되는 경우 `HTTP 403; Forbidden error=insufficent_claims`가 반환될 수 있습니다.** Microsoft Graph 및 조건부 액세스에 대한 자세한 내용은 [Azure Active Directory 조건부 액세스에 대한 개발자 지침](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-conditional-access-developer)을 참조하세요.  |
| 404         | 없음                       | 요청된 리소스가 없습니다.                                                                                                  |
| 405         | 메서드가 허용되지 않음              | 요청의 HTTP 메서드가 리소스에서 허용되지 않습니다.                                                                         |
| 406         | 허용되지 않음                  | 이 서비스는 Accept 헤더에 요청된 형식을 지원하지 않습니다.                                                                |
| 409         | 충돌                        | 현재 상태가 요청에 필요한 내용과 충돌합니다. 예를 들어, 지정된 상위 폴더가 존재하지 않을 수 있습니다.                   |
| 410         | 없음                            | 서버에서 요청된 리소스를 더 이상 사용할 수 없습니다.                                               |
| 411         | 길이가 필요함                 | 요청에 Content-Length 헤더가 필요합니다.                                                                                    |
| 412         | 사전 조건 실패             | 요청에 제공된 사전 조건(예: an if-match 헤더)이 리소스의 현재 상태와 일치하지 않습니다.                       |
| 413         | 요청 엔터티가 너무 큼        | 요청 크기가 최대 한도를 초과합니다.                                                                                            |
| 415         | 지원되지 않는 미디어 유형          | 요청의 컨텐츠 유형이 서비스에서 지원하지 않는 유형입니다.                                                      |
| 416         | 요청한 범위가 충분하지 않음 | 지정된 바이트 범위가 유효하지 않거나 사용할 수 없습니다.                                                                                    |
| 422         | 처리할 수 없는 엔터티            | 의미 체계가 올바르지 않아서 요청을 처리할 수 없습니다.                                                                        |
| 423         | 잠금                          | 액세스 중인 리소스가 잠겨 있습니다.                                                                                          |
| 429         | 요청이 너무 많음               | 클라이언트 애플리케이션이 제한되었으며 일정 시간이 경과할 때까지 요청을 반복할 수 없습니다.                |
| 500         | 내부 서버 오류           | 요청을 처리하는 동안 내부 서버 오류가 발생했습니다.                                                                       |
| 501         | 구현되지 않음                 | 요청한 기능이 구현되지 않았습니다.                                                                                               |
| 503         | 서비스 이용 불가             | 유지 관리를 위해 서비스를 일시적으로 사용할 수 없거나 오버로드되었습니다. 지연 후 요청을 반복할 수 있으며 지연 길이는 Retry-After 헤더에 지정할 수 있습니다.|
| 504         | 게이트웨이 시간 초과                 | 서버가 프록시 역할을 하면서 요청을 완료하기 위해 액세스해야 하는 업스트림 서버로부터 적시에 응답을 받지 못했습니다. 503과 함께 발생할 수 있습니다. |
| 507         | 스토리지 부족            | 최대 스토리지 할당량에 도달했습니다.                                                                                            |
| 509         | 대역폭 한도 초과        | 최대 대역폭 한도를 초과하여 앱이 제한되었습니다. 시간이 더 경과하면 앱에서 요청을 다시 시도할 수 있습니다. |

오류 응답은 **error**라는 단일 속성을 포함하는 단일 JSON 개체입니다. 이 개체에는 오류에 대한 모든 세부 정보가 포함됩니다. 여기에 반환된 정보를 HTTP 상태 코드 대신 또는 HTTP 상태 코드와 함께 사용할 수 있습니다. 전체 JSON 오류 본문의 예는 다음과 같습니다.

## <a name="error-resource-type"></a>오류 리소스 유형

오류 응답은 **error**라는 단일 속성을 포함하는 단일 JSON 개체입니다. 이 개체에는 오류에 대한 모든 세부 정보가 포함됩니다. 여기에 반환된 정보를 HTTP 상태 코드 대신 또는 HTTP 상태 코드와 함께 사용할 수 있습니다. 전체 JSON 오류 본문의 예는 다음과 같습니다.

다음 표와 코드 샘플은 오류 응답의 스키마를 설명합니다.

| 이름        | 형식   | 설명                                                                                    |
|-------------|--------|------------------------------------------------------------------------------------------------|
| code        | 문자열 | 항상 반환됩니다. 발생한 오류의 종류를 나타냅니다. Null이 아닙니다.                          |
| message | 문자열 | 항상 반환됩니다. 오류를 자세히 설명하고 추가 디버깅 정보를 제공합니다. Null이 아니고 비어있지 않습니다. 최대 길이는 1024자입니다. |
| innerError        | object  | 선택 사항. 최상위 오류보다 구체적인 추가 오류 개체입니다.                                   |
| target      | 문자열 | 오류가 발생한 대상입니다.                                                      |

### <a name="code-property"></a>Code 속성

`code` 속성에는 다음 가능한 값 중 하나가 포함됩니다. 이러한 오류를 처리할 수 있도록 앱을 준비해야 합니다.

| 코드                      | 설명
|:--------------------------|:--------------
| **accessDenied**          | 호출자에게 작업을 수행할 권한이 없습니다.
| **generalException**      | 알 수 없는 오류가 발생했습니다.
| **invalidRequest**        | 요청이 잘못되었거나 형식이 잘못되었습니다.
| **itemNotFound**          | 리소스를 찾을 수 없습니다.
|**preconditionFailed**     | 요청에 제공된 사전 조건(예: an if-match 헤더)이 리소스의 현재 상태와 일치하지 않습니다.
| **resourceModified**      | 호출자가 마지막으로 읽은 후 업데이트 중인 리소스가 변경되었습니다. 일반적으로 eTag가 일치하지 않습니다.
| **serviceNotAvailable**   | 서비스를 사용할 수 없습니다. 지연 후 요청을 다시 시도하세요. Retry-After 헤더가 있을 수 있습니다.
| **unauthenticated**       | 호출자가 인증되지 않았습니다.

### <a name="message-property"></a>Message 속성

루트의 `message` 속성에는 개발자가 읽을 수 있는 오류 메시지가 포함되어 있습니다. 오류 메시지는 현지화되지 않으며 사용자에게 직접 표시되지 않아야 합니다. 오류를 처리할 때 코드는 언제라도 변경될 수 있고 실패한 요청과 관련된 동적 정보를 포함하는 경우가 많기 때문에 `message` 값을 키 오프하지 않아야 합니다. `code` 속성에 반환된 오류 코드에 대해서만 코드를 작성해야 합니다.

### <a name="innererror-object"></a>InnerError 개체

`innererror` 개체에는 보다 구체적인 추가 오류 코드가 있는 `innererror` 개체가 재귀적으로 더 많이 포함될 수 있습니다. 오류를 처리할 때 앱은 사용 가능한 오류 코드를 모두 반복해야 하고 이해하기 가장 쉬운 항목을 사용해야 합니다.

중첩된 `innererror` 개체 내에서 앱에 발생할 수 있는 몇 가지 추가 오류가 있습니다. 앱이 이러한 항목을 처리할 필요는 없지만 원하는 경우에는 가능합니다. 서비스는 언제든지 새로운 오류 코드를 추가하거나 이전 오류 코드 반환을 중지할 수 있으므로 모든 앱이 [기본 오류 코드]를 처리할 수 있어야 합니다.

```json
{
  "error": {
    "code": "unAuthorized",
    "message": "Caller is not authorized to access the resource.",
    "target": "referral",
    "innerError": {
      "code": "innerErrorCode",
      "message": "Unauthorized referral access"
    }
  }
}
```