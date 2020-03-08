---
title: 조회 커넥터.
description: Microsoft Flow를 사용하여 파트너 조회를 Dynamics 365 CRM 잠재 고객과 동기화합니다.
ms.date: 07/22/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-csp
ms.localizationpriority: medium
ms.openlocfilehash: 995d0932da9cd28407babfdc54e695caac9882d5
ms.sourcegitcommit: 50d18c96d24755174beb4fcb694223325a7fe450
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "76542028"
---
# <a name="referral-connectors"></a>조회 커넥터

조회 커넥터를 사용하여 파트너 조회를 CRM(고객 관계 관리) 잠재 고객과 동기화할 수 있습니다. [Microsoft Flow](https://flow.microsoft.com)를 파트너 조회를 수신하는 HTTPS 엔드포인트로 사용하여 조회 커넥터를 만들 수 있습니다. 그런 다음, Flow에서 받은 조회를 CRM 시스템에 잠재 고객으로 쓸 수 있습니다.

## <a name="prerequisites"></a>필수 조건

* Microsoft Flow 구독
  * 이 구독에 대한 관리자 액세스 권한이 있는 계정
* Azure AD(Azure Active Directory) 애플리케이션 ID, 사용자 ID, 암호 및 테넌트 ID(파트너 API에 액세스하는 데 사용됨). 설치 지침은 [파트너 인증](api-authentication.md)을 참조하세요.
* [Azure 함수 앱](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) 구독
* [조회 생성](https://docs.microsoft.com/partner-center/develop/partner-center-webhook-events) 및 [조회 업데이트](https://docs.microsoft.com/partner-center/develop/partner-center-webhook-events#referral-created-event) 이벤트에 대한 [파트너 센터 웹후크 이벤트](https://docs.microsoft.com/partner-center/develop/partner-center-webhook-events#referral-updated-event)
* [Microsoft Dynamics 365](https://dynamics.microsoft.com) 구독
  * 판매 모듈 사용
  * 이 구독에 대한 관리자 액세스 권한이 있는 계정

## <a name="flow-overview"></a>흐름 개요

다음 흐름을 사용하여 조회를 CRM으로 가져옵니다.

1. 파트너가 조회 알림을 받도록 웹후크를 설정합니다.
2. 파트너가 파트너 센터에 웹후크를 등록합니다. 파트너가 조회가 생성되거나 업데이트되는 경우 웹후크 이벤트를 구독합니다.
3. 조회 클라이언트가 조회를 만들거나 업데이트합니다.
4. 파트너 센터 웹후크 시스템이 파트너의 등록을 확하고 웹후크에 알림을 보냅니다.
5. 웹후크가 알림을 받습니다.
6. Microsoft Flow의 Flow 문서가 토큰을 사용하여 파트너 센터 조회 API를 호출합니다.
7. Flow 엔드포인트가 조회를 가져옵니다.
8. Flow 엔드포인트가 CRM 잠재 고객을 만듭니다.

![CRM으로 파트너 조회를 가져오는 섹션의 단계를 보여주는 순서도입니다.](../images/referralwebhook.png)

## <a name="flow-document-process"></a>Flow 문서 프로세스

조회 커넥터의 Flow 문서는 파트너 조회를 Dynamics 365의 CRM 잠재 고객과 동기화합니다.

1. 커넥터가 `https://api.partner.microsoft.com/v1.0/engagements/referrals`에 연결할 토큰을 가져옵니다.
2. 커넥터가 `https://api.partner.microsoft.com/v1.0/engagements/referrals/{id}`를 사용하여 커넥터를 트리거한 조회를 가져옵니다.
3. 커넥터가 Dynamics 365에 연결합니다.
4. 커넥터가 조회의 최신 정보를 사용하여 기존 잠재 고객을 업데이트하거나 새 잠재 고객을 만듭니다.
5. 커넥터가 CRM 잠재 고객의 최신 업데이트로 조회를 업데이트합니다.

![Flow 문서 프로세스에 대한 섹션의 단계를 보여주는 순서도입니다.](../images/ReferralFlowSteps.png)

## <a name="sample-referral-connector"></a>샘플 조회 커넥터

다음 *샘플 조회 커넥터*는 Dynamics 365에서 CRM 잠재 고객에 파트너 센터 조회를 동기화하는 방법을 보여줍니다.

> [!IMPORTANT]
> 샘플 코드에서 [Flow 커넥터](https://flow.microsoft.com/en-us/connectors/)를 교체하여 다른 CRM에 쓸 수 있습니다.

### <a name="import-flow-synchronization-package"></a>흐름 동기화 패키지 가져오기

샘플 코드 패키지를 다운로드하여 Microsoft Flow로 가져와서 Dynamics 365에 연결합니다.

1. [GitHub 리포지토리](https://github.com/microsoft/Partner-Center-Referrals/blob/master/flowconnectors/MicrosoftDynamicsCRM/PartnerReferralsToDynamicsCRMLead.zip?raw=true)에서 [흐름 동기화 패키지](https://github.com/microsoft/Partner-Center-Referrals)를 다운로드합니다.
2. 적절한 자격 증명을 사용하여 [Microsoft Flow](https://flow.microsoft.com)에 로그인합니다.
3. 탐색 메뉴에서 **내 흐름**을 선택합니다. 그런 다음, **가져오기**를 선택합니다.
4. **패키지 가져오기** 페이지에서 다운로드한 흐름 동기화 패키지를 선택합니다. 그런 다음, **업로드**를 선택합니다.

    ![패키지 파일의 패키지 가져오기 화면](../images/importPackage.png)

5. 패키지 업로드가 완료된 후 **패키지 콘텐츠 검토**에서 업로드한 패키지를 찾습니다.

    ![패키지 가져오기 화면의 세부 정보](../images/importPackageDetails.png)

6. 업로드한 패키지에 대한 **작업** 단추(연필 아이콘)를 선택합니다. 그러면 **설치 가져오기** 블레이드가 열립니다.
7. **설치** 유형을 선택합니다.

    * 새 흐름을 만들려면 **새 항목으로 만들기**를 선택하고 새 **리소스 이름**을 입력합니다.
    * 같은 이름으로 기존 흐름을 업데이트하려면 **업데이트**를 선택합니다.

    ![새 패키지 생성 또는 업데이트 화면](../images/CreateNewConnection.png)

8. **패키지 가져오기** 페이지의 **관련 리소스** 아래 **패키지 콘텐츠 검토** 섹션에서 Dynamics 365 연결을 찾습니다.
9. Dynamics 365 연결에 대한 **작업** 단추(연필 아이콘)를 선택합니다. 그러면 관련 리소스에 대한 **설치 가져오기** 블레이드가 열립니다.
10. **새로 만들기**를 선택하여 새로운 Dynamics 365 연결을 만들거나 기존 연결을 선택합니다.
11. **패키지 가져오기** 페이지에 선택한 흐름 설치 유형과 Dynamics 365 연결이 표시되는지 확인합니다. 그런 다음, **가져오기**를 선택합니다.

    ![패키지 가져오기 상태 화면](../images/importStatus.png)

12. 이제 흐름 리소스가 만들어지거나 업데이트되었는지 확인합니다.

### <a name="configure-flow-parameters"></a>흐름 매개 변수 구성

흐름 리소스의 매개 변수를 구성합니다.

1. [Microsoft Flow](https://flow.microsoft.com)의 탐색 메뉴에서 **내 흐름**을 선택합니다.
2. 이전 섹션에서 만들거나 업데이트한 흐름 리소스를 선택합니다.
3. 흐름 페이지에서 **흐름 편집**을 선택합니다.
4. **AAD-Application (client) ID** 변수를 선택하고 Azure AD 애플리케이션의 ID를 입력합니다.
5. **UserId** 변수를 선택하고 사용자 ID를 입력합니다.
6. **UserPassword** 변수를 선택하고 사용자 암호를 입력합니다.
7. **AAD-Directory (tenant) ID** 변수를 선택합니다. Azure AD 애플리케이션의 테넌트 ID를 입력합니다.
8. **저장**을 클릭하여 흐름을 저장합니다.

    ![흐름 변수 설정 화면](../images/SetFlowVariables.png)

### <a name="authenticate-the-callback"></a>콜백 인증

파트너 센터에서 콜백 이벤트를 인증합니다.

> [!TIP]
> 예제는 다음 섹션에서 [샘플 함수 앱 코드](#sample-function-app-code)를 참조하세요.

1. [파트너 센터에서 콜백 이벤트를 인증](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal)하는 [Azure 함수 앱을 만듭니다](https://docs.microsoft.com/partner-center/develop/partner-center-webhooks#how-to-authenticate-the-callback).

    1. 필요한 헤더(**Authorization**, **x-ms-certificate-url**, **x-ms-signature-algorithm**)가 있는지 확인합니다.
    2. 콘텐츠를 서명하는 데 사용된 인증서(**x-ms-certificate-url**)를 다운로드합니다.
    3. 인증서 체인을 확인합니다.
    4. 인증서의 **조직**을 확인합니다.
    5. UTF-8 인코딩 콘텐츠를 버퍼로 읽습니다.
    6. RSA 암호화 공급자를 만듭니다.
    7. [서명이 지정된 해시 알고리즘(예 : SHA256)으로 서명된 것과 일치](https://docs.microsoft.com/partner-center/develop/partner-center-webhooks#example-for-signature-validation)하는지 확인합니다.
    8. 확인이 성공하면 **OK** 메시지가 반환됩니다.

2. 함수 앱의 HTTP 엔드포인트에 대해 생성된 콜백 URI를 확인합니다. 이 URI는 함수 앱을 만들 때 표시됩니다. 함수 앱의 Azure 리소스 페이지에서 이 URI를 찾을 수도 있습니다.
3. [Microsoft Flow](https://flow.microsoft.com)에서 *[흐름 동기화 패키지 가져오기](#import-flow-synchronization-package)* 섹션에서 가져온 "Microsoft Dynamics CRM 잠재 고객에 대한 파트너 조회"를 편집합니다.

    1. 함수 앱의 URI 값을 "웹후크 인증서 유효성 검사" 단계에 추가합니다.
    2. 함수 앱의 콜백 URI를 복사하여 Flow 문서에 붙여 넣습니다.
    3. Flow 문서를 저장합니다.

#### <a name="sample-function-app-code"></a>샘플 함수 앱 코드

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using System;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.Threading.Tasks;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    string requestBody = null;
    if (!string.IsNullOrWhiteSpace(GetFirstValueFromHeader(req, "x-ms-certificate-url")) && !string.IsNullOrWhiteSpace(GetFirstValueFromHeader(req, "x-ms-signature-algorithm")))
    {
        var certificateUrl = req?.Headers["x-ms-certificate-url"].First();
        try
        {
            string resultContent = null;
            using (var client = new HttpClient())
            {
                var result = await client.GetAsync(req.Headers["x-ms-certificate-url"].First());
                resultContent = await result.Content.ReadAsStringAsync();
                log.LogInformation(resultContent);
            }
            if (!string.IsNullOrEmpty(resultContent))
            {
                var certificate = new X509Certificate2(Encoding.UTF8.GetBytes(resultContent));
                var validationResult = certificate.Verify() && certificate.Issuer.Contains("O=Microsoft Corporation");
                if (validationResult)
                {
                    return new OkResult();
                }
                else
                {
                    return new BadRequestResult();
                }
            }
        }
        catch (Exception)
        {
            new BadRequestObjectResult("Certificate could not be retrieved, invalid caller to flow");
        }

        requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    }
    else
    {
        new BadRequestObjectResult("Missing headers");
    }

    return new BadRequestObjectResult("Certificate validation failed");
}

private static string GetFirstValueFromHeader(HttpRequest request, string headerName)
{
    StringValues matchingHeaderValues;
    request.Headers.TryGetValue(headerName, out matchingHeaderValues);
    return matchingHeaderValues.Count != 0 ? matchingHeaderValues.First() : string.Empty;
}
```

### <a name="register-flow-with-partner-center"></a>파트너 센터에 흐름 등록

파트너 센터에 흐름 리소스를 등록하여 흐름을 트리거하고 웹후크 이벤트를 수신합니다.

1. [Microsoft Flow](https://flow.microsoft.com)의 탐색 메뉴에서 **내 흐름**을 선택합니다.
2. 만들거나 업데이트한 흐름을 선택합니다.
3. 흐름 페이지에서 **흐름 편집**을 선택합니다.
4. 흐름의 **HTTP POST URL**을 복사하여 저장합니다. 이 URL은 흐름을 트리거하는 데 사용해야 합니다.
5. 조회가 생성되거나 업데이트될 때 [웹후크 이벤트를 수신하도록 등록](https://docs.microsoft.com/partner-center/develop/partner-center-webhooks#register-to-receive-events)합니다. 다음 본문 형식을 사용합니다.

```json
{
    "WebhookUrl": "<<FlowUrl>>",
    "WebhookEvents": [
        "referral-created",
        "referral-updated"
    ],
    "signatureTokenToMsSignatureHeader": true
}
```
