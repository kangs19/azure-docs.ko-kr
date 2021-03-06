---
title: Azure Service Fabric의 재구성
description: 상태 저장 서비스 복제본의 구성과 변경 중 일관성 및 가용성을 유지 하는 데 사용 하 Service Fabric 재구성 프로세스에 대해 알아봅니다.
author: appi101
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: bd46a7776495624affef77a44fcf68334750ba17
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75609998"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Azure Service Fabric의 재구성
*구성*은 상태 저장 서비스의 파티션에 대한 복제본과 그 역할로 정의됩니다.

*재구성*은 한 구성을 다른 구성으로 이동하는 프로세스입니다. 상태 저장 서비스의 파티션에 대해 복제본 세트에 변경을 수행합니다. 이전 구성을 *이전 구성 (PC)* 이라고 하며, 새 구성을 *현재 구성 (CC)* 이라고 합니다. Azure Service Fabric의 재구성 프로토콜은 복제본 세트에 대한 모든 변경에서 일관성을 유지하고 가용성을 유지 관리합니다.

장애 조치(Failover) 관리자는 시스템의 여러 이벤트에 대한 대처로 재구성을 시작합니다. 예를 들어, 주 복제본이 실패하면 활성 보조를 주 복제본으로 승격시키기 위해 재구성이 시작됩니다. 또 다른 예로 노드 업그레이드를 위해 주 복제본을 다른 노드로 이동해야 하는 애플리케이션 업그레이드를 들 수 있습니다.

## <a name="reconfiguration-types"></a>재구성 유형
재구성은 두 가지 유형으로 분류할 수 있습니다.

- 주 복제본이 변경 중인 재구성
    - **장애 조치**: 장애 조치 (failover)는 실행 중인 주 복제본의 오류에 대 한 응답으로 재구성 됩니다.
    - **SwapPrimary**: 교체는 보통 부하 분산이나 업그레이드 등의 이유로 Service Fabric이 한 노드에서 다른 노드로 주 복제본의 실행을 이동하는 재구성입니다.

- 주 복제본이 변경되지 않는 재구성

## <a name="reconfiguration-phases"></a>재구성 단계
재구성은 몇 가지 단계로 진행됩니다.

- **단계 0**: 이 단계는 현재 주 복제본이 상태를 새 주 복제본으로 전달하고 활성 보조로 전환하는 주 복제본 교체 재구성에서 발생합니다.

- **단계 1**: 이 단계는 주 복제본이 변경 중인 재구성에서 발생합니다. 이 단계 중에는 Service Fabric이 현재 복제본 중 어떤 것이 정확한 주 복제본인가를 식별합니다. 주 복제본 교체 재구성 중에는 새 주 복제본이 이미 선택되었으므로 이 단계가 필요하지 않습니다. 

- **단계 2**:이 단계 중 Service Fabric은 현재 구성의 복제본 대부분에서 모든 데이터를 사용할 수 있도록 합니다.

내부 전용인 몇 가지 다른 단계가 있습니다.

## <a name="stuck-reconfigurations"></a>멈춘 재구성
재구성은 다양한 이유로 *멈출 수* 있습니다. 몇 가지 일반적인 이유는 다음과 같습니다.

- **복제본 다운**: 일부 재구성 단계에서는 구성에서 대부분의 복제본이 작동 해야 합니다.
- **네트워크 또는 통신 문제**: 재구성에서는 서로 다른 노드 간의 네트워크 연결이 필요합니다.
- **API 오류**: 재구성 프로토콜에서는 서비스 구현이 특정 API를 완료해야 합니다. 예를 들어, 신뢰할 수 있는 서비스에서 토큰 취소를 적용하지 않으면 SwapPrimary 재구성이 멈춥니다.

System.FM, System.RA, System.RAP 등과 같은 시스템 구성 요소의 상태 보고서를 사용하여 재구성이 멈춘 지점을 진단합니다. [시스템 상태 보고서 페이지](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)에서 이러한 상태 보고서를 설명합니다.

## <a name="next-steps"></a>다음 단계
Service Fabric 개념에 대한 자세한 내용은 다음 문서를 참조하세요.

- [Reliable Services 수명 주기 - C#](service-fabric-reliable-services-lifecycle.md)
- [시스템 상태 보고서](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [복제본 및 인스턴스](service-fabric-concepts-replica-lifecycle.md)
