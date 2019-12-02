---
title: 파트너 API REST 헤더
description: 파트너 REST API에서 지원하는 HTTP 요청 및 응답 헤더는 다음과 같습니다.
ms.date: 05/21/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-csp
ms.localizationpriority: medium
ms.openlocfilehash: 9df240642526b51e0f986a450dbd3d85db80d561
ms.sourcegitcommit: f7918b7775ca8c6192b2a3e61edb74547730672d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "74556453"
---
# <a name="partner-rest-api-headers"></a>파트너 REST API 헤더

파트너 REST API에서 지원되는 HTTP 요청 및 응답 헤더는 다음과 같습니다.

> [!NOTE]
> 모든 API 호출이 모든 헤더를 수락하는 것은 아닙니다.

## <a name="request-headers"></a>요청 헤더

파트너 REST API에서 지원하는 HTTP 요청 헤더는 다음과 같습니다.

| 헤더                       | 값 유형 | 설명                                                                            |
|------------------------------|------------|----------------------------------------------------------------------------------------|
| 권한 부여           | 문자열     | 필수. 전달자 &lt;토큰&gt;의 인증 토큰입니다.                    |
| 수락                  | 문자열     | 요청 및 응답 유형 "application/json"을 지정합니다.                           |
| client-request-id         | GUID       | 필수. 호출의 고유 식별자이며, 오류 문제 해결을 위한 로그 및 네트워크 추적에 유용합니다. 모든 호출마다 값을 재설정해야 합니다. 모든 작업에 이 헤더를 포함해야 합니다. |
| If-Match:                    | 문자열     | 동시성 제어에 사용됩니다. 일부 API 호출은 If-Match 헤더를 통해 ETag를 전달해야 합니다. ETag는 일반적으로 리소스에 있기 때문에 최신 GET-ting이 필요합니다. |

## <a name="response-headers"></a>응답 헤더

파트너 REST API에서 반환하는 HTTP 응답 헤더는 다음과 같습니다.

| 헤더                    | 값    유형 | 설명                                                                                                               |
|-------------------|------------|--------------------------------------------------------------------------------------------------|
| 수락                | 문자열     | 요청 및 응답 유형 "application/json"을 지정합니다.                                     |
| request-id        | GUID       | 호출에 대한 고유 식별자이며 id-potency를 보장하는 데 사용됩니다. 시간 초과가 발생할 경우 다시 시도 호출에 동일한 값을 포함해야 합니다. 응답(성공 또는 비즈니스 오류)을 수신하면 다음 호출에 대한 값을 다시 설정해야 합니다. |
| client-request-id| GUID| 호출의 고유 식별자이며, 오류 문제 해결을 위한 로그 및 네트워크 추적에 유용합니다. 모든 호출마다 값을 재설정해야 합니다. 모든 작업에 이 헤더를 포함해야 합니다.                                                |
| x-ms-ags-diagnostic   | 문자열 | 서비스의 진단 정보를 포함하는 문자열입니다.
| timestamp|문자열 | API에 도달했을 때 요청의 타임 스탬프입니다.
|ETag |문자열 | 리소스의 ETag입니다.