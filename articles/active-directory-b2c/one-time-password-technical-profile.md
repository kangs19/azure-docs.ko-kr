---
title: OTP (일회용 암호) 확인 사용
titleSuffix: Azure AD B2C
description: Azure AD B2C 사용자 지정 정책을 사용 하 여 OTP (일회용 암호) 시나리오를 설정 하는 방법에 대해 알아봅니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9592afbf74e65bcb2fe9319da764bf06d8d4eb6c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385725"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C 사용자 지정 정책에서 일회성 암호 기술 프로필을 정의 합니다.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C)는 일회성 암호의 생성 및 유효성 검사를 관리 하는 기능을 제공 합니다. 기술 프로필을 사용 하 여 코드를 생성 한 다음 나중에 해당 코드를 확인 합니다.

일회용 암호 기술 프로필은 코드 확인 중에 오류 메시지를 반환할 수도 있습니다. **유효성 검사 기술 프로필**을 사용 하 여 일회용 암호와의 통합을 설계 합니다. 유효성 검사 기술 프로필은 일회용 암호 기술 프로필을 호출 하 여 코드를 확인 합니다. 유효성 검사 기술 프로필은 사용자 경험이 계속되기 전에 사용자가 제공한 데이터의 유효성을 검사합니다. 유효성 검사 기술 프로필을 사용 하면 자체 어설션된 페이지에 오류 메시지가 표시 됩니다.

## <a name="protocol"></a>프로토콜

**Protocol** 요소의 **Name** 특성은 `Proprietary`로 설정해야 합니다. **Handler** 특성은 Azure AD B2C에서 사용 하는 프로토콜 처리기 어셈블리의 정규화 된 이름을 포함 해야 합니다.

```xml
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

다음 예에서는 일회성 암호 기술 프로필을 보여 줍니다.

```xml
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>코드 생성

이 기술 프로필의 첫 번째 모드는 코드를 생성 하는 것입니다. 이 모드에 대해 구성할 수 있는 옵션은 다음과 같습니다.

### <a name="input-claims"></a>입력 클레임

**Inputclaims** 요소는 일회용 암호 프로토콜 공급자에 게 보내는 데 필요한 클레임 목록을 포함 합니다. 클레임의 이름을 아래 정의 된 이름에 매핑할 수도 있습니다.

| ClaimReferenceId | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| identifier | 예 | 코드를 나중에 확인 해야 하는 사용자를 식별 하는 식별자입니다. 일반적으로 전자 메일 주소 또는 전화 번호와 같이 코드가 전달 되는 대상의 식별자로 사용 됩니다. |

**InputClaimsTransformations** 요소에는 일회용 암호 프로토콜 공급자로 보내기 전에 입력 클레임을 수정 하거나 새로 생성 하는 데 사용 되는 **InputClaimsTransformation** 요소의 컬렉션이 포함 될 수 있습니다.

### <a name="output-claims"></a>출력 클레임

**Outputclaims** 요소는 일회용 암호 프로토콜 공급자가 생성 하는 클레임 목록을 포함 합니다. 클레임의 이름을 아래 정의 된 이름에 매핑할 수도 있습니다.

| ClaimReferenceId | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| otpGenerated | 예 | Azure AD B2C에서 세션을 관리 하는 생성 된 코드입니다. |

**OutputClaimsTransformations** 요소는 출력 클레임을 수정하거나 새 출력 클레임을 생성하는 데 사용되는 **OutputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

### <a name="metadata"></a>메타데이터

다음 설정은 코드 생성 모드를 구성 하는 데 사용할 수 있습니다.

| 특성 | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| CodeExpirationInSeconds | 아니요 | 코드 만료 까지의 시간 (초)입니다. 최소: `60` ; 최대: `1200` ; 기본값은 `600` 입니다. |
| CodeLength | 아니요 | 코드의 길이입니다. 기본값은 `6`입니다. |
| CharacterSet | 아니요 | 정규식에서 사용 하기 위해 형식이 지정 된 코드에 대 한 문자 집합입니다. 정의합니다(예: `a-z0-9A-Z`). 기본값은 `0-9`입니다. 문자 집합은 지정 된 집합에 최소 10 개의 다른 문자를 포함 해야 합니다. |
| NumRetryAttempts | 아니요 | 코드가 잘못 된 것으로 간주 되기 전의 확인 시도 횟수입니다. 기본값은 `5`입니다. |
| 연산 | 예 | 수행할 작업입니다. 가능한 값: `GenerateCode` . |
| ReuseSameCode | 아니요 | 지정 된 코드가 만료 되지 않고 여전히 유효한 경우 새 코드를 생성 하는 대신 중복 코드를 지정 해야 하는지 여부입니다. 기본값은 `false`입니다. |

### <a name="example"></a>예제

다음 예제는 `TechnicalProfile` 코드를 생성 하는 데 사용 됩니다.

```xml
<TechnicalProfile Id="GenerateCode">
  <DisplayName>Generate Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">GenerateCode</Item>
    <Item Key="CodeExpirationInSeconds">600</Item>
    <Item Key="CodeLength">6</Item>
    <Item Key="CharacterSet">0-9</Item>
    <Item Key="NumRetryAttempts">5</Item>
    <Item Key="ReuseSameCode">false</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpGenerated" />
  </OutputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>코드 확인

이 기술 프로필의 두 번째 모드는 코드를 확인 하는 것입니다. 이 모드에 대해 구성할 수 있는 옵션은 다음과 같습니다.

### <a name="input-claims"></a>입력 클레임

**Inputclaims** 요소는 일회용 암호 프로토콜 공급자에 게 보내는 데 필요한 클레임 목록을 포함 합니다. 클레임의 이름을 아래 정의 된 이름에 매핑할 수도 있습니다.

| ClaimReferenceId | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| identifier | 예 | 이전에 코드를 생성 한 사용자를 식별 하는 식별자입니다. 일반적으로 전자 메일 주소 또는 전화 번호와 같이 코드가 전달 되는 대상의 식별자로 사용 됩니다. |
| otpToVerify | 예 | 사용자가 제공한 확인 코드입니다. |

**InputClaimsTransformations** 요소에는 일회용 암호 프로토콜 공급자로 보내기 전에 입력 클레임을 수정 하거나 새로 생성 하는 데 사용 되는 **InputClaimsTransformation** 요소의 컬렉션이 포함 될 수 있습니다.

### <a name="output-claims"></a>출력 클레임

이 프로토콜 공급자의 코드를 확인 하는 동안 제공 된 출력 클레임이 없습니다.

**OutputClaimsTransformations** 요소는 출력 클레임을 수정하거나 새 출력 클레임을 생성하는 데 사용되는 **OutputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

### <a name="metadata"></a>메타데이터

다음 설정을 사용 하 여 코드 확인 모드를 설정할 수 있습니다.

| 특성 | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| 연산 | 예 | 수행할 작업입니다. 가능한 값: `VerifyCode` . |


### <a name="ui-elements"></a>UI 요소

다음 메타 데이터를 사용 하 여 코드 확인 실패 시 표시 되는 오류 메시지를 구성할 수 있습니다. 메타 데이터는 [자체 어설션된](self-asserted-technical-profile.md) 기술 프로필에서 구성 해야 합니다. 오류 메시지는 [지역화](localization-string-ids.md#one-time-password-error-messages)될 수 있습니다.

| 특성 | 필요한 공간 | 설명 |
| --------- | -------- | ----------- |
| UserMessageIfSessionDoesNotExist | 아니요 | 코드 확인 세션이 만료 된 경우 사용자에 게 표시할 메시지입니다. 코드가 만료 되었거나 지정 된 식별자에 대 한 코드가 생성 되지 않았습니다. |
| UserMessageIfMaxRetryAttempted | 아니요 | 허용 되는 최대 확인 시도 횟수를 초과 하는 경우 사용자에 게 표시할 메시지입니다. |
| UserMessageIfInvalidCode | 아니요 | 잘못 된 코드를 제공한 경우 사용자에 게 표시할 메시지입니다. |
| UserMessageIfVerificationFailedRetryAllowed | 아니요 | 사용자가 잘못 된 코드를 제공 하 고 사용자가 올바른 코드를 제공할 수 있는 경우 사용자에 게 표시할 메시지입니다.  |
|UserMessageIfSessionConflict|아니요| 코드를 확인할 수 없는 경우 사용자에 게 표시할 메시지입니다.|

### <a name="example"></a>예제

다음 예제는 `TechnicalProfile` 코드를 확인 하는 데 사용 됩니다.

```xml
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Verify Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">VerifyCode</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
    <InputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpToVerify" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="next-steps"></a>다음 단계

사용자 지정 전자 메일 확인과 함께 일회성 암호 기술 프로필을 사용 하는 예는 다음 문서를 참조 하세요.

- Azure Active Directory B2C에서 사용자 지정 전자 메일 확인 ([Mailjet](custom-email-mailjet.md), [SendGrid](custom-email-sendgrid.md))

