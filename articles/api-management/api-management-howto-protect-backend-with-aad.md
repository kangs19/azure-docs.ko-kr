---
title: AAD 및 API Management에서 OAuth 2.0을 사용 하 여 API 보호
titleSuffix: Azure API Management
description: Azure Active Directory 및 API Management로 웹 API 백 엔드를 보호하는 방법에 대해 알아봅니다.
services: api-management
documentationcenter: ''
author: miaojiang
manager: dcscontentpm
editor: ''
ms.service: api-management
ms.workload: mobile
ms.topic: article
ms.date: 06/24/2020
ms.author: apimpm
ms.openlocfilehash: 72899e743e167eef5ee7d1be04cb50cafc1f2a95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445511"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory 및 API Management에서 OAuth 2.0을 사용하여 API 보호

이 가이드에서는 Azure AD(Azure Active Directory)에서 OAuth 2.0 프로토콜을 사용하여 API를 보호하도록 Azure API Management 인스턴스를 구성하는 방법을 보여 줍니다. 

> [!NOTE]
> 이 기능은 API Management **개발자**, **기본**, **표준**및 **프리미엄** 계층에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서의 단계를 따르려면 다음이 있어야 합니다.

- API Management 인스턴스
- API Management 인스턴스를 사용하는 게시되고 있는 API
- Azure AD 테넌트

## <a name="overview"></a>개요

다음은 단계에 대 한 간략 한 개요입니다.

1. API를 나타내기 위해 Azure AD에 애플리케이션(backend-app) 등록합니다.
1. API를 호출해야 하는 클라이언트 애플리케이션을 나타내기 위해 Azure AD에 다른 애플리케이션(client-app)을 등록합니다.
1. Azure AD에서 client-app에 backend-app을 호출할 수 있도록 권한을 부여합니다.
1. OAuth 2.0 사용자 권한 부여를 사용 하 여 API를 호출 하도록 개발자 콘솔을 구성 합니다.
1. **validate-jwt** 정책을 추가하여 들어오는 요청마다 OAuth 토큰의 유효성을 검사합니다.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API를 나타내기 위해 Azure AD에 애플리케이션 등록

Azure AD를 사용 하 여 API를 보호 하려면 먼저 API를 나타내는 응용 프로그램을 Azure AD에 등록 합니다. 

1. [Azure Portal](https://portal.azure.com) 로 이동 하 여 응용 프로그램을 등록 합니다. **앱 등록**을 검색 하 고 선택 합니다.

1. **새 등록**을 선택합니다. 

1. **애플리케이션 등록** 페이지가 표시되면 애플리케이션의 등록 정보를 입력합니다.

   - **이름** 섹션에 응용 프로그램 사용자에 게 표시 되는 의미 있는 응용 프로그램 이름 (예: *백 엔드-앱)* 을 입력 합니다. 
   - **지원 되는 계정 유형** 섹션에서 시나리오에 적합 한 옵션을 선택 합니다. 

1. **리디렉션 URI** 섹션을 비워 둡니다.

1. **등록**을 선택하여 애플리케이션을 만듭니다. 

1. 나중에 사용할 수 있도록 앱 **개요** 페이지에서 **애플리케이션(클라이언트) ID** 값을 찾아서 기록해 둡니다.

1. **API** 표시를 선택 하 고 **응용 프로그램 ID URI** 를 기본값으로 설정 합니다. 나중에이 값을 기록 합니다.

1. 범위 **추가** 단추를 선택 하 여 **범위 추가** 페이지를 표시 합니다. 그런 다음 API에서 지원 되는 새 범위 (예:)를 만듭니다 `Files.Read` .

1. 범위 **추가** 단추를 선택 하 여 범위를 만듭니다. API에서 지원 되는 모든 범위를 추가 하려면이 단계를 반복 합니다.

1. 범위가 만들어지면 이후 단계에서 사용할 수 있도록 해당 범위를 적어 둡니다. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>클라이언트 애플리케이션을 나타내기 위해 Azure AD에 다른 애플리케이션 등록

API를 호출 하는 모든 클라이언트 응용 프로그램을 Azure AD의 응용 프로그램으로 등록 해야 합니다. 이 예제에서 클라이언트 응용 프로그램은 API Management 개발자 포털의 **개발자 콘솔** 입니다. 

개발자 콘솔을 나타내기 위해 Azure AD에 다른 응용 프로그램을 등록 하려면 다음을 수행 합니다.

1. [Azure Portal](https://portal.azure.com) 로 이동 하 여 응용 프로그램을 등록 합니다.

1.  **앱 등록**을 검색 하 고 선택 합니다.

1. **새 등록**을 선택합니다.

1. **애플리케이션 등록** 페이지가 표시되면 애플리케이션의 등록 정보를 입력합니다.

   - **이름** 섹션에서 응용 프로그램의 사용자에 게 표시 되는 의미 있는 응용 프로그램 이름 (예: *클라이언트-앱)* 을 입력 합니다. 
   - **지원 되는 계정 유형** 섹션에서 **모든 조직 디렉터리 (임의의 Azure AD 디렉터리-다중 테 넌 트)의 계정**을 선택 합니다. 

1. **URI 리디렉션** 섹션에서 `Web` URL 필드를 선택 하 고 현재에 대해 비워 둡니다.

1. **등록**을 선택하여 애플리케이션을 만듭니다. 

1. 나중에 사용할 수 있도록 앱 **개요** 페이지에서 **애플리케이션(클라이언트) ID** 값을 찾아서 기록해 둡니다.

1. 이후 단계에서 사용할이 응용 프로그램에 대 한 클라이언트 암호를 만듭니다.

   1. 클라이언트 앱에 대 한 페이지 목록에서 **인증서 & 암호**를 선택 하 고 **새 클라이언트 암호**를 선택 합니다.

   1. **클라이언트 암호 추가**에서 **설명을**입력 합니다. 키가 만료 되는 시점을 선택 하 고 **추가**를 선택 합니다.

비밀을 만들 때 이후 단계에서 사용할 키 값을 기록해 둡니다. 

## <a name="grant-permissions-in-azure-ad"></a>Azure AD에서 권한 부여

API 및 개발자 콘솔을 나타내기 위해 두 개의 응용 프로그램을 등록 했으므로 클라이언트 앱이 백 엔드 앱을 호출할 수 있도록 권한을 부여 합니다.  

1. [Azure Portal](https://portal.azure.com) 로 이동 하 여 클라이언트 응용 프로그램에 사용 권한을 부여 합니다. **앱 등록**을 검색 하 고 선택 합니다.

1. 클라이언트 앱을 선택 합니다. 그런 다음 앱에 대 한 페이지 목록에서 **API 권한**을 선택 합니다.

1. **권한 추가를**선택 합니다.

1. **Api 선택**에서 **내 api**를 선택한 다음, 백 엔드 앱을 찾아 선택 합니다.

1. **위임 된 권한**에서 백 엔드 앱에 대 한 적절 한 사용 권한을 선택한 다음 **권한 추가**를 선택 합니다.

1. 필요에 따라 **API 사용 권한** 페이지에서이 디렉터리의 모든 사용자를 대신 하 여 동의를 부여 하기 위해 **에 대 한 \<your-tenant-name> 관리자 동의 부여** 를 선택 합니다. 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>개발자 포털에서 OAuth 2.0 사용자 권한 부여를 사용하도록 설정

이제 Azure AD에서 애플리케이션을 만들었으며 client-app에서 backend-app을 호출하도록 허용하는 적절한 권한을 부여했습니다. 

이 예제에서는 개발자 콘솔이 client-app입니다. 아래 단계에서는 개발자 콘솔에서 OAuth 2.0 사용자 권한 부여를 사용하도록 설정하는 방법을 설명합니다. 

1. Azure Portal에서 API Management 인스턴스로 이동 합니다.

1. **OAuth 2.0**  >  **추가**를 선택 합니다.

1. **표시 이름** 및 **설명**을 제공합니다.

1. **클라이언트 등록 페이지 URL**에 대해 `http://localhost`와 같은 자리 표시자 값을 입력합니다. **클라이언트 등록 페이지 URL** 은 사용자가이를 지 원하는 OAuth 2.0 공급자에 대해 고유한 계정을 만들고 구성 하는 데 사용할 수 있는 페이지를 가리킵니다. 이 예제에서는 사용자가 자신의 계정을 만들고 구성하지 않으므로 대신 자리 표시자를 사용합니다.

1. **권한 부여 유형**에 **인증 코드**를 선택합니다.

1. **권한 부여 엔드포인트 URL** 및**토큰 엔드포인트 URL**을 지정합니다. 이러한 값은 Azure AD 테넌트의 **엔드포인트** 페이지에서 검색합니다. **앱 등록** 페이지로 다시 이동하여 **엔드포인트**를 선택합니다.


1. **OAuth 2.0 권한 부여 엔드포인트**를 복사하여 **권한 부여 엔드포인트 URL** 텍스트 상자에 붙여 넣습니다. 권한 부여 요청 방법에서 **POST** 를 선택 합니다.

1. **OAuth 2.0 토큰 엔드포인트**를 복사하여 **토큰 엔드포인트 URL** 텍스트 상자에 붙여 넣습니다. 

   >[!IMPORTANT]
   > **V1** 또는 **v2** 끝점을 사용 합니다. 그러나 선택한 버전에 따라 아래 단계가 달라 집니다. V2 끝점을 사용 하는 것이 좋습니다. 

1. **V1** 끝점을 사용 하는 경우 **리소스**라는 body 매개 변수를 추가 합니다. 이 매개 변수 값에 대해 백 엔드 앱의 **응용 프로그램 ID** 를 사용 합니다. 

1. **V2** 끝점을 사용 하는 경우 **기본 범위** 필드에서 백 엔드 앱에 대해 만든 범위를 사용 합니다. 또한 [`accessTokenAcceptedVersion`](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#accesstokenacceptedversion-attribute) `2` [응용 프로그램 매니페스트에서](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)속성의 값을로 설정 해야 합니다.

1. 다음으로 클라이언트 자격 증명을 지정합니다. 이러한 자격 증명은 client-app에 대한 자격 증명입니다.

1. **클라이언트 id**에는 클라이언트 앱의 **응용 프로그램 ID** 를 사용 합니다.

1. **클라이언트 암호**로 이전에 client-app에 대해 만든 키를 사용합니다. 

1. 클라이언트 암호 바로 뒤에는 권한 부여 코드 권한 유형에 대한 **redirect_url**이 나옵니다. 이 URL을 적어둡니다.

1. **만들기**를 선택합니다.

1. Azure Active Directory에서 클라이언트-앱 등록으로 돌아가서 **인증**을 선택 합니다.

1. **플랫폼 구성** 에서 **플랫폼 추가**를 클릭 하 고, **웹**으로 유형을 선택 하 고, **리디렉션 URI**아래에 **redirect_url** 을 붙여넣은 다음, **구성** 단추를 클릭 하 여 저장 합니다.

OAuth 2.0 권한 부여 서버를 구성했으므로 개발자 콘솔에서 Azure AD의 액세스 토큰을 가져올 수 있습니다. 

다음 단계는 API에 OAuth 2.0 사용자 권한 부여를 사용하도록 설정하는 것입니다. 이를 통해 개발자 콘솔에서 API를 호출하기 전에 사용자를 대신하여 액세스 토큰을 확보해야 한다는 것을 알 수 있습니다.

1. API Management 인스턴스로 이동하고 **API**로 이동합니다.

1. 보호하려는 API를 선택합니다. 예: `Echo API`.

1. **설정**으로 이동합니다.

1. **보안** 아래에서 **OAuth 2.0**을 선택하고 이전에 구성한 OAuth 2.0 서버를 선택합니다. 

1. **저장**을 선택합니다.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>개발자 포털에서 API 호출 성공

> [!NOTE]
> 이 섹션은 개발자 포털을 지원하지 않는 **소비** 계층에는 적용되지 않습니다.

이제 API에서 OAuth 2.0 사용자 권한 부여를 사용 하도록 설정 했으므로 개발자 콘솔은 API를 호출 하기 전에 사용자 대신 액세스 토큰을 얻습니다.

1. 개발자 포털에서 API 아래에 있는 작업으로 이동 하 여 **체험**을 선택 합니다. 개발자 콘솔로 이동됩니다.

1. 방금 추가한 권한 부여 서버에 해당하는 **권한 부여** 섹션의 새 항목을 참고합니다.

1. 권한 부여 드롭다운 목록에서 **권한 부여 코드**를 선택하면 Azure AD 테넌트에 로그인하라는 메시지가 표시됩니다. 이미 이 계정으로 로그인한 경우 메시지가 나타나지 않을 수 있습니다.

1. 성공적으로 로그인한 후 `Authorization` 헤더가 Azure AD의 액세스 토큰과 함께 요청에 추가됩니다. 다음은 샘플 토큰입니다(Base64로 인코딩됨).

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

1. **보내기** 를 선택 하 여 API를 성공적으로 호출 합니다.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>미리 요청 권한을 부여하도록 JWT 유효성 검사 정책 구성

이 때 사용자가 개발자 콘솔에서 호출을 시도하면 로그인하라는 메시지가 표시됩니다. 개발자 콘솔은 사용자를 대신 하 여 액세스 토큰을 가져오고 API에 대 한 요청에 토큰을 포함 합니다.

그러나 누군가가 토큰 또는 잘못 된 토큰 없이 API를 호출 하는 경우는 어떻게 되나요? 예를 들어 헤더 없이 API를 호출 하려고 하면 `Authorization` 호출이 계속 진행 됩니다. API Management가 이 시점에는 액세스 토큰의 유효성을 검사하지 않기 때문입니다. 백 엔드 API에 `Authorization` 헤더만 전달합니다.

[JWT 유효성 검사](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#ValidateJWT) 정책을 사용 하 여 들어오는 각 요청의 액세스 토큰에 대 한 유효성을 검사 하 여 API Management에서 요청을 사전 인증 합니다. 요청에 유효한 토큰이 없으면 API Management에서 차단합니다. 예를 들어의 정책 섹션에 다음 정책을 추가 합니다 `<inbound>` `Echo API` . 액세스 토큰에서 audience 클레임을 확인하고 토큰이 유효하지 않은 경우 오류 메시지를 반환합니다. 정책을 구성하는 방법에 대한 내용은 [정책 설정 또는 편집](https://docs.microsoft.com/azure/api-management/set-edit-policies)을 참조하세요.


```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

> [!NOTE]
> 이 `openid-config` URL은 v1 끝점에 해당 합니다. V2 끝점의 `openid-config` 경우 다음 URL을 사용 합니다.
>
> `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.

## <a name="build-an-application-to-call-the-api"></a>API를 호출하는 애플리케이션 빌드

이 가이드에서는 OAuth 2.0으로 보호되는 `Echo API`를 호출하기 위해 API Management에서 개발자 콘솔을 샘플 클라이언트 애플리케이션으로 사용했습니다. OAuth 2.0을 구현하고 애플리케이션을 빌드하는 방법을 자세히 알려보려면 [Azure Active Directory 코드 샘플](../active-directory/develop/sample-v2-code.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Active Directory 및 OAuth2.0](../active-directory/develop/authentication-scenarios.md)에 대해 자세히 알아봅니다.
- API Management에 대한 추가 [비디오](https://azure.microsoft.com/documentation/videos/index/?services=api-management) 를 확인합니다.
- 백 엔드 서비스를 보호하는 다른 방법은 [상호 인증서 인증](./api-management-howto-mutual-certificates.md)을 참조하세요.
- [API Management 서비스 인스턴스를 만듭니다](./get-started-create-service-instance.md).
- [첫 번째 API 관리](./import-and-publish.md)
