---
title: 'VPN Gateway: P2S VPN 연결에 대 한 Azure AD 테 넌 트: Azure AD 인증'
description: Azure AD 인증을 사용 하 여 VNet에 연결 하는 데 P2S VPN을 사용할 수 있습니다.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/17/2020
ms.author: alzam
ms.openlocfilehash: 2dda6cb84fc881b4ca628ff1cecdec7c00555e8b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85414313"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN 프로토콜 연결을 위한 Azure Active Directory 테넌트 만들기

VNet에 연결 하는 경우 인증서 기반 인증 또는 RADIUS 인증을 사용할 수 있습니다. 그러나 오픈 VPN 프로토콜을 사용 하는 경우 Azure Active Directory 인증을 사용할 수도 있습니다. 이 문서는 P2S 오픈 VPN 인증을 위해 Azure AD 테 넌 트를 설정 하는 데 도움이 됩니다.

> [!NOTE]
> Azure AD 인증은 OpenVPN® 프로토콜 연결에만 지원됩니다.
>


## <a name="1-verify-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD 테 넌 트 확인

Azure AD 테 넌 트가 있는지 확인 합니다. Azure AD 테 넌 트가 없는 경우 [새 테 넌 트 만들기](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 문서의 단계를 사용 하 여 만들 수 있습니다.

* 조직 구성 이름
* 초기 도메인 이름

예제:

   ![새 Azure AD 테넌트](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Azure AD 테 넌 트 사용자 만들기

Azure AD 테 넌 트에는 전역 관리자 계정 및 마스터 사용자 계정과 같은 계정이 필요 합니다. 마스터 사용자 계정은 마스터 임베딩 계정(서비스 계정)으로 사용됩니다. Azure AD 테넌트 사용자 계정을 만들 때 만들려는 사용자 유형에 대한 디렉터리 역할을 조정합니다.

[이 문서](../active-directory/fundamentals/add-users-azure-active-directory.md)의 단계를 사용하여 Azure AD 테넌트에 대해 두 명 이상의 사용자를 만듭니다. 계정 유형을 만들려면 **디렉터리 역할**을 변경해야 합니다.

* 전역 관리자
* 사용자

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. VPN 게이트웨이에서 Azure AD 인증을 사용 하도록 설정

1. 인증에 사용할 디렉터리의 디렉터리 ID를 찾습니다. Active Directory 페이지의 속성 섹션에 나열되어 있습니다.

    ![디렉터리 ID](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. 디렉터리 ID를 복사합니다.

3. **전역 관리자** 역할이 할당된 사용자로 Azure Portal에 로그인합니다.

4. 다음으로, 관리자 동의를 제공합니다. 브라우저의 주소 표시줄에서 배포 위치와 관련된 URL을 복사하여 붙여넣습니다.

    공용

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
   https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud 독일

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. 메시지가 표시되면 **전역 관리자** 계정을 선택합니다.

    ![디렉터리 ID](./media/openvpn-create-azure-ad-tenant/pick.png)

6. 메시지가 표시되면 **수락**을 선택합니다.

    ![수락](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Azure AD의 **엔터프라이즈 응용 프로그램**에는 나열 된 **azure VPN** 이 표시 됩니다.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)
    
8. 작동하는 지점-사이트 간 환경이 아직 없는 경우 지침에 따라 환경을 만듭니다. 지점 및 사이트 간 vpn 게이트웨이 만들기 및 구성에 대 한 [지점 및 사이트 간 Vpn 만들기](vpn-gateway-howto-point-to-site-resource-manager-portal.md) 를 참조 하세요. 

    > [!IMPORTANT]
    > 기본 SKU는 OpenVPN에서 지원되지 않습니다.

9. **지점 및 사이트 간 구성** 으로 이동 하 고 **터널 유형**으로 **openvpn (SSL)** 을 선택 하 여 VPN 게이트웨이에서 Azure AD 인증을 사용 하도록 설정 합니다. **인증 유형** 으로 **Azure Active Directory** 를 선택 하 고 **Azure Active Directory** 섹션 아래의 정보를 입력 합니다.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azure-ad-auth-portal.png)


   > [!NOTE]
   > 값의 끝에 후행 슬래시를 포함 해야 `AadIssuerUri` 합니다. 그렇지 않으면 연결이 실패할 수 있습니다.

10. **VPN 클라이언트 다운로드** 링크를 클릭 하 여 프로필을 만들고 다운로드 합니다.

11. 다운로드 한 zip 파일을 추출 합니다.

12. 압축을 푼 "AzureVPN" 폴더로 이동 합니다.

13. "azurevpnconfig.xml" 파일의 위치를 적어둡니다. azurevpnconfig.xml에는 VPN 연결에 대 한 설정이 포함 되어 있으며, Azure VPN 클라이언트 응용 프로그램으로 직접 가져올 수 있습니다. 또한 전자 메일 또는 다른 방법을 통해 연결 해야 하는 모든 사용자에 게이 파일을 배포할 수 있습니다. 성공적으로 연결 하려면 사용자에 게 유효한 Azure AD 자격 증명이 필요 합니다.

## <a name="next-steps"></a>다음 단계

가상 네트워크에 연결 하려면 VPN 클라이언트 프로필을 만들고 구성 해야 합니다. [P2S vpn 연결에 대 한 vpn 클라이언트 구성](openvpn-azure-ad-client.md)을 참조 하세요.
