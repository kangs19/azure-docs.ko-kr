---
title: 보안 정보를 사용하여 암호 재설정 - Azure Active Directory | Microsoft Docs
description: 암호를 잊어버린 경우 보안 정보 및 2단계 인증을 사용하여 암호를 재설정하는 방법을 설명합니다.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 05/28/2020
ms.author: curtand
ms.openlocfilehash: 58ec2c00e75b12d6010b106ca7daed0da234bf1d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84266120"
---
# <a name="reset-your-work-or-school-password-using-security-info"></a>보안 정보를 사용하여 회사 또는 학교 암호 재설정

회사 또는 학교 암호가 생각 나지 않거나, 조직으로부터 암호를 받은 적이 없거나, 계정이 잠긴 경우 보안 정보 및 모바일 디바이스를 사용하여 회사 또는 학교 암호를 재설정할 수 있습니다. 관리자는 사용자가 정보를 설정하고 [암호를 재설정](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-passwords-reset-register)할 수 있도록 이 기능을 켜야 합니다.

암호를 알고 있는 경우 암호를 변경 하려면이 문서의 [암호 단계 변경](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-passwords-update-your-own-password#how-to-change-your-password) 섹션을 참조 하세요.

>[!Important]
>이 문서는 분실했거나 알 수 없는 회사 또는 학교 계정 암호를 재설정하려는 사용자를 위해 작성되었습니다. 직원 또는 다른 사용자에 대해 셀프 서비스 암호 재설정을 켜는 방법을 원하는 관리자는 [Azure AD 셀프 서비스 암호 재설정 배포 및 기타 문서](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment)를 참조하세요.

## <a name="how-to-reset-or-unlock-your-password-for-a-work-or-school-account"></a>회사 또는 학교 계정의 암호를 재설정 또는 잠금 해제하는 방법

Azure AD(Azure Active Directory) 계정에 액세스할 수 없는 경우 원인은 다음 둘 중 하나일 수 있습니다.

- 암호가 작동하지 않아 암호를 재설정하려고 합니다.

- 암호를 알지만 계정이 잠겨 있어 계정을 잠금 해제하려고 합니다.

### <a name="to-reset-your-password-and-get-back-into-your-account"></a>암호를 재설정하고 계정으로 돌아가려면

1. **암호 입력** 화면에서 **암호를 잊어버렸습니다**를 선택합니다.

2. **계정으로 돌아가기** 화면에서 회사 또는 학교 **사용자 ID**(예: 이메일 주소)를 입력하고, 화면에 표시되는 문자를 입력하여 로봇이 아님을 증명하고, **다음**을 선택합니다.

   ![계정으로 돌아가기 화면](media/security-info/security-info-back-into-acct.png)

   >[!NOTE]
   >관리자가 암호 재설정 기능을 설정하지 않은 경우 **계정으로 돌아가기** 화면 대신 **관리자에게 문의** 링크가 표시됩니다. 이 링크를 사용하면 이메일 또는 웹 포털을 통해 관리자에게 암호 재설정 방법을 문의할 수 있습니다.

3. 다음 중에서 본인 여부를 확인하고 암호 변경에 사용할 방법을 선택합니다. 관리자가 조직을 설정한 방식에 따라 이 프로세스를 두 번째로 진행하여 두 번째 인증 단계에 대한 정보를 추가해야 할 수도 있습니다.

    ![계정으로 돌아가기, 인증 단계 #1](media/security-info/security-info-back-into-acct2.png)

    >[!NOTE]
    >관리자가 조직을 설정한 방식에 따라 이러한 인증 옵션 중 일부를 사용하지 못할 수도 있습니다. 이전에 다음 방법 중 하나 이상을 사용하여 인증에 사용할 모바일 디바이스를 설정해 놓았어야 합니다.<br><br>또한 새 암호가 특정 강도 요구 사항을 충족해야 합니다. 강력한 암호는 일반적으로 대문자 및 소문자, 하나 이상의 숫자, 하나 이상의 특수 문자가 포함된 8~16자입니다.

- **이메일 주소를 사용하여 암호 재설정.** 이전에 2단계 인증 또는 보안 정보에서 설정한 이메일 주소로 이메일을 보냅니다. 관리자가 보안 정보 환경을 설정한 경우 [이메일을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-email.md) 문서에서 이메일 주소 설정에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 이메일 주소 설정에 대해 자세히 알아볼 수 있습니다. 

    1. **내 암호 확인용 이메일 주소로 이메일 보내기**를 선택한 다음, **이메일**을 선택합니다.

    2. 이메일의 확인 코드를 입력하고 **다음**을 선택합니다.

    3. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

- **문자 메시지를 사용하여 암호 재설정.** 이전에 보안 정보에서 설정한 전화 번호로 문자 메시지를 보냅니다. 관리자가 보안 정보 환경을 설정한 경우 [문자 메시지를 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-text-msg.md) 문서에서 문자 메시지 설정에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 문자 메시지 설정에 대해 자세히 알아볼 수 있습니다.

    1. **휴대폰으로 문자 보내기**를 선택하고, 전화 번호를 입력하고, **문자**를 선택합니다.

    2. 문자 메시지의 확인 코드를 상자에 입력하고 **다음**을 선택합니다.

    3. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

- **전화 번호를 사용하여 암호 재설정.** 이전에 보안 정보에서 설정한 전화 번호로 문자 메시지를 보냅니다. 관리자가 보안 정보 환경을 설정한 경우 [전화 통화를 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-phone-number.md) 문서에서 전화 번호 설정에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 전화 번호 설정에 대해 자세히 알아볼 수 있습니다.

    1. **내 휴대폰으로 전화 걸기**를 선택하고, 전화 번호를 입력하고, **호출**을 선택합니다.

    2. 전화를 받아서 지침에 따라 신원을 확인한 후 **다음**을 선택합니다.

    3. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

- **보안 질문을 사용하여 암호 재설정.** 보안 정보에서 설정한 보안 질문 목록이 표시됩니다. 관리자가 보안 정보 환경을 설정한 경우 [미리 정의된 보안 질문을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-questions.md) 문서에서 보안 질문 설정에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 보안 질문 설정에 대해 자세히 알아볼 수 있습니다.

    1. **보안 질문에 답하기**를 선택하고, 질문에 대답하고, **다음**을 선택합니다.

    2. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

- **Authenticator 앱에서 알림을 사용하여 암호 재설정.** Authenticator 앱에 승인 알림을 보냅니다. 관리자가 보안 정보 환경을 설정한 경우 [인증 앱을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-auth-app.md) 문서에서 Authenticator 앱 설정에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 Authenticator 앱 설정에 대해 자세히 알아볼 수 있습니다.

    1. **Authenticator 앱에서 알림 승인**을 선택하고 **알림 보내기**를 선택합니다.

    2. Authenticator 앱에서 로그인을 승인합니다.

    3. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

- **Authenticator 앱의 코드를 사용하여 암호 재설정.** 인증 앱에서 제공한 임의의 코드를 수락합니다. 관리자가 보안 정보 환경을 설정한 경우 [인증 앱을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-auth-app.md) 문서에서 코드를 제공하도록 Authenticator 앱을 설정하는 방법에 대해 자세히 알아볼 수 있습니다. 아직 보안 정보를 사용하지 않는 경우 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md) 문서에서 코드를 제공하도록 Authenticator 앱을 설정하는 방법에 대해 자세히 알아볼 수 있습니다.

  1. **Authenticator 앱의 코드 입력**을 선택하고 **알림 보내기**를 선택합니다.

  2. Authenticator 앱을 열고, 계정 확인 코드를 상자에 입력하고, **다음**을 선택합니다.

  3. 새 암호를 입력하고 확인한 다음, **완료**를 선택합니다.

  4. 암호가 재설정되었다는 메시지가 표시되면 새 암호를 사용하여 계정에 로그인 할 수 있습니다.

     여전히 계정에 액세스할 수 없는 경우 조직의 관리자에게 연락하여 도움을 요청해야 합니다.

암호를 재설정 한 후에 "Microsoft를 대신 하 여"와 같은 계정에서 확인 전자 메일을 받을 수 있습니다 \<*your_organization*> . 최근에 암호를 재설정하지 않았는데도 이와 비슷한 이메일을 받으면 즉시 조직의 관리자에게 알려야 합니다.

## <a name="how-to-change-your-password"></a>암호를 변경하는 방법

단순히 암호를 변경하려는 경우 Office 365 포털, Azure 액세스 패널 또는 Windows 10 로그인 페이지를 통해 변경할 수 있습니다.

### <a name="to-change-your-password-using-the-office-365-portal"></a>Office 365 포털을 사용하여 암호를 변경하려면

주로 Office 포털을 통해 앱에 액세스하는 경우 이 방법을 사용하세요.

1. 기존 암호를 사용하여 [Office 365 계정](https://portal.office.com)에 로그인합니다.

2. 오른쪽 위에서 자신의 프로필을 선택한 다음 **계정 보기**를 선택합니다.

3. **보안 및 개인 정보 보호** > **암호**를 선택합니다.

4. 이전 암호를 입력하고, 새 암호를 만들고 확인한 후 **제출**을 선택합니다.

### <a name="to-change-your-password-from-the-azure-access-panel"></a>Azure 액세스 패널에서 암호를 변경하려면

주로 Azure 액세스 패널(MyApps)에서 앱에 액세스하는 경우 이 방법을 사용하세요.

1. 기존 암호를 사용하여 [Azure 액세스 패널](https://myapps.microsoft.com/)에 로그인합니다.

2. 오른쪽 위에서 자신의 프로필을 선택한 다음 **프로필**을 선택합니다.

3. **암호 변경**을 선택합니다.

4. 이전 암호를 입력하고, 새 암호를 만들고 확인한 후 **제출**을 선택합니다.

### <a name="to-change-your-password-at-windows-sign-in"></a>Windows 로그인 시 암호를 변경하려면

관리자가 이 기능을 설정한 경우 Windows 7, Windows 8, Windows 8.1 또는 Windows 10 로그인 화면에 **암호 재설정** 링크가 보입니다.

1. 일반 웹 기반 환경을 사용할 필요 없이 **암호 재설정** 링크를 선택하여 암호 재설정 프로세스를 시작합니다.

2. 사용자 ID를 확인하고 **다음**을 선택합니다.

3. 확인을 위해 연락 방법을 선택하고 확인합니다. 필요한 경우 첫 번째 확인 옵션과 다른 두 번째 확인 옵션을 선택하고 필요한 정보를 입력합니다.

4. **새 암호 만들기** 페이지에서 새 암호를 입력하고 확인한 후 **다음**을 선택합니다.

    강력한 암호는 일반적으로 대문자 및 소문자, 하나 이상의 숫자, 하나 이상의 특수 문자가 포함된 8~16자입니다.

5. 암호가 재설정되었다는 메시지가 표시되면 **완료**를 선택할 수 있습니다.

    여전히 계정에 액세스할 수 없는 경우 조직의 관리자에게 연락하여 도움을 요청해야 합니다.

## <a name="common-problems-and-their-solutions"></a>일반적인 문제 및 해결 방법

다음은 일반적인 오류 사례 및 해결 방법입니다.

|문제|Description|해결 방법|
| --- | --- | --- |
|암호를 변경하려고 하면 오류가 발생합니다. |암호에 쉽게 추측할 수 있는 단어, 구 또는 패턴이 포함되어 있습니다.| 더 강력한 암호를 사용하여 다시 시도하세요.|
|사용자 ID를 입력하면 "관리자에 문의하세요"라는 페이지로 이동됩니다.|Microsoft는 해당 사용자 계정 암호가 온-프레미스 환경의 관리자를 통해 관리되는 것을 확인했습니다. 따라서 "계정에 액세스할 수 없음" 링크에서 암호를 재설정할 수 없습니다. |관리자에게 문의하여 도움을 받으세요.|
|사용자 ID를 입력하면 "암호 재설정을 위해 계정을 사용할 수 없습니다" 오류가 발생합니다.|사용자가 암호를 재설정할 수 있도록 관리자가 계정을 설정하지 않았습니다.|관리자가 "계정에 액세스할 수 없음" 링크에서 조직에 대해 암호 재설정을 활성화하지 않았거나 기능을 사용하도록 허가하지 않았기 때문입니다.<br><br> 암호를 재설정하려면 "관리자에게 문의" 링크를 선택하고 회사의 관리자에게 이메일을 보내서 암호를 재설정하려 한다고 알려야 합니다.|
|사용자 ID를 입력한 후 "계정을 확인할 수 없습니다" 오류가 발생합니다.|로그인 프로세스에서 계정 정보를 확인할 수 없었습니다.|두 가지 이유로 이 메시지가 표시될 수 있습니다.<br><br>1. 관리자가 조직에 대해 암호 재설정 기능을 설정했지만, 사용자가 서비스를 사용하도록 등록하지 않았습니다. 암호 재설정을 등록하려면 확인 방법에 따라 다음 문서 중 하나를 참조하세요. [Authenticator 앱을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-auth-app.md), [전화 통화를 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-phone-number.md), [문자 메시지를 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-text-msg.md), [이메일을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-email.md) 또는 [보안 질문을 사용하도록 보안 정보 설정(미리 보기)](security-info-setup-questions.md).<br><br>2. 관리자가 조직에 대해 암호 재설정을 설정하지 않았습니다. 이 경우 "관리자에게 문의" 링크를 선택하여 관리자에게 암호 재설정을 요청하는 이메일을 보내야 합니다.|

## <a name="next-steps"></a>다음 단계

- [보안 정보(미리 보기) 개요](user-help-security-info-overview.md) 문서에서 보안 정보에 대해 자세히 알아봅니다.

- Xbox, hotmail.com 또는 outlook.com과 같은 개인 계정에 다시 액세스하려면 [Microsoft 계정에 로그인할 수 없는 경우](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) 문서의 권장 사항을 시도해 보세요.
