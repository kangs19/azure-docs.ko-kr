---
title: 고가용성 및 부하 분산-Azure AD 응용 프로그램 프록시
description: 트래픽 배포가 응용 프로그램 프록시 배포와 함께 작동 하는 방식입니다. 커넥터 성능을 최적화 하 고 백 엔드 서버에 대 한 부하 분산을 사용 하는 방법에 대 한 팁을 제공 합니다.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: kenwith
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ab3b340654fd6d824edef0a33d1ea363a913654
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84764590"
---
# <a name="high-availability-and-load-balancing-of-your-application-proxy-connectors-and-applications"></a>응용 프로그램 프록시 커넥터 및 응용 프로그램의 고가용성 및 부하 분산

이 문서에서는 응용 프로그램 프록시 배포와 트래픽 배포가 작동 하는 방식을 설명 합니다. 다음에 대해 설명 합니다.

- 커넥터 성능 최적화에 대 한 팁과 함께 사용자 및 커넥터 간에 트래픽이 분산 되는 방식

- 여러 백 엔드 서버 간 부하 분산에 대 한 권장 사항과 함께 커넥터와 백 엔드 앱 서버 간 트래픽 흐름 방법

## <a name="traffic-distribution-across-connectors"></a>커넥터 간 트래픽 분산

커넥터는 고가용성을 위한 원칙에 따라 연결을 설정 합니다. 트래픽이 항상 커넥터 간에 균등 하 게 분산 되 고 세션 선호도는 보장 되지 않습니다. 그러나 사용량은 다르며 요청은 응용 프로그램 프록시 서비스 인스턴스로 임의로 보내집니다. 결과적으로 트래픽은 일반적으로 커넥터를 통해 거의 균등 하 게 분산 됩니다. 아래 다이어그램 및 단계에서는 사용자와 커넥터 간에 연결을 설정 하는 방법을 보여 줍니다.

![사용자와 커넥터 간의 연결을 보여 주는 다이어그램](media/application-proxy-high-availability-load-balancing/application-proxy-connections.png)

1. 클라이언트 장치의 사용자가 응용 프로그램 프록시를 통해 게시 된 온-프레미스 응용 프로그램에 액세스 하려고 합니다.
2. 요청은 요청을 수행 해야 하는 응용 프로그램 프록시 서비스 인스턴스를 확인 하기 위해 Azure Load Balancer를 거칩니다. 지역에 따라 요청을 수락 하는 데 사용할 수 있는 수만 개의 인스턴스가 있습니다. 이 메서드는 서비스 인스턴스 간에 트래픽을 균등 하 게 분산 하는 데 도움이 됩니다.
3. 요청은 [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/)전송 됩니다.
4. Service Bus 사용 가능한 커넥터에 신호를 보냅니다. 그런 다음 커넥터는 Service Bus에서 요청을 선택 합니다.
   - 2 단계에서 요청은 다른 응용 프로그램 프록시 서비스 인스턴스로 이동 하므로 서로 다른 커넥터를 사용 하 여 연결을 만들 수 있습니다. 결과적으로 커넥터는 그룹 내에서 거의 균등 하 게 사용 됩니다.
5. 커넥터는 응용 프로그램의 백 엔드 서버에 요청을 전달 합니다. 그런 다음 응용 프로그램은 커넥터에 응답을 다시 보냅니다.
6. 커넥터는 요청이 발생 한 서비스 인스턴스에 대 한 아웃 바운드 연결을 열어 응답을 완료 합니다. 그러면이 연결이 즉시 닫힙니다. 기본적으로 각 커넥터는 200 개의 동시 아웃 바운드 연결로 제한 됩니다.
7. 그런 다음 응답은 서비스 인스턴스에서 클라이언트로 다시 전달 됩니다.
8. 동일한 연결의 후속 요청은 위의 단계를 반복 합니다.

응용 프로그램은 종종 많은 리소스를 포함 하 고 로드 될 때 여러 개의 연결을 엽니다. 각 연결은 위의 단계를 수행 하 여 서비스 인스턴스에 할당 되 고, 연결이 아직 이전에 커넥터와 연결 되지 않은 경우 사용 가능한 새 커넥터를 선택 합니다.


## <a name="best-practices-for-high-availability-of-connectors"></a>커넥터의 고가용성에 대 한 모범 사례

- 고가용성을 위해 커넥터 간에 트래픽이 분산 되는 방식 때문에 커넥터 그룹에는 항상 두 개 이상의 커넥터가 있어야 합니다. 커넥터 사이에 추가 버퍼를 제공 하기 위해 세 가지 커넥터를 선호 합니다. 필요한 커넥터 수를 결정 하려면 용량 계획 설명서를 참조 하세요.

- 단일 실패 지점을 방지 하기 위해 커넥터를 서로 다른 아웃 바운드 연결에 저장 합니다. 커넥터가 동일한 아웃 바운드 연결을 사용 하는 경우 연결에 대 한 네트워크 문제로 인해 커넥터를 사용 하는 모든 커넥터에 영향을 줄 수 있습니다.

- 프로덕션 응용 프로그램에 연결 될 때 커넥터를 강제로 다시 시작 하지 않습니다. 이렇게 하면 커넥터 간 트래픽 배포에 부정적인 영향을 줄 수 있습니다. 커넥터를 다시 시작 하면 더 많은 커넥터를 사용할 수 없게 되 고 나머지 사용 가능한 커넥터에 연결 합니다. 결과적으로 커넥터를 처음부터 균등 하 게 사용 하지 않습니다.

- 커넥터와 Azure 간의 아웃 바운드 TLS 통신에 대 한 모든 형태의 인라인 검사를 방지 합니다. 이 유형의 인라인 검사를 수행 하면 통신 흐름이 저하 됩니다.

- 커넥터에 대해 자동 업데이트를 계속 실행 해야 합니다. 응용 프로그램 프록시 Connector Updater 서비스가 실행 중인 경우에는 커넥터가 자동으로 업데이트 되 고 최신 업그레이드를 수신 합니다. 서버에 커넥터 업데이터 서비스가 표시되지 않는 경우 업데이트를 받으려면 커넥터를 다시 설치해야 합니다.

## <a name="traffic-flow-between-connectors-and-back-end-application-servers"></a>커넥터와 백 엔드 응용 프로그램 서버 간 트래픽 흐름

고가용성이 요소인 또 다른 주요 영역은 커넥터와 백 엔드 서버 간의 연결입니다. Azure AD 응용 프로그램 프록시를 통해 응용 프로그램을 게시 하는 경우 사용자가 응용 프로그램으로 이동 하는 트래픽은 세 가지 홉을 통해 흐릅니다.

1. 사용자는 Azure에서 Azure AD 응용 프로그램 프록시 서비스 공용 끝점에 연결 합니다. 연결은 클라이언트의 원래 클라이언트 IP 주소 (공용)와 응용 프로그램 프록시 끝점의 IP 주소 간에 설정 됩니다.
2. 응용 프로그램 프록시 커넥터는 응용 프로그램 프록시 서비스에서 클라이언트의 HTTP 요청을 가져옵니다.
3. 응용 프로그램 프록시 커넥터는 대상 응용 프로그램에 연결 합니다. 커넥터는 자체 IP 주소를 사용 하 여 연결을 설정 합니다.

![응용 프로그램 프록시를 통해 응용 프로그램에 연결 하는 사용자 다이어그램](media/application-proxy-high-availability-load-balancing/application-proxy-three-hops.png)

### <a name="x-forwarded-for-header-field-considerations"></a>X 전달-헤더 필드 고려 사항
일부 상황 (예: 감사, 부하 분산 등)에서 외부 클라이언트의 원래 IP 주소를 온-프레미스 환경과 공유 하는 것이 요구 사항입니다. 요구 사항을 해결 하기 위해 Azure AD 응용 프로그램 프록시 커넥터는 원래 클라이언트 IP 주소 (공용)와 함께 X 전달 된 헤더 필드를 HTTP 요청에 추가 합니다. 그러면 적절 한 네트워크 장치 (부하 분산 장치, 방화벽) 또는 웹 서버나 백 엔드 응용 프로그램에서 정보를 읽고 사용할 수 있습니다.

## <a name="best-practices-for-load-balancing-among-multiple-app-servers"></a>여러 앱 서버 간 부하 분산에 대 한 모범 사례
응용 프로그램 프록시 응용 프로그램에 할당 된 커넥터 그룹에 두 개 이상의 커넥터가 있고 여러 서버 (서버 팜)에서 백 엔드 웹 응용 프로그램을 실행 하는 경우 적절 한 부하 분산 전략이 필요 합니다. 좋은 전략을 사용 하면 서버에서 클라이언트 요청을 균일 하 게 선택 하 고 서버 팜에서 서버를 과도 하 게 사용 하는 것을 방지할 수 있습니다.
### <a name="scenario-1-back-end-application-does-not-require-session-persistence"></a>시나리오 1: 백 엔드 응용 프로그램에 세션 지 속성이 필요 하지 않음
가장 간단한 시나리오는 백 엔드 웹 응용 프로그램에서 세션의 공동 작업이 필요 하지 않은 경우 (세션 지 속성)입니다. 서버 팜의 백 엔드 응용 프로그램 인스턴스가 사용자의 모든 요청을 처리할 수 있습니다. 계층 4 부하 분산 장치를 사용 하 고 선호도 없이 구성할 수 있습니다. 일부 옵션에는 Microsoft 네트워크 부하 분산 및 Azure Load Balancer 또는 다른 공급 업체의 부하 분산 장치가 있습니다. 또는 라운드 로빈 DNS를 구성할 수 있습니다.
### <a name="scenario-2-back-end-application-requires-session-persistence"></a>시나리오 2: 백 엔드 응용 프로그램에 세션 지 속성 필요
이 시나리오에서 백 엔드 웹 응용 프로그램을 사용 하려면 인증 된 세션 중에 세션 유지 (세션 유지)가 필요 합니다. 사용자의 모든 요청은 서버 팜의 동일한 서버에서 실행 되는 백 엔드 응용 프로그램 인스턴스에서 처리 되어야 합니다.
클라이언트는 일반적으로 응용 프로그램 프록시 서비스에 대 한 여러 연결을 설정 하므로이 시나리오는 더 복잡할 수 있습니다. 다른 연결에 대 한 요청은 팜의 다른 커넥터 및 서버에 도착할 수 있습니다. 각 커넥터는이 통신에 자체 IP 주소를 사용 하기 때문에 부하 분산 장치는 커넥터의 IP 주소에 따라 세션의 연결을 유지할 수 없습니다. 원본 IP 선호도는 사용할 수 없습니다.
시나리오 2에 대 한 몇 가지 옵션은 다음과 같습니다.

- 옵션 1: 부하 분산 장치에 의해 설정 된 세션 쿠키에 대 한 세션 지 속성을 기반으로 합니다. 이 옵션을 사용 하면 백 엔드 서버 간에 부하를 균등 하 게 분산할 수 있으므로이 옵션을 사용 하는 것이 좋습니다. 이 기능에는 계층 7 부하 분산 장치가 필요 하며,이 기능을 사용 하 여 HTTP 트래픽을 처리 하 고 TLS 연결을 종료할 수 있습니다. Azure 애플리케이션 게이트웨이 (세션 선호도) 또는 다른 공급 업체의 부하 분산 장치를 사용할 수 있습니다.

- 옵션 2: X로 전달 된 헤더 필드에서 세션 지 속성을 기반으로 합니다. 이 옵션을 사용 하려면이 기능을 사용 하는 계층 7 부하 분산 장치가 필요 하며 HTTP 트래픽을 처리 하 고 TLS 연결을 종료할 수 있습니다.  

- 옵션 3: 세션 지 속성을 요구 하지 않도록 백 엔드 응용 프로그램을 구성 합니다.

백 엔드 응용 프로그램의 부하 분산 요구 사항을 이해 하려면 소프트웨어 공급 업체의 설명서를 참조 하십시오.

## <a name="next-steps"></a>다음 단계

- [애플리케이션 프록시 사용](application-proxy-add-on-premises-application.md)
- [Single Sign-On 사용](application-proxy-configure-single-sign-on-with-kcd.md)
- [조건부 액세스 사용](application-proxy-integrate-with-sharepoint-server.md)
- [애플리케이션 프록시에서 발생한 문제 해결](application-proxy-troubleshoot.md)
- [Azure AD 아키텍처가 고가용성을 지 원하는 방법 알아보기](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-architecture)
