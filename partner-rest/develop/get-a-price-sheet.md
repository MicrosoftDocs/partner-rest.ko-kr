---
title: 가격표 가져오기
description: 지정 된 시장 및 보기에 대 한 가격표를 가져옵니다. 는 월별로 기록을 가져오는 필터를 지원 합니다.
ms.date: 01/24/2020
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: 5195ebed6559bd71a7832a667e63ee801be1c82f
ms.sourcegitcommit: bb3f5f7ee0489bded86fe52e55018c1f4f5032e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88001660"
---
# <a name="get-a-price-sheet"></a>가격표 가져오기

적용 대상:

- 파트너 API

이 항목에서는 지정 된 시장 및 보기에 대 한 가격표를 가져오는 방법에 대해 설명 합니다. 이 메서드는 월별 기록을 가져오는 필터를 지원 합니다.

## <a name="prerequisites"></a>필수 구성 요소

- 자격 증명은 [파트너 API 인증](api-authentication.md)에 설명되어 있습니다. 이 시나리오에서는 응용 프로그램 사용자 인증만 지원 합니다. 응용 프로그램-모드은 아직 지원 되지 않습니다.
- 이 API는 현재 파트너가 전역 관리자, 관리 에이전트 또는 판매 에이전트 역할 중 하나 여야 하는 사용자 액세스만 지원 합니다.

## <a name="details"></a>세부 정보

- 현재는 Azure 계획 사용량과 예약 제품에 대해서만 데이터를 반환 합니다.
- 현재 [가격](pricing.md) 에는 API가 호출 된 날짜까지 현재 달에 사용 가능한 모든 미터 및 제품이 포함 됩니다. 이전 달에는 지정 된 월에 사용할 수 있는 모든 미터 및 제품이 포함 됩니다.
- 소비 측정기 가격은 USD에 불과합니다. 파트너는 외부 교환 요금 API를 사용 하 여 현지 통화 비용을 계산 하는 것입니다.
- 소비 측정기 가격은 예상 소매 가격입니다. 파트너 할인은 파트너의 [획득 크레딧을](https://docs.microsoft.com/partner-center/partner-earned-credit-explanation)통해 사용할 수 있습니다.
- 예약 측정기 가격은 CSP 파트너 할인을 포함 합니다. 예약에 대 한 예상 소매 가격은 파트너 센터 "가격 책정 및 제품" 페이지에서 다운로드할 수 있는 예약 공유 서비스에서 찾을 수 있습니다.
- Azure 계획 가격 책정에 대 한 자세한 내용은 [azure 계획 가격 책정 설명서](https://docs.microsoft.com/partner-center/azure-plan-price-list)에서 찾을 수 있습니다.
- 파트너 가격 책정 및 외부 환율 Api는 [파트너 센터 SDK](https://docs.microsoft.com/partner-center/develop/get-started)의 일부가 아닙니다.
- 이 메서드는 가격 목록을 파일 스트림으로 반환 합니다. 파일 스트림은 .csv 파일 이거나 .csv의 zip 압축 된 버전입니다. 압축 된 파일을 요청 하는 방법에 대 한 자세한 내용은 아래에 포함 되어 있습니다.

## <a name="rest-request"></a>REST 요청

### <a name="request-syntax"></a>요청 구문

| 방법   | 요청 URI                                                                                                 |
|----------|-------------------------------------------------------------------------------------------------------------|
| **GET** | https://api.partner.microsoft.com/v1.0/sales/pricesheets(Market=' {market} ', PricesheetView = ' {view} ')/$value                                     |

### <a name="uri-required-parameters"></a>URI 필수 매개 변수

다음 경로 매개 변수를 사용 하 여 원하는 가격 책정 시트의 시장 및 유형을 요청 합니다.

| Name                   | Type     | 필수 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|시장                      | 문자열   | 예       | 시장에서 요청 하는 국가의 두 문자 국가 코드       |
|PricesheetView | 문자열   | 예       | 요청 되는 가격표의 유형입니다. azure_consumption 하거나 azure_reservations       |

### <a name="uri-filter-parameters"></a>URI 필터 매개 변수

다음 필터 매개 변수를 사용 합니다.

| Name                   | Type     | 필수 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|타임라인| 문자열   | No| 전달 되지 않은 경우 기본값은 current입니다. 가능한 값은 기록, 현재 및 미래입니다.       |
|월| 문자열   | No| 기록이 요청 된 경우에만 필요 합니다. 요청 되는 가격표에 대해 YYYYMM을 준수 해야 합니다.       |

### <a name="request-headers"></a>요청 헤더

- 자세한 내용은 [파트너 REST 헤더](headers.md)를 참조하세요.

위의 헤더 외에도 가격 파일은 압축 된 대역폭과 다운로드 시간으로 검색할 수 있습니다. 기본적으로 파일은 압축 되지 않습니다. 압축 된 버전의 파일을 가져오려면 아래 헤더 값을 포함 하면 됩니다. 압축 된 시트는 4 월 2020 일에만 사용할 수 있으며 4 월 2020 일 전의 모든 시트는 압축 되지 않음 으로만 사용할 수 있습니다.

| header                   | 값 형식     | 값 | 설명                                                     |
|------------------------|----------|----------|-----------------------------------------------------------------|
|Accept-Encoding| 문자열   | deflate| 선택 사항입니다. 생략 된 파일 스트림이 압축 되지 않은 경우       |

### <a name="request-example"></a>요청 예제

```http
GET https://api.partner.microsoft.com/v1.0/sales/pricesheets(Market='ad',PricesheetView='azure_consumption')/$value?timeline=history&month=201909 HTTP/1.1
Authorization: Bearer
Accept-Encoding: deflate
Host: api.partner.microsoft.com

```

## <a name="rest-response"></a>REST 응답

성공 하면이 메서드는 가격 목록을 파일 스트림으로 반환 합니다. 파일 스트림은 .csv 파일 이거나 .csv의 zip 압축 된 버전입니다.

### <a name="response-success-and-error-codes"></a>응답 성공 및 오류 코드

각 응답에는 성공 또는 실패와 추가 디버깅 정보를 나타내는 HTTP 상태 코드가 함께 제공됩니다. 네트워크 추적 도구를 사용하여 이 코드, 오류 유형 및 추가 매개 변수를 읽을 수 있습니다. 전체 목록은 [오류 코드](error-codes.md)를 참조하세요.

### <a name="response-example"></a>응답 예제

``` http
HTTP/1.1 200 OK
Cache-Control: private
Content-Length: 42180180
Content-Type: application/octet-stream
Content-Disposition: attachment; filename=pricesheet
Request-ID: 9f8bed52-e4df-4d0c-9ca6-929a187b0731
Date: Wed, 02 Oct 2019 03:41:20 GMT

"ProductTitle","ProductId","SkuId","SkuTitle","Publisher","SkuDescription","UnitOfMeasure","TermDuration","Market","Currency","UnitPrice","PricingTierRangeMin","PricingTierRangeMax","EffectiveStartDate","EffectiveEndDate","MeterIds","MeterType","Tags“
"Advanced Data Security - SQL Database","DZH318Z0C16V","001J","Advanced Data Security - SQL Database - Standard - US East 2","Microsoft","Advanced Data Security - SQL Database - Standard - US East 2","1 Node/Month","payG-1","US","USD","15","","","3/1/2018 12:00:00 AM","11/30/9999 11:59:59 PM","cb0969aa-aaaa-4d6c-ab4b-7e182fa06aff","1 Node/Month","Azure“
======= Truncated ==============

```
