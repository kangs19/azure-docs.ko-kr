---
title: 조건부 액세스 - 위치별 액세스 차단 - Azure Active Directory
description: IP 위치별로 리소스에 대한 액세스를 차단하는 사용자 지정 조건부 액세스 정책 만들기
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5350e728ad9c30a6e9258ce9c7615baf020473a8
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995125"
---
# <a name="conditional-access-block-access-by-location"></a>조건부 액세스: 위치별 액세스 차단

조건부 액세스에서 위치 조건을 사용하여 사용자의 네트워크 위치를 기준으로 클라우드 앱에 대한 액세스를 제어할 수 있습니다. 위치 조건은 조직에서 트래픽이 들어오지 않아야 한다고 알고 있는 국가/지역의 액세스를 차단하는 데 주로 사용됩니다.

## <a name="define-locations"></a>위치 정의

1. **Azure Portal**에 전역 관리자, 보안 관리자 또는 조건부 액세스 관리자로 로그인합니다.
1. **Azure Active Directory** > **보안** > **조건부 액세스** > **명명된 위치**로 이동합니다.
1. **새 위치**를 선택합니다.
1. 위치에 이름을 지정합니다.
1. 해당 위치 또는 **국가/지역**을 구성하는 외부에서 액세스할 수 있는 특정 IPv4 주소 범위를 알고 있는 경우 **IP 범위**를 선택합니다.
   1. **IP 범위**를 제공하거나 지정하는 위치에 대한 **국가/지역**을 선택합니다.
      * 국가/지역을 선택하는 경우 알 수 없는 영역을 포함하도록 선택할 수 있습니다.
1. **저장**을 선택합니다.

조건부 액세스의 위치 조건에 대한 자세한 내용은 [Azure Active Directory 조건부 액세스의 위치 조건이란?](location-condition.md) 문서에서 찾을 수 있습니다.

## <a name="create-a-conditional-access-policy"></a>조건부 액세스 정책 만들기

1. **Azure Portal**에 전역 관리자, 보안 관리자 또는 조건부 액세스 관리자로 로그인합니다.
1. **Azure Active Directory** > **Security** > **조건부 액세스**로 이동합니다.
1. **새 정책**을 선택합니다.
1. 정책에 이름을 지정합니다. 조직에서 정책 이름에 대해 의미 있는 표준을 만드는 것이 좋습니다.
1. **할당** 아래에서 **사용자 및 그룹**을 선택합니다.
   1. **포함**에서 **모든 사용자**를 선택합니다.
   1. **완료**를 선택합니다.
1. **클라우드 앱 또는 작업** > **포함**에서 **모든 클라우드 앱**을 선택하고 **완료**를 선택합니다.
1. **조건** > **위치**에서 다음을 수행합니다.
   1. **구성**을 **예**로 설정합니다.
   1. **포함**에서 **선택한 위치**를 선택합니다.
   1. 조직에 대해 만든 차단된 위치를 선택합니다.
   1. **선택** > **완료** > **완료**를 클릭합니다.
1. **조건**  > **클라이언트 앱(미리 보기)** 에서 **구성**을 **예**로 설정하고 **완료**를 선택합니다.
1. **액세스 제어** > **차단**에서 **선택**을 선택합니다.
1. 설정을 확인하고 **정책 사용**을 **켜기**로 설정합니다.
1. **만들기**를 선택하여 정책을 만들어 사용하도록 설정합니다.

## <a name="next-steps"></a>다음 단계

[조건부 액세스 일반 정책](concept-conditional-access-policy-common.md)

[조건부 액세스 보고서 전용 모드를 사용하여 영향 확인](howto-conditional-access-report-only.md)

[조건부 액세스 What If 도구를 사용하여 로그인 동작 시뮬레이션](troubleshoot-conditional-access-what-if.md)
