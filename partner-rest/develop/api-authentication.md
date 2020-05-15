---
title: 파트너 API 인증
description: 인증을 위해 Azure AD에서 파트너 API를 사용하도록 인증 설정을 구성합니다.
ms.date: 05/17/2019
ms.service: partner-dashboard
ms.subservice: partnercenter-sdk
ms.localizationpriority: medium
ms.openlocfilehash: c5810678dccc2be1c3c084c901299961d6ba7820
ms.sourcegitcommit: 3c165938f544ff226cbe11ca21ed5aa00448d9b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80342306"
---
# <a name="partner-api-authentication"></a>파트너 API 인증

적용 대상:

- 파트너 API

파트너 API는 인증을 위해 Azure AD(Azure Active Directory)를 사용합니다. 파트너 API와 상호 작용 하는 경우 Azure AD 애플리케이션을 올바르게 구성하고 액세스 토큰을 가져와야 합니다. [애플리케이션 및 사용자 액세스](#application-and-user-access) 또는 [애플리케이션 전용 액세스](#application-only-access)를 위한 액세스 토큰을 가져올 수 있습니다.

## <a name="application-and-user-access"></a>애플리케이션 및 사용자 액세스

이 방법은 API에 대한 **애플리케이션 및 사용자 액세스**를 설정하는 데 사용하는 것이 좋습니다.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **Azure Active Directory** 서비스를 선택합니다.
3. **앱 등록**을 선택한 다음, **새 애플리케이션 등록**을 선택합니다.
4. 새 애플리케이션을 만듭니다. **애플리케이션 종류**에 **기본**을 선택합니다. 이름과 URL을 입력한 후 **만들기**를 선택합니다.
5. 애플리케이션에 대한 **API 사용 권한**을 선택합니다. **API 사용 권한 요청** 화면에서 **사용 권한 추가**를 선택한 다음, **내 조직이 사용하는 API**를 선택합니다.
6. Microsoft 파트너(Microsoft 개발자 센터) API(`4990cffe-04e8-4e8b-808a-1175604b879f`)를 검색합니다.  

    ![Microsoft 파트너 API를 검색하는 API 사용 권한 요청 화면 스크린샷](../images/SearchGatewayApi.png)

7. **위임된 권한**을 **파트너 센터**로 설정합니다.

    ![Microsoft 파트너 API에 대한 위임된 권한 구성 화면 스크린샷](../images/SelectUserPermission.png)
    
8. *Microsoft 파트너* *(Microsoft 파트너 센터*) API(`fa3d9a0c-3fb0-42cc-9193-47c7ecd2edbd`)를 검색합니다.

    ![Microsoft 파트너 센터 API를 검색하는 API 사용 권한 요청 화면 스크린샷](../images/SearchPCApi.png)
    
9. **Microsoft 파트너 센터**를 선택하고 **user_impersonation**을 확인합니다.

10. **위임된 권한**을 **파트너 센터**로 설정합니다.

    ![Microsoft 파트너 센터 API에 대한 위임된 권한 구성 화면 스크린샷](../images/SelectPCUserPermission.png)

## <a name="application-only-access"></a>애플리케이션 전용 액세스

이 방법은 API에 대한 **애플리케이션 전용 액세스**를 설정하는 데 사용하는 것이 좋습니다.

> [!IMPORTANT]
> Azure AD 애플리케이션의 애플리케이션 ID, 애플리케이션 키 및 디렉토리 ID를 제공해야 합니다.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **Azure Active Directory** 서비스를 선택합니다.
3. **앱 등록**을 선택한 다음, **새 애플리케이션 등록**을 선택합니다.
4. 새 애플리케이션을 만듭니다. **애플리케이션 유형**에 **웹앱/API**를 선택합니다. 애플리케이션 **이름**과 **URL**을 입력합니다. 그런 다음, **만들기**를 선택합니다.
5. 애플리케이션에 대한 **API 사용 권한**을 선택합니다. **사용 권한 추가**를 선택한 다음, **내 조직이 사용하는 API**를 선택합니다.
6. Microsoft 파트너(Microsoft 개발자 센터) API(`4990cffe-04e8-4e8b-808a-1175604b879f`)를 검색합니다.  

    ![Microsoft 파트너 API를 검색하는 API 사용 권한 요청 화면 스크린샷](../images/SearchGatewayApi.png)

7. **위임된 권한**을 **파트너 센터**로 설정합니다.

    ![Microsoft 파트너 API에 대한 위임된 권한 구성 화면 스크린샷](../images/SelectUserPermission.png)

8. 등록한 애플리케이션에 대한 **속성**을 선택한 다음, **애플리케이션 ID 복사**를 선택합니다.
9. **설정**을 선택한 후**인증서 및 비밀**을 선택합니다. **새 클라이언트 암호**를 선택하고 **만료**를 **무기한**으로 설정합니다. 그런 다음, **저장**을 선택합니다.
10. **키** 메뉴에서 **Copy the key value**(키 값 복사)를 선택합니다. 이 값의 복사본을 저장합니다.

> [!WARNING]
> 만든 키에 대한 키 값의 복사본을 저장해야 합니다. 이 키 값을 사용해야 나중에 토큰을 가져올 수 있습니다.

## <a name="partner-consent"></a>파트너 동의

Azure 관리 포털에서 **엔터프라이즈 애플리케이션**을 선택합니다. 이전 섹션에서 만든 애플리케이션을 검색하고 해당 애플리케이션을 선택합니다. **사용 권한**을 선택한 다음, **Grant Admin Consent for Partner Account**(파트너 계정에 대한 관리자 동의 부여)를 선택합니다.
