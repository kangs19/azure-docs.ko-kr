---
title: Azure AD에서 레거시 인증 프로토콜 차단
description: 조직이 레거시 인증 프로토콜을 차단 하는 방법 및 이유 알아보기
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 1799f676e8971726832cc50598e119f029bc331d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83196385"
---
# <a name="blocking-legacy-authentication"></a>레거시 인증 차단
 
사용자가 클라우드 앱에 쉽게 액세스할 수 있도록 Azure AD(Active Directory)에서 레거시 인증을 포함한 다양한 인증 프로토콜을 지원합니다. 레거시 인증은 다음을 통해 수행된 인증 요청을 나타내는 용어입니다.

- 최신 인증을 사용 하지 않는 이전 Office 클라이언트 (예: Office 2010 클라이언트)
- IMAP/SMTP/POP3와 같은 레거시 메일 프로토콜을 사용 하는 모든 클라이언트

오늘날 대부분의 손상 된 로그인 시도는 대부분 레거시 인증에서 제공 됩니다. 레거시 인증은 MFA (multi-factor authentication)를 지원 하지 않습니다. 디렉터리에서 MFA 정책을 사용 하도록 설정한 경우에도 잘못 된 행위자는 레거시 프로토콜을 사용 하 여 인증 하 고 MFA를 우회할 수 있습니다. 레거시 프로토콜에서 수행 하는 악의적인 인증 요청 으로부터 계정을 보호 하는 가장 좋은 방법은 이러한 시도를 모두 차단 하는 것입니다.

## <a name="identify-legacy-authentication-use"></a>레거시 인증 사용 확인

디렉터리에서 레거시 인증을 차단하려면 먼저 사용자에게 레거시 인증을 사용하는 앱이 있는지, 이로 인해 전체 디렉터리에 어떤 영향을 미치는지 이해해야 합니다. Azure AD 로그인 로그를 사용하여 레거시 인증을 사용하고 있는지 파악할 수 있습니다.

1.  **Azure Portal**   >  **Azure Active Directory**   >  **로그인**으로 이동 합니다.
1.  **열**클라이언트 앱을 클릭 하 여 표시 되지 않는 경우 **클라이언트 앱** 열을 추가   >  **Client App**합니다.
1.  **클라이언트 앱** 을 기준으로 필터링  > 제공 된 모든 **레거시 인증 클라이언트** 옵션을 확인 합니다.
1. **상태별**로 필터링  >  **Success**합니다. 
1. **날짜 필터를** 사용 하 여 필요한 경우 날짜 범위를 확장 합니다.

필터링은 선택한 레거시 인증 프로토콜에서 수행한 성공적인 로그인 시도만 표시 합니다. 각 개별 로그인 시도를 클릭하면 추가 세부 정보가 표시됩니다. 데이터의 개별 행을 선택 하 고 나 서 기본 정보 탭의 클라이언트 앱 열 또는 클라이언트 앱 필드는 사용 된 레거시 인증 프로토콜을 표시 합니다. 이러한 로그에는 레거시 인증에 여전히 의존하는 사용자와 레거시 프로토콜을 사용하여 인증 요청을 수행하는 애플리케이션이 표시됩니다. 이러한 로그에 표시 되지 않고 레거시 인증을 사용 하지 않는 사용자의 경우 조건부 액세스 정책을 구현 하거나 기준 정책 사용: 해당 사용자에 대해서만 레거시 인증 차단을 선택 합니다.

## <a name="moving-away-from-legacy-authentication"></a>레거시 인증에서 밖으로 이동 

디렉터리에서 레거시 인증을 사용 하는 사용자와이에 종속 된 응용 프로그램을 잘 이해 했으면 다음 단계는 사용자가 최신 인증을 사용 하도록 업그레이드 하는 것입니다. 최신 인증은 보다 안전한 사용자 인증 및 권한 부여를 제공 하는 id 관리 방법입니다. 디렉터리에 MFA 정책이 있는 경우 최신 인증을 사용 하면 필요할 때 사용자에 게 MFA를 요청 하는 메시지가 표시 됩니다. 레거시 인증 프로토콜에 대 한 보다 안전한 대안입니다.

이 섹션에서는 최신 인증으로 환경을 업데이트 하는 방법에 대 한 단계별 개요를 제공 합니다. 조직에서 레거시 인증 차단 정책을 사용 하도록 설정 하기 전에 아래 단계를 참조 하세요.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>1 단계: 디렉터리에서 최신 인증을 사용 하도록 설정

최신 인증을 사용 하도록 설정 하는 첫 번째 단계는 디렉터리에서 최신 인증을 지원 하는지 확인 하는 것입니다. 최신 인증은 2017 년 8 월 1 일 이후에 생성 되는 디렉터리에 대해 기본적으로 사용 하도록 설정 됩니다. 이 날짜 이전에 디렉터리가 만들어진 경우 다음 단계를 사용 하 여 디렉터리에 대 한 최신 인증을 수동으로 사용 하도록 설정 해야 합니다.

1.  `Get-CsOAuthConfiguration`    [비즈니스용 Skype Online PowerShell 모듈](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell)에서를 실행 하 여 디렉터리에서 최신 인증을 이미 지원 하는지 확인 합니다.
1. 명령에서 빈  `OAuthServers`   속성을 반환 하는 경우 최신 인증을 사용할 수 없습니다. 을 사용 하 여 최신 인증을 사용 하도록 설정을 업데이트  `Set-CsOAuthConfiguration` 합니다. 속성에  `OAuthServers`   항목이 포함 되어 있으면 이동 하는 것이 좋습니다.

앞으로 이동 하기 전에이 단계를 완료 해야 합니다. 모든 Office 클라이언트에서 사용할 프로토콜을 결정 하기 때문에 디렉터리 구성이 먼저 변경 되는 것이 중요 합니다. 최신 인증을 지 원하는 Office 클라이언트를 사용 하는 경우에도 디렉터리에서 최신 인증을 사용 하지 않도록 설정 하면 기존 프로토콜을 사용 하는 것이 기본적으로 사용 됩니다.

### <a name="step-2-office-applications"></a>2 단계: Office 응용 프로그램

디렉터리에서 최신 인증을 사용 하도록 설정한 후에는 Office 클라이언트에 대 한 최신 인증을 사용 하도록 설정 하 여 응용 프로그램 업데이트를 시작할 수 있습니다. Office 2016 이상 클라이언트는 기본적으로 최신 인증을 지원 합니다. 추가 단계가 필요 하지 않습니다.

Office 2013 Windows 클라이언트 또는 이전 버전을 사용 하는 경우 Office 2016 이상으로 업그레이드 하는 것이 좋습니다. 디렉터리에서 최신 인증을 사용 하도록 설정 하는 이전 단계를 완료 한 후에도 이전 Office 응용 프로그램은 계속 레거시 인증 프로토콜을 사용 합니다. Office 2013 클라이언트를 사용 하 고 Office 2016 이상으로 즉시 업그레이드할 수 없는 경우 [Windows 장치에서 office 2013에 대 한 최신 인증을 사용 하도록 설정](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication)하려면 다음 문서의 단계를 수행 합니다. 레거시 인증을 사용 하는 동안 계정을 보호 하려면 디렉터리에서 강력한 암호를 사용 하는 것이 좋습니다. 디렉터리에서 weak 암호를 금지 하려면 [AZURE AD 암호 보호](../authentication/concept-password-ban-bad.md)   를 확인 하세요.

Office 2010은 최신 인증을 지원 하지 않습니다. Office 2010를 사용 하는 모든 사용자를 최신 버전의 Office로 업그레이드 해야 합니다. 기본적으로 레거시 인증을 차단 하므로 Office 2016 이상으로 업그레이드 하는 것이 좋습니다.

MacOS를 사용 하는 경우 Mac 2016 이상용 Office로 업그레이드 하는 것이 좋습니다. 네이티브 메일 클라이언트를 사용 하는 경우 모든 장치에서 macOS 버전 10.14 이상을 사용 해야 합니다.

### <a name="step-3-exchange-and-sharepoint"></a>3 단계: Exchange 및 SharePoint

Windows 기반 Outlook 클라이언트에서 최신 인증을 사용 하려면 Exchange Online도 최신 인증을 사용 하도록 설정 해야 합니다. Exchange Online에 대해 최신 인증을 사용 하지 않도록 설정한 경우 최신 인증 (Outlook 2013 이상)을 지 원하는 Windows 기반 Outlook 클라이언트는 기본 인증을 사용 하 여 Exchange Online 사서함에 연결 합니다.

최신 인증 기본값에 대해 SharePoint Online을 사용 하도록 설정 합니다. 2017 년 8 월 1 일 이후에 생성 된 디렉터리의 경우 최신 인증은 Exchange Online에서 기본적으로 사용 하도록 설정 됩니다. 그러나 이전에 최신 인증을 사용 하지 않도록 설정 했거나이 날짜 이전에 만든 디렉터리를 사용 하는 경우 [Exchange Online에서 최신 인증을 사용 하도록 설정](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online)하려면 다음 문서의 단계를 따르세요.

### <a name="step-4-skype-for-business"></a>4 단계: 비즈니스용 Skype

비즈니스용 Skype에서 수행 하는 레거시 인증 요청을 방지 하려면 비즈니스용 Skype Online에 대 한 최신 인증을 사용 하도록 설정 해야 합니다. 2017 년 8 월 1 일 이후에 만들어진 디렉터리의 경우 비즈니스용 Skype에 대 한 최신 인증을 기본적으로 사용할 수 있습니다.

최신 인증을 기본적으로 지 원하는 Microsoft 팀으로 전환 하는 것이 좋습니다. 그러나 지금은 마이그레이션할 수 없는 경우 비즈니스용 skype Online에 대 한 최신 인증을 사용 하도록 설정 하 여 비즈니스용 Skype 클라이언트에서 최신 인증을 사용 하기 시작 해야 합니다.  [최신 인증을 사용 하 여 지원 되는 비즈니스용 skype 토폴로지에 대 한](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported)이 문서의 단계에 따라 비즈니스용 skype에 대 한 최신 인증을 사용 하도록 설정 합니다.

비즈니스용 skype Online에 대 한 최신 인증을 사용 하도록 설정 하는 것 외에도 비즈니스용 Skype에 최신 인증을 사용 하도록 설정 하는 경우 Exchange Online에 대 한 최신 인증을 사용 하도록 설정 이 프로세스는 Exchange Online 및 비즈니스용 Skype online에서 최신 인증의 상태를 동기화 하는 데 도움이 되며 비즈니스용 Skype 클라이언트에 대 한 여러 로그인 프롬프트를 방지 합니다.

### <a name="step-5-using-mobile-devices"></a>5 단계: 모바일 장치 사용

모바일 장치의 응용 프로그램 에서도 레거시 인증을 차단 해야 합니다. 모바일을 위해 Outlook을 사용 하는 것이 좋습니다. 모바일 용 Outlook은 기본적으로 최신 인증을 지원 하 고 다른 MFA 기준 보호 정책을 충족 합니다.

네이티브 iOS 메일 클라이언트를 사용 하려면 iOS 버전 11.0 이상을 실행 하 여 메일 클라이언트가 레거시 인증을 차단 하도록 업데이트 되었는지 확인 해야 합니다.

### <a name="step-6-on-premises-clients"></a>6 단계: 온-프레미스 클라이언트

온-프레미스 Exchange Server와 온-프레미스 비즈니스용 Skype를 사용 하는 하이브리드 고객 인 경우 최신 인증을 사용 하도록 두 서비스를 업데이트 해야 합니다. 하이브리드 환경에서 최신 인증을 사용 하는 경우에도 온-프레미스에서 사용자를 인증 하는 것입니다. 리소스 (파일 또는 전자 메일)에 대 한 액세스 권한을 부여 하는 영역입니다.

온-프레미스에서 최신 인증을 사용 하려면 먼저 필수 구성 요소를 충족 하는지 확인 하세요. 이제 온-프레미스에서 최신 인증을 사용할 준비가 되었습니다.

최신 인증을 사용 하도록 설정 하는 단계는 다음 문서에서 찾을 수 있습니다.

* [하이브리드 최신 인증을 사용 하도록 Exchange Server 온-프레미스를 구성 하는 방법](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [비즈니스용 Skype에서 최신 인증 (ADAL)을 사용 하는 방법](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="next-steps"></a>다음 단계

- [하이브리드 최신 인증을 사용 하도록 Exchange Server 온-프레미스를 구성 하는 방법](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
- [비즈니스용 Skype에서 최신 인증 (ADAL)을 사용 하는 방법](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)
- [레거시 인증 차단](../conditional-access/block-legacy-authentication.md)
