---
title: 환율 가져오기
description: 지정 된 월의 외부 환율을 가져옵니다.
ms.date: 01/24/2020
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: 1e718624db54dcc2ed2b5d2d93dfd1cef0e6f96f
ms.sourcegitcommit: bb3f5f7ee0489bded86fe52e55018c1f4f5032e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88001672"
---
# <a name="get-foreign-exchange-rates"></a>환율 가져오기

적용 대상:

- 파트너 API

이 항목에서는 특정 월의 외부 환율을 가져오는 방법을 설명 합니다.

## <a name="prerequisites"></a>필수 구성 요소

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오에서는 응용 프로그램 사용자 인증만 지원 합니다. 응용 프로그램 전용은 아직 지원 되지 않습니다.
- 이 API는 현재 파트너가 전역 관리자, 관리 에이전트 또는 판매 에이전트 역할 중 하나 여야 하는 사용자 액세스만 지원 합니다.


## <a name="details"></a>세부 정보

- 현재 Azure 계획 CSP 로컬 통화에 대 한 예상 요금을 계산 하기 위해 [get price SHEET API](get-a-price-sheet.md) 와 함께 사용 됩니다.
- 외부 환율이 게시 된 달 전체에 대해 적용 됩니다.
- Azure 계획 [가격 책정](pricing.md) 에 대 한 자세한 내용은 [azure 계획 가격 책정 설명서](https://docs.microsoft.com/partner-center/azure-plan-price-list)에서 찾을 수 있습니다.
- 파트너 가격 책정 및 외부 환율 Api는 [파트너 센터 SDK](https://docs.microsoft.com/partner-center/develop/get-started)의 일부가 아닙니다.
- 이 메서드는 결과를 파일 스트림으로 반환 합니다. 파일 스트림은 .csv 파일 이거나 .csv의 zip 압축 된 버전입니다. 압축 된 파일을 요청 하는 방법에 대 한 자세한 내용은 아래에 포함 되어 있습니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 방법   | 요청 URI                                                                                                 |
|----------|-------------------------------------------------------------------------------------------------------------|
| **GET** | https://api.partner.microsoft.com/v1.0/sales/fxrates(Month=' {month} ')/$value                                  |

### <a name="uri-required-parameters"></a>URI 필수 매개 변수

다음 경로 매개 변수를 사용 하 여 원하는 외부 환율이 있는 월을 요청 합니다.

| Name                   | Type     | 필수 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|월                      | 문자열   | 예       | YYYMM 형식 이어야 합니다. 생략 하는 경우 기본값은 현재 월입니다.       |

### <a name="request-headers"></a>요청 헤더

- 자세한 내용은 [파트너 REST 헤더](headers.md)를 참조하세요.

위의 헤더 외에도 압축 된 대역폭과 다운로드 시간을 줄여서 파일을 검색할 수 있습니다. 기본적으로 파일은 압축 되지 않습니다. 압축 된 버전의 파일을 가져오려면 아래 헤더 값을 포함 하면 됩니다. 압축 된 시트는 4 월 2020 일에만 사용할 수 있으므로 4 월 2020 이전 모든 요청은 압축 되지 않음 으로만 사용할 수 있습니다.

| header                   | 값 형식     | 값 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|Accept-Encoding| 문자열   | deflate| 선택 사항입니다. 생략 된 파일 스트림이 압축 되지 않은 경우       |

### <a name="request-example"></a>요청 예제

```http
GET https://api.partner.microsoft.com/v1.0/sales/fxrates(Month='201909')/$value HTTP/1.1
Authorization: Bearer
Accept-Encoding: deflate
Host: api.partner.microsoft.com

```

## <a name="rest-response"></a>REST 응답

성공 하면이 메서드는 외부 환율을 파일 스트림으로 반환 합니다. 파일 스트림은 .csv 파일 이거나 .csv의 zip 압축 된 버전입니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 HTTP 상태 코드가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 이 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다. 전체 목록은 [오류 코드](error-codes.md)를 참조하세요.

### <a name="response-example"></a>응답 예제

``` http
HTTP/1.1 200 OK
Cache-Control: private
Content-Length: 18548
Content-Type: application/octet-stream
Content-Disposition: attachment; filename=fxrates
Request-ID: 65fb6e59-051b-42f7-8771-c8c139b3c901
Date: Wed, 02 Oct 2019 03:42:54 GMT

"CurrencyCode","USDPerUnit","Month"
"AED","0.27224589249009701","2019”
======= Truncated ==============

```
