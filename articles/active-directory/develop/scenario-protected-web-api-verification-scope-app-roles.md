---
title: 범위 및 앱 역할 확인 보호된 웹 API | Azure
titleSuffix: Microsoft identity platform
description: 보호된 웹 API를 빌드하고 애플리케이션의 코드를 구성하는 방법에 대해 알아봅니다.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: a4ee2679da5065ab9e9b02d4ddb313fab75e78f7
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845138"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>보호된 웹 API: 범위 및 앱 역할 확인

이 문서에서는 웹 API에 권한 부여를 추가할 수 있는 방법을 설명합니다. 이 보호는 API가 다음에 의해서만 호출되도록 합니다.

- 올바른 범위를 가진 사용자를 대신하는 애플리케이션
- 올바른 애플리케이션 역할이 있는 디먼 앱

> [!NOTE]
> 이 문서의 코드 조각은 완전히 작동하는 다음 샘플에서 추출됩니다.
>
> - GitHub의 [ASP.NET Core 웹 API 증분 자습서](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37)
> - [ASP.NET Web API 샘플](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

ASP.NET 또는 ASP.NET Core 웹 API를 보호하려면 `[Authorize]` 특성을 다음 항목 중 하나에 추가해야 합니다.

- 모든 컨트롤러 작업을 보호하려는 경우 컨트롤러 자체
- API에 대한 개별 컨트롤러 작업

```csharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

그러나 이 보호는 충분하지 않습니다. ASP.NET 및 ASP.NET Core가 토큰의 유효성을 검사한다는 것만 보장합니다. API는 API를 호출하는 데 사용되는 토큰이 예상된 클레임으로 요청되는지 확인해야 합니다. 특히 다음과 같은 클레임을 확인해야 합니다.

- 사용자를 대신하여 API를 호출하는 경우 ‘범위’
- 디먼 앱에서 API를 호출할 수 있는 경우 ‘앱 역할’

## <a name="verify-scopes-in-apis-called-on-behalf-of-users"></a>사용자를 대신하여 호출된 API의 범위 확인

클라이언트 앱이 사용자를 대신하여 API를 호출하는 경우 API는 API에 대한 특정 범위를 가진 전달자 토큰을 요청해야 합니다. 자세한 내용은 [코드 구성 | 전달자 토큰](scenario-protected-web-api-app-configuration.md#bearer-token)을 참조하세요.

```csharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

`VerifyUserHasAnyAcceptedScope` 메서드는 다음 단계와 같은 작업을 수행합니다.

- `http://schemas.microsoft.com/identity/claims/scope` 또는 `scp`라는 클레임이 있는지 확인합니다.
- 클레임에 API에서 예상하는 범위를 포함하는 값이 있는지 확인합니다.

```csharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

위의 [샘플 코드](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47)는 ASP.NET Core용입니다. ASP.NET의 경우 `HttpContext.User`를 `ClaimsPrincipal.Current`로 바꾸고 클레임 유형 `"http://schemas.microsoft.com/identity/claims/scope"`를 `"scp"`로 바꿉니다. 또한 이 문서의 뒷부분에 나오는 코드 조각을 참조하세요.

## <a name="verify-app-roles-in-apis-called-by-daemon-apps"></a>디먼 앱에서 호출된 API의 앱 역할 확인

웹 API가 [디먼 앱](scenario-daemon-overview.md)에 의해 호출되는 경우 해당 앱에서 웹 API에 대한 애플리케이션 권한을 요구해야 합니다. [애플리케이션 사용 권한(앱 역할) 노출](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles)에 나온 것처럼 API는 이러한 사용 권한을 노출합니다. 한 가지 예는 `access_as_application` 앱 역할입니다.

이제 API에서 받은 토큰에 `roles` 클레임이 포함되어 있고 이 클레임에 예상한 값이 있는지 확인하도록 해야 합니다. 확인 코드는 컨트롤러 작업이 범위 대신 역할을 테스트한다는 것을 제외하면 위임된 권한을 확인하는 코드와 비슷합니다.

```csharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

`ValidateAppRole` 메서드는 다음과 비슷할 수 있습니다.

```csharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage
        { StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found"
        });
    }
}
}
```

이번에는 ASP.NET에 대한 코드 조각입니다. ASP.NET Core의 경우 `ClaimsPrincipal.Current`를 `HttpContext.User`로 바꾸고 클레임 이름 `"roles"`를 `"http://schemas.microsoft.com/ws/2008/06/identity/claims/role"`로 바꿉니다. 또한 이 문서의 앞부분에 나오는 코드 조각을 참조하세요.

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>웹 API가 디먼 앱에 의해서만 호출되어야 하는 경우 앱 전용 토큰 수락

사용자는 사용자 할당 패턴에서 역할 클레임을 사용할 수도 있습니다. 이 내용은 [방법: 애플리케이션에 앱 역할 추가 및 토큰으로 수신](howto-add-app-roles-in-azure-ad-apps.md)에 나와 있습니다. 역할을 둘 다에 할당할 수 있는 경우 역할을 확인하면 앱은 사용자로 로그인하고 사용자는 앱으로 로그인할 수 있습니다. 이러한 혼동을 방지하기 위해 사용자와 앱에 대해 서로 다른 역할을 선언하는 것이 좋습니다.

디먼 앱만 웹 API를 호출하도록 하려면 앱 역할의 유효성을 검사할 때 토큰이 앱 전용 토큰이라는 조건을 추가합니다.

```csharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

역 조건을 확인하면 사용자로 로그인하는 앱만 API를 호출할 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [프로덕션으로 이동](scenario-protected-web-api-production.md)
