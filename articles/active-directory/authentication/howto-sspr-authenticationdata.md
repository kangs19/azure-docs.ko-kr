---
title: Azure AD SSPR 데이터 요구 사항-Azure Active Directory
description: Azure AD 셀프 서비스 암호 재설정의 데이터 요구 사항 및 충족 방법
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 12/09/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42f7e120745357d3bd5735cca568bdd6971ea061
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80652351"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>최종 사용자를 등록할 필요 없이 암호 재설정 배포

Azure Active Directory(Azure AD) 셀프 서비스 암호 재설정(SSPR)을 배포하려면 인증 데이터가 있어야 합니다. 일부 조직에서는 사용자들이 직접 자신의 인증 데이터를 입력하도록 하지만 다른 조직에서는 Active Directory에 이미 있는 데이터와 동기화 하는 것을 선호 합니다. 이 동기화 된 데이터는 다음 요구 사항을 충족 하는 경우 사용자 개입 없이 Azure AD 및 SSPR에서 사용할 수 있게 됩니다.

* 온-프레미스 디렉터리에 데이터가 올바른 형식으로 저장되어 있습니다.
* [기본 설정을 사용하여 Azure AD Connect](../hybrid/how-to-connect-install-express.md)가 구성되어 있습니다.

올바르게 작동하려면 전화 번호가 *+국가코드 전화번호* 형식으로 저장되어야 합니다(예: +1 4255551234).

> [!NOTE]
> 국가 번호와 전화 번호 사이에 공백이 필요합니다.
>
> 암호 재설정은 전화 번호 확장을 지원하지 않습니다. +1 4255551234X12345 형식에서도 전화를 걸지 전에 확장이 제거됩니다.

## <a name="fields-populated"></a>채워진 필드

Azure AD Connect에서 기본 설정을 사용한 경우 다음과 같은 매핑이 수행됩니다.

| 온-프레미스 Active Directory | Azure AD |
| --- | --- |
| telephoneNumber | 사무실 전화 |
| mobile | 휴대폰 |

사용자가 휴대폰 번호를 확인 한 후에 Azure AD의 **인증 연락처 정보** 에 있는 *전화* 필드도 해당 숫자로 채워집니다.

## <a name="authentication-contact-info"></a>인증 연락처 정보

Azure Portal에서 Azure AD 사용자에 대 한 **인증 방법** 페이지에서 전역 관리자는 다음 예제 스크린샷에 표시 된 것 처럼 인증 연락처 정보를 수동으로 설정할 수 있습니다.

![Azure AD의 사용자에 대 한 인증 연락처 정보][Contact]

* **휴대폰** 필드가 채워지고 SSPR 정책에서 **휴대폰** 필드가 사용 하도록 설정 된 경우 사용자는 암호 재설정 등록 페이지 및 암호 재설정 워크플로 중에 해당 번호를 확인 합니다.
* **대체 전화** 필드는 암호 재설정에 사용 되지 않습니다.
* **전자 메일** 필드가 채워지고 SSPR 정책에서 **전자 메일** 을 사용 하도록 설정 하면 사용자가 암호 재설정 등록 페이지 및 암호 재설정 워크플로 중에 해당 전자 메일을 볼 수 있습니다.
* **대체 전자 메일** 필드가 채워지고 SSPR 정책에서 **전자 메일** 을 사용 하는 경우 사용자는 암호 재설정 등록 페이지에 해당 전자 메일이 표시 **되지** 않지만 암호 재설정 워크플로 중에 해당 전자 메일이 표시 됩니다.

## <a name="security-questions-and-answers"></a>보안 질문 및 답변

본인 확인 질문과 답변은 Azure AD 테넌트에 안전하게 저장되며 사용자가 [SSPR 등록 포털](https://aka.ms/ssprsetup)을 통해서만 액세스할 수 있습니다. 관리자는 다른 사용자의 질문과 답변 내용을 보거나, 설정하거나, 수정할 수 없습니다.

## <a name="what-happens-when-a-user-registers"></a>사용자가 등록하면 어떻게 되나요?

사용자가 등록하면 등록 페이지에서 다음 필드를 설정합니다.

* **인증 전화**
* **인증 전자 메일**
* **보안 질문 및 답변**

**휴대폰** 또는 **대체 전자 메일**에 대 한 값을 제공 하는 경우 사용자는 서비스에 등록 하지 않은 경우에도 해당 값을 사용 하 여 암호를 다시 설정할 수 있습니다. 또한 사용자는 처음으로 등록할 때 해당 값을 볼 수 있고, 원하는 경우 값을 변경할 수도 있습니다. 등록을 성공적으로 완료 한 후에는 **인증 전화** 및 **인증 전자 메일** 필드에 각각 해당 값이 유지 됩니다.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>PowerShell을 사용하여 인증 데이터 설정 및 읽기

PowerShell을 사용하여 다음 필드를 설정할 수 있습니다.

* **대체 전자 메일**
* **휴대폰**
* **사무실 전화**: 온-프레미스 디렉터리와 동기화하지 않는 경우에만 설정할 수 있습니다.

### <a name="use-powershell-version-1"></a>PowerShell 버전 1 사용

시작하려면 [Azure AD PowerShell 모듈을 다운로드하고 설치](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)해야 합니다. 설치를 완료한 후에는 다음 단계에 따라서 각 필드를 구성합니다.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell 버전 1을 사용하여 인증 데이터 설정

```PowerShell
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>PowerShell 버전 1을 사용하여 인증 데이터 읽기

```PowerShell
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>인증 전화 및 인증 메일 옵션 읽기

PowerShell 버전 1에서 아래 명령을 사용하여 **인증 전화**와 **인증 메일**을 읽습니다.

```PowerShell
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>PowerShell 버전 2 사용

시작하려면 [Azure AD 버전 2 PowerShell 모듈을 다운로드하여 설치](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0)합니다. 설치를 완료한 후에는 다음 단계에 따라서 각 필드를 구성합니다.

아래 명령을 사용하면 Install-Module을 지원하는 최신 버전의 PowerShell에서 빠르게 설치할 수 있습니다. (첫 줄에서는 모듈이 이미 설치되어 있는지 확인합니다.)

```PowerShell
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>PowerShell 버전 2를 사용하여 인증 데이터 설정

```PowerShell
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>PowerShell 버전 2를 사용하여 인증 데이터 읽기

```PowerShell
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>다음 단계

* [성공적인 SSPR 롤아웃을 어떻게 완료합니까?](howto-sspr-deployment.md)
* [암호 재설정 또는 변경](../user-help/active-directory-passwords-update-your-own-password.md)
* [셀프 서비스 암호 재설정 등록](../user-help/active-directory-passwords-reset-register.md)
* [라이선스 관련 질문이 있습니까?](concept-sspr-licensing.md)
* [사용자가 사용할 수 있는 인증 방법은 무엇입니까?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR에서 사용하는 정책 옵션은 무엇입니까?](concept-sspr-policy.md)
* [비밀번호 쓰기 저장은 무엇이며, 왜 관심을 가져야 합니까?](howto-sspr-writeback.md)
* [SSPR 작업은 어떻게 보고 합니까?](howto-sspr-reporting.md)
* [모든 SSPR 옵션과 그 의미는 무엇입니까?](concept-sspr-howitworks.md)
* [무엇인가 손상된 문제가 있습니다. SSPR 문제는 어떻게 해결합니까?](active-directory-passwords-troubleshoot.md)
* [다른 곳에서 다루지 않았던 질문이 있습니다.](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "전역 관리자는 사용자의 인증 연락처 정보를 수정할 수 있습니다."
