---
title: 개념-Network 상호 연결과
description: Azure VMware 솔루션 (AVS)에서 네트워킹 및 상호 연결과의 주요 측면과 사용 사례에 대해 알아봅니다.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 35d886fe0f6a68e522d7f2cf20b450b5d9afc199
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629202"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Azure VMware 솔루션 (AVS) 미리 보기 네트워킹 및 상호 연결과 개념

Azure의 Azure VMware 솔루션 (AVS) 사설 클라우드와 온-프레미스 환경 또는 가상 네트워크 간의 네트워크 상호 연결과을 통해 사설 클라우드를 액세스 하 고 사용할 수 있습니다. 상호 연결과의 기반을 설정 하는 몇 가지 주요 네트워킹 및 상호 연결과 개념은이 문서에 설명 되어 있습니다.

상호 연결과에 대 한 유용한 관점은 두 가지 유형의 AVS 사설 클라우드 구현을 고려 하는 것입니다. 기본 Azure 전용 상호 연결과 구현 및 사설 클라우드 상호 연결과에 대 한 전체 온-프레미스의 구현입니다.

AVS 사설 클라우드의 사용 사례는 다음과 같습니다.
- 클라우드의 새 VMware VM 워크 로드
- 클라우드로 VM 작업 버스트
- 클라우드로 VM 워크 로드 마이그레이션
- 재해 복구
- Azure 서비스 사용량

 AVS 서비스의 모든 사용 사례는 온-프레미스에서 사설 클라우드 연결을 사용 하 여 사용 하도록 설정 됩니다. 기본 상호 연결과 모델은 온-프레미스 환경에서 액세스 하지 않아도 되는 AVS 평가 또는 구현에 가장 적합 합니다.

다음 섹션에서는 두 가지 유형의 AVS 사설 클라우드 상호 연결과에 대해 설명 합니다.  가장 기본적인 상호 연결과는 "Azure virtual network 연결"입니다. Azure에서 단일 가상 네트워크를 사용 하 여 사설 클라우드를 관리 하 고 사용할 수 있습니다. "온-프레미스 연결"에 설명 된 상호 연결과는 가상 네트워크 연결을 확장 하 여 온-프레미스 환경과 AVS 사설 클라우드 간의 상호 연결과 포함 합니다.

## <a name="azure-virtual-network-interconnectivity"></a>Azure virtual network 상호 연결과

사설 클라우드 배포 시 설정 된 기본 네트워크 상호 연결과 아래 다이어그램에 표시 됩니다. Azure의 가상 네트워크와 사설 클라우드의 논리적 Express 경로 기반 네트워킹을 보여 줍니다. 상호 연결과는 다음과 같은 세 가지 주요 사용 사례를 충족 합니다.
- VCenter server 및 NSX manager가 있는 관리 네트워크에 대 한 인바운드 액세스
    - 온-프레미스 시스템이 아닌 Azure 구독 내의 Vm에서 액세스할 수 있습니다.
- Vm에서 Azure 서비스로의 아웃 바운드 액세스.
- 사설 클라우드를 실행 하는 작업의 인바운드 액세스 및 사용.

![기본 가상 네트워크-사설 클라우드 연결](./media/concepts/adjacency-overview-drawing-single.png)

사설 클라우드의 express 경로 회로에 대 한 구독의 가상 네트워크에서 연결을 만들 때이 가상 네트워크에서 사설 클라우드로의 Express 경로 회로 시나리오를 설정할 수 있습니다. 피어 링은 Azure Portal에서 요청 하는 권한 부여 키와 회로 ID를 사용 합니다. 피어 링을 통해 설정 된 Express 경로 연결은 사설 클라우드와 가상 네트워크 간의 일대일 연결입니다. 사설 클라우드를 관리 하 고, 사설 클라우드에서 워크 로드를 사용 하 고, 해당 Express 경로 연결을 통해 Azure 서비스에 액세스할 수 있습니다.

AVS 사설 클라우드를 배포할 때에는 단일/22 개인 네트워크 주소 공간이 필요 합니다. 이 주소 공간은 구독의 다른 가상 네트워크에 사용 된 주소 공간과 겹치지 않아야 합니다. 이 주소 공간 내에서 관리, 프로 비전 및 vMotion 네트워크는 자동으로 프로 비전 됩니다. 라우팅은 BGP 기반 이며, 각 사설 클라우드 배포에 대해 기본적으로 자동으로 프로 비전 되 고 사용 하도록 설정 됩니다.

사설 클라우드를 배포 하면 vCenter 및 NSX Manager의 IP 주소가 제공 됩니다. 이러한 관리 인터페이스에 액세스 하려면 구독의 가상 네트워크에 추가 리소스를 만듭니다. 이러한 리소스를 만들고 Express 경로 개인 피어 링을 설정 하는 절차는 자습서에서 제공 됩니다.

사설 클라우드 논리 네트워킹을 설계 하 고 NSX-T를 사용 하 여 구현 합니다. 사설 클라우드는 미리 프로 비전 된 NSX와 함께 제공 됩니다. 계층 0 게이트웨이 & 계층 1 게이트웨이는 사용자를 위해 사전 프로 비전 됩니다. 세그먼트를 만들어 기존 계층-1 게이트웨이에 연결 하거나 정의할 수 있는 새 계층 1 게이트웨이에 연결할 수 있습니다. NSX 논리 네트워킹 구성 요소는 워크 로드 간에 동-서 연결을 제공 하며 인터넷 및 Azure 서비스에 대 한 북아메리카 연결도 제공 합니다. 

## <a name="on-premises-interconnectivity"></a>온-프레미스 상호 연결과

또한 온-프레미스 환경을 AVS 사설 클라우드에 연결할 수 있습니다. 이 유형의 상호 연결과은 이전 섹션에 설명 된 기본 상호 연결과에 대 한 확장입니다.

![가상 네트워크 및 온-프레미스 전체 사설 클라우드 연결](./media/concepts/adjacency-overview-drawing-double.png)

사설 클라우드에 대 한 전체 상호 연결과를 설정 하려면 Azure Portal을 사용 하 여 사설 클라우드 Express 경로 회로와 온-프레미스 Express 경로 회로 간에 Express 경로 Global Reach를 사용 하도록 설정 합니다. 이 구성은 기본 연결을 확장 하 여 온-프레미스 환경에서 사설 클라우드에 대 한 액세스를 포함 합니다.

온-프레미스에서 Azure로의 가상 네트워크 Express 경로 회로는 온-프레미스 환경에서 Azure의 사설 클라우드로 연결 하는 데 필요 합니다. 이 Express 경로 회로는 구독에 있으며 사설 클라우드 배포에 포함 되지 않습니다. 온-프레미스 Express 경로 회로는이 문서의 범위를 벗어났습니다. 사설 클라우드에 대 한 온-프레미스 연결이 필요한 경우 기존 Express 경로 회로 중 하나를 사용 하거나 Azure Portal에서 구매할 수 있습니다.

Global Reach 연결 되 면 두 개의 Express 경로 회로는 온-프레미스 환경과 사설 클라우드 간에 네트워크 트래픽을 라우팅합니다. 온-프레미스와 사설 클라우드 상호 연결과는 위의 다이어그램에 나와 있습니다. 다이어그램에 표시 되는 상호 연결과를 사용 하면 다음과 같은 사용 사례를 사용할 수 있습니다.

- 핫/콜드 교차 vCenter vMotion
- 온-프레미스에서 AVS 사설 클라우드 관리 액세스

전체 연결을 사용 하도록 설정 하려면 Azure Portal에서 Global Reach에 대 한 인증 키와 개인 피어 링 ID를 요청할 수 있습니다. 키와 ID를 사용 하 여 구독의 Express 경로 회로와 새 사설 클라우드의 Express 경로 회로 간에 Global Reach을 설정 합니다. [사설 클라우드를 만들기 위한 자습서](tutorial-create-private-cloud.md) 에서는 키와 ID를 요청 하 고 사용 하는 절차를 제공 합니다.

솔루션의 라우팅 요구 사항은 다른 가상 네트워크 및 온-프레미스 네트워크와 겹치지 않도록 사설 클라우드 네트워크 주소 공간을 계획 해야 합니다. AVS 프라이빗 클라우드의 서브넷에는 `/22` CIDR 네트워크 주소 블록 이상이 필요합니다(아래 참조). 이 네트워크는 온-프레미스 네트워크를 보완합니다. 온-프레미스 환경 및 가상 네트워크에 연결하려면 겹치지 않는 네트워크 주소 블록이어야 합니다.

`/22` CIDR 네트워크 주소 블록의 예: `10.10.0.0/22`

서브넷:

| 네트워크 사용량             | 서브넷 | 예제        |
| ------------------------- | ------ | -------------- |
| 프라이빗 클라우드 관리            | `/24`    | `10.10.0.0/24`   |
| vMotion 네트워크       | `/24`    | `10.10.1.0/24`   |
| VM 워크로드 | `/24`   | `10.10.2.0/24`   |
| ExpressRoute 피어링 | `/24`    | `10.10.3.8/30`   |

## <a name="next-steps"></a>다음 단계 

다음 단계는 [사설 클라우드 저장소 개념](concepts-storage.md)에 대해 학습 하는 것입니다.

<!-- LINKS - external -->
[enable Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach

<!-- LINKS - internal -->

