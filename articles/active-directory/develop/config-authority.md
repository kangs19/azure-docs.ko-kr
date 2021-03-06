---
title: Id 공급자 구성 (MSAL iOS/macOS) | Microsoft
titleSuffix: Microsoft identity platform
description: IOS 및 macOS 용 MSAL에서 B2C, 소 버린 클라우드 및 게스트 사용자와 같은 다양 한 기관을 사용 하는 방법에 대해 알아봅니다.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 4810de772e44be22ee5bd4a9fb6ef0ef756e62f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77085216"
---
# <a name="how-to-configure-msal-for-ios-and-macos-to-use-different-identity-providers"></a>방법: 다른 id 공급자를 사용 하도록 iOS 및 macOS에 대 한 MSAL 구성

이 문서에서는 Azure Active Directory (Azure AD), B2b (B2C), 소 버린 클라우드 및 게스트 사용자와 같은 다양 한 기관에 대해 MSAL (iOS 및 macOS) 용 Microsoft 인증 라이브러리 앱을 구성 하는 방법을 보여 줍니다.  이 문서 전체에서 일반적으로 인증 기관을 id 공급자로 간주할 수 있습니다.

## <a name="default-authority-configuration"></a>기본 인증 기관 구성

`MSALPublicClientApplication`는 `https://login.microsoftonline.com/common` 대부분의 AAD (Azure Active Directory) 시나리오에 적합 한의 기본 기관 URL을 사용 하 여 구성 됩니다. 국가별 클라우드와 같은 고급 시나리오를 구현 하거나 B2C를 사용 하지 않는 경우에는 변경할 필요가 없습니다.

> [!NOTE]
> ADFS (Active Directory Federation Services id 공급자)를 사용 하는 최신 인증은 지원 되지 않습니다 (자세한 내용은 [개발자 용 adfs](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios) 참조). ADFS는 페더레이션을 통해 지원 됩니다.

## <a name="change-the-default-authority"></a>기본 인증 기관 변경

B2C (b2b)와 같은 일부 시나리오에서는 기본 인증 기관을 변경 해야 할 수 있습니다.

### <a name="b2c"></a>B2C

B2C를 사용 하려면 [MSAL (Microsoft 인증 라이브러리)](reference-v2-libraries.md) 에 다른 기관 구성이 필요 합니다. MSAL은 B2C 한 기관 URL 형식을 자체적으로 인식 합니다. 예를 들어, 인식할 수 있는 B2C authority 형식은입니다 `https://<host>/tfp/<tenant>/<policy>` `https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy` . 그러나 authority를 B2C authority로 명시적으로 선언 하 여 지원 되는 다른 B2C authority Url을 사용할 수도 있습니다.

B2C에 대 한 임의의 URL 형식을 지원 하기 위해 `MSALB2CAuthority` 다음과 같은 임의 url을 사용 하 여를 설정할 수 있습니다.

Objective-C
```objc
NSURL *authorityURL = [NSURL URLWithString:@"arbitrary URL"];
MSALB2CAuthority *b2cAuthority = [[MSALB2CAuthority alloc] initWithURL:authorityURL
                                                                     error:&b2cAuthorityError];
```
Swift
```swift
guard let authorityURL = URL(string: "arbitrary URL") else {
    // Handle error
    return
}
let b2cAuthority = try MSALB2CAuthority(url: authorityURL)
```

기본 B2C authority 형식을 사용 하지 않는 모든 B2C 기관은 알려진 인증 기관으로 선언 되어야 합니다.

기관이 정책에만 다른 경우에도 각 B2C 기관을 알려진 기관 목록에 추가 합니다.

Objective-C
```objc
MSALPublicClientApplicationConfig *b2cApplicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:b2cAuthority];
b2cApplicationConfig.knownAuthorities = @[b2cAuthority];
```
Swift
```swift
let b2cApplicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: b2cAuthority)
b2cApplicationConfig.knownAuthorities = [b2cAuthority]
```

앱에서 새 정책을 요청 하는 경우 기관 url이 정책 마다 다르므로 기관 URL을 변경 해야 합니다. 

B2C 응용 프로그램을 구성 하려면를 `@property MSALAuthority *authority` `MSALB2CAuthority` `MSALPublicClientApplicationConfig` 만들기 전에 다음과 같이의 인스턴스를 사용 하 여를 설정 합니다 `MSALPublicClientApplication` .

Objective-C
```ObjC
    // Create B2C authority URL
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy"];
    
    MSALB2CAuthority *b2cAuthority = [[MSALB2CAuthority alloc] initWithURL:authorityURL
                                                                     error:&b2cAuthorityError];
    if (!b2cAuthority)
    {
        // Handle error
        return;
    }
    
    // Create MSALPublicClientApplication configuration
    MSALPublicClientApplicationConfig *b2cApplicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                                   initWithClientId:@"your-client-id"
                                                                   redirectUri:@"your-redirect-uri"
                                                                   authority:b2cAuthority];

    // Initialize MSALPublicClientApplication
    MSALPublicClientApplication *b2cApplication =
    [[MSALPublicClientApplication alloc] initWithConfiguration:b2cApplicationConfig error:&error];
    
    if (!b2cApplication)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    // Create B2C authority URL
    guard let authorityURL = URL(string: "https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SignInPolicy") else {
        // Handle error
        return
    }
    let b2cAuthority = try MSALB2CAuthority(url: authorityURL)

    // Create MSALPublicClientApplication configuration
    let b2cApplicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: b2cAuthority)

    // Initialize MSALPublicClientApplication
    let b2cApplication = try MSALPublicClientApplication(configuration: b2cApplicationConfig)
} catch {
    // Handle error
}
```

### <a name="sovereign-clouds"></a>소버린 클라우드

앱이 소 버린 클라우드에서 실행 되는 경우에서 기관 URL을 변경 해야 할 수 있습니다 `MSALPublicClientApplication` . 다음 예에서는 독일어 AAD 클라우드를 사용 하도록 기관 URL을 설정 합니다.

Objective-C
```objc
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.de/common"];
    MSALAuthority *sovereignAuthority = [MSALAuthority authorityWithURL:authorityURL error:&authorityError];
    
    if (!sovereignAuthority)
    {
        // Handle error
        return;
    }
    
    MSALPublicClientApplicationConfig *applicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:sovereignAuthority];
    
    
    MSALPublicClientApplication *sovereignApplication = [[MSALPublicClientApplication alloc] initWithConfiguration:applicationConfig error:&error];
    
    
    if (!sovereignApplication)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    guard let authorityURL = URL(string: "https://login.microsoftonline.de/common") else {
        //Handle error
        return
    }
    let sovereignAuthority = try MSALAuthority(url: authorityURL)
            
    let applicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: sovereignAuthority)
            
    let sovereignApplication = try MSALPublicClientApplication(configuration: applicationConfig)
} catch {
    // Handle error
}
```

각 소 버린 클라우드에 다른 범위를 전달 해야 할 수 있습니다. 보낼 범위는 사용 중인 리소스에 따라 달라 집니다. 예를 들어, `"https://graph.microsoft.com/user.read"` 전 세계 클라우드에서 및 독일어 클라우드에서를 사용할 수 있습니다 `"https://graph.microsoft.de/user.read"` .

### <a name="signing-a-user-into-a-specific-tenant"></a>특정 테 넌 트에 사용자 서명

인증 기관 URL이로 설정 되 면 `"login.microsoftonline.com/common"` 사용자는 해당 홈 테 넌 트에 로그인 됩니다. 그러나 일부 앱은 사용자를 다른 테 넌 트에 서명 해야 할 수 있으며, 일부 앱은 단일 테 넌 트에서만 작동 합니다.

특정 테 넌 트에 사용자를 서명 하려면 `MSALPublicClientApplication` 특정 권한으로를 구성 합니다. 예를 들어:

`https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4`

다음은 특정 테 넌 트에 사용자를 서명 하는 방법을 보여 줍니다.

Objective-C
```objc
    NSURL *authorityURL = [NSURL URLWithString:@"https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4"];
    MSALAADAuthority *tenantedAuthority = [[MSALAADAuthority alloc] initWithURL:authorityURL error:&authorityError];
    
    if (!tenantedAuthority)
    {
        // Handle error
        return;
    }
    
    MSALPublicClientApplicationConfig *applicationConfig = [[MSALPublicClientApplicationConfig alloc]
                                                               initWithClientId:@"your-client-id"
                                                               redirectUri:@"your-redirect-uri"
                                                               authority:tenantedAuthority];
    
    MSALPublicClientApplication *application =
    [[MSALPublicClientApplication alloc] initWithConfiguration:applicationConfig error:&error];
    
    if (!application)
    {
        // Handle error
        return;
    }
```
Swift
```swift
do{
    guard let authorityURL = URL(string: "https://login.microsoftonline.com/469fdeb4-d4fd-4fde-991e-308a78e4bea4") else {
        //Handle error
        return
    }    
    let tenantedAuthority = try MSALAADAuthority(url: authorityURL)
            
    let applicationConfig = MSALPublicClientApplicationConfig(clientId: "your-client-id", redirectUri: "your-redirect-uri", authority: tenantedAuthority)
            
    let application = try MSALPublicClientApplication(configuration: applicationConfig)
} catch {
    // Handle error
}
```

## <a name="supported-authorities"></a>지원 되는 기관

### <a name="msalauthority"></a>MSALAuthority

`MSALAuthority`클래스는 MSAL authority 클래스의 기본 추상 클래스입니다. 또는을 사용 하 여 인스턴스를 만들지 `alloc` 마세요 `new` . 대신 서브 클래스 중 하나 (,)를 직접 `MSALAADAuthority` 만들거나 `MSALB2CAuthority` 팩터리 메서드를 사용 하 여 `authorityWithURL:error:` 기관 URL을 사용 하 여 하위 클래스를 만듭니다.

`url`정규화 된 인증 기관 URL을 가져오려면 속성을 사용 합니다. 추가 매개 변수 및 경로 구성 요소 또는 권한 일부가 아닌 조각은 반환 된 정규화 된 인증 기관 URL에 포함 되지 않습니다.

다음은 `MSALAuthority` 사용 하려는 기관에 따라 인스턴스화할 수 있는의 서브 클래스입니다.

### <a name="msalaadauthority"></a>MSALAADAuthority

`MSALAADAuthority`AAD 기관을 나타냅니다. Authority url은 다음과 같은 형식 이어야 합니다 `<port>` . 여기서은 선택 사항입니다.`https://<host>:<port>/<tenant>`

### <a name="msalb2cauthority"></a>MSALB2CAuthority

`MSALB2CAuthority`B2C 기관을 나타냅니다. 기본적으로 B2C authority url은 다음과 같은 형식 이어야 합니다 `<port>` . 여기서은 선택 사항입니다. `https://<host>:<port>/tfp/<tenant>/<policy>` 그러나 MSAL은 다른 임의의 B2C authority 형식도 지원 합니다.

## <a name="next-steps"></a>다음 단계

[인증 흐름 및 애플리케이션 시나리오](authentication-flows-app-scenarios.md)에 대해 알아보기
