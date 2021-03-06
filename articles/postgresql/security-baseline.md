---
title: Azure Database for PostgreSQL 단일 서버에 대한 Azure 보안 기준
description: Azure Database for PostgreSQL 단일 서버에 대한 Azure 보안 기준
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 89d760e28025f64b8cd8940fc08f06d23c969e12
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655746"
---
# <a name="azure-security-baseline-for-azure-database-for-postgresql-single-server"></a>Azure Database for PostgreSQL 단일 서버에 대한 Azure 보안 기준

Azure Database for PostgreSQL 단일 서버에 대한 Azure 보안 기준에는 배포의 보안 상태를 향상시키는 데 도움이 되는 추천 사항이 있습니다.

이 서비스의 기준은 [Azure Security Benchmark 버전 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview)에서 가져왔으며, 모범 사례 지침을 통해 Azure에서 클라우드 솔루션을 보호하는 방법에 대한 추천 사항을 제공합니다.

자세한 내용은 [Azure 보안 기준 개요](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)를 참조하세요.

## <a name="network-security"></a>네트워크 보안

*자세한 내용은 [보안 그룹: 네트워크 보안](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)을 참조하세요.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Virtual Network에서 네트워크 보안 그룹 또는 Azure Firewall을 사용하여 리소스 보호

**지침**: 프라이빗 엔드포인트를 사용하여 Azure Database for PostgreSQL에 대한 Private Link를 구성합니다. Private Link를 사용하면 프라이빗 엔드포인트를 통해 Azure의 다양한 PaaS 서비스에 연결할 수 있습니다. Azure Private Link는 기본적으로 개인 VNet(Virtual Network) 내에 Azure 서비스를 제공합니다. 가상 네트워크와 PostgreSQL 인스턴스 사이의 트래픽은 Microsoft 백본 네트워크로 이동합니다.

또는 Virtual Network 서비스 엔드포인트를 사용하여 Azure Database for PostgreSQL 구현에 대한 네트워크 액세스를 보호하고 제한할 수 있습니다. 가상 네트워크 규칙은 가상 네트워크의 특정 서브넷에서 보낸 통신을 수락하는지 여부를 Azure Database for PostgreSQL 서버에서 제어하는 하나의 방화벽 보안 기능입니다.

방화벽 규칙을 사용하여 Azure Database for PostgreSQL 서버를 보호할 수도 있습니다. 서버 방화벽은 권한이 있는 컴퓨터를 지정할 때까지 데이터베이스 서버에 대한 모든 액세스를 차단합니다. 방화벽을 구성하려면 허용 가능한 IP 주소 범위를 지정하는 방화벽 규칙을 생성해야 합니다. 서버 수준의 방화벽 규칙을 만들 수 있습니다.

Azure Database for PostgreSQL에 대한 Private Link를 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

Azure Database for PostgreSQL에서 VNet 서비스 엔드포인트 및 VNet 규칙을 만들고 관리하는 방법: https://docs.microsoft.com/azure/postgresql/howto-manage-vnet-using-portal

Azure Database for PostgreSQL 방화벽 규칙을 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-manage-firewall-using-portal

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, 서브넷 및 NIC의 구성과 트래픽에 대한 모니터링 및 기록

**지침**: Azure Database for PostgreSQL 인스턴스를 프라이빗 엔드포인트에서 보호하는 경우 가상 머신을 동일한 가상 네트워크에 배포할 수 있습니다. 데이터 반출의 위험을 줄이기 위해 NSG(네트워크 보안 그룹)를 사용할 수 있습니다. NSG 흐름 로그를 사용하도록 설정하고, 트래픽 감사를 위해 로그를 스토리지 계정에 보냅니다. 또한 NSG 흐름 로그를 Log Analytics 작업 영역에 보내고, 트래픽 분석을 사용하여 Azure 클라우드의 트래픽 흐름에 대한 인사이트를 제공할 수 있습니다. 트래픽 분석의 장점 중 일부는 네트워크 활동을 시각화하고, 핫 스폿을 식별하며, 보안 위협을 식별하고, 트래픽 흐름 패턴을 이해하며, 잘못된 네트워크 구성을 파악할 수 있다는 것입니다.

Azure Database for PostgreSQL에 대한 Private Link를 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

NSG 흐름 로그를 사용하도록 설정하는 방법: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

트래픽 분석을 사용하도록 설정하고 사용하는 방법: https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="13-protect-critical-web-applications"></a>1.3: 중요한 웹 애플리케이션 보호

**지침**: 해당 없음. 이 추천 사항은 Azure App Service 또는 컴퓨팅 리소스에서 실행되는 웹 애플리케이션을 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: 알려진 악성 IP 주소와의 통신 거부

**지침**: Azure Database for PostgreSQL용 Advanced Threat Protection을 사용합니다. Advanced Threat Protection은 비정상적이며 잠재적으로 유해한 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상 활동을 탐지합니다.

DDoS 공격으로부터 보호하기 위해 Azure Database for PostgreSQL 인스턴스에 연결된 가상 네트워크에서 DDoS Protection 표준을 사용하도록 설정합니다. Azure Security Center 통합 위협 인텔리전스를 사용하여 알려진 악성 인터넷 IP 주소 또는 사용하지 않는 인터넷 IP 주소와의 통신을 거부합니다.

Azure Database for PostgreSQL용 Advanced Threat Protection을 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

DDoS Protection을 구성하는 방법: https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: 네트워크 패킷 및 흐름 로그 기록

**지침**: Azure Database for PostgreSQL 인스턴스를 프라이빗 엔드포인트에서 보호하는 경우 가상 머신을 동일한 가상 네트워크에 배포할 수 있습니다. 그런 다음, 데이터 반출의 위험을 줄이기 위해 NSG(네트워크 보안 그룹)를 구성할 수 있습니다. NSG 흐름 로그를 사용하도록 설정하고, 트래픽 감사를 위해 로그를 스토리지 계정에 보냅니다. 또한 NSG 흐름 로그를 Log Analytics 작업 영역에 보내고, 트래픽 분석을 사용하여 Azure 클라우드의 트래픽 흐름에 대한 인사이트를 제공할 수 있습니다. 트래픽 분석의 장점 중 일부는 네트워크 활동을 시각화하고, 핫 스폿을 식별하며, 보안 위협을 식별하고, 트래픽 흐름 패턴을 이해하며, 잘못된 네트워크 구성을 파악할 수 있다는 것입니다.

NSG 흐름 로그를 사용하도록 설정하는 방법: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

트래픽 분석을 사용하도록 설정하고 사용하는 방법: https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: 네트워크 기반 IDS/IPS(침입 탐지/침입 방지 시스템) 배포

**지침**: Azure Database for PostgreSQL용 Advanced Threat Protection을 사용합니다. Advanced Threat Protection은 비정상적이며 잠재적으로 유해한 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상 활동을 탐지합니다.

Azure Database for PostgreSQL용 Advanced Threat Protection을 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: 웹 애플리케이션에 대한 트래픽 관리

**지침**: 해당 없음. 이 추천 사항은 Azure App Service 또는 컴퓨팅 리소스에서 실행되는 웹 애플리케이션을 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: 네트워크 보안 규칙의 복잡성 및 관리 오버헤드 최소화

**지침**: Azure Database for PostgreSQL 인스턴스에 액세스해야 하는 리소스의 경우 가상 네트워크 서비스 태그를 사용하여 네트워크 보안 그룹 또는 Azure Firewall에서 네트워크 액세스 제어를 정의합니다. 보안 규칙을 만들 때 특정 IP 주소 대신 서비스 태그를 사용할 수 있습니다. 서비스 태그 이름(예: SQL.WestUs)을 규칙의 적절한 원본 또는 대상 필드에 지정하면 해당 서비스에 대한 트래픽을 허용하거나 거부할 수 있습니다. Microsoft는 서비스 태그에 포함되는 주소 접두사를 관리하고 주소가 변경되면 서비스 태그를 자동으로 업데이트합니다.

참고: Azure Database for PostgreSQL은 "Microsoft.Sql" 서비스 태그를 사용합니다.

서비스 태그를 사용하는 방법에 대한 자세한 내용: https://docs.microsoft.com/azure/virtual-network/service-tags-overview

Azure Database for PostgreSQL의 서비스 태그 사용 이해: https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet#terminology-and-description

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: 네트워크 디바이스에 대한 표준 보안 구성 유지 관리

**지침**: Azure Policy를 사용하여 Azure Database for PostgreSQL 인스턴스에 연결된 네트워크 설정 및 네트워크 리소스에 대한 표준 보안 구성을 정의하고 구현합니다. "Microsoft.DBforPostgreSQL" 및 "Microsoft.Network" 네임스페이스에서 Azure Policy 별칭을 사용하여 Azure Database for PostgreSQL 인스턴스에 대한 네트워크 구성을 감사하거나 적용하는 사용자 지정 정책을 만듭니다. 네트워킹 또는 Azure Database for PostgreSQL 인스턴스와 관련된 다음과 같은 기본 제공 정책 정의를 사용할 수도 있습니다.

- DDoS Protection 표준을 사용하도록 설정해야 합니다.

- TLS 연결 적용을 PostgreSQL 데이터베이스 서버에 사용하도록 설정해야 합니다.

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

네트워킹을 위한 Azure Policy 샘플: https://docs.microsoft.com/azure/governance/policy/samples/

Azure Blueprint를 만드는 방법: https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: 트래픽 구성 규칙 문서화

**지침**: Azure Database for PostgreSQL 인스턴스에 대한 네트워크 보안 및 트래픽 흐름에 관련된 리소스에 대한 태그를 사용하여 메타데이터 및 논리적 조직을 제공합니다.

태그 지정과 관련된 기본 제공 Azure Policy 정의(예: "태그 및 해당 값 필요")를 사용하여 모든 리소스에서 태그를 포함하도록 만들고 태그가 없는 기존 리소스를 알립니다.

Azure PowerShell 또는 Azure CLI를 사용하여 태그를 기준으로 리소스에 대한 작업을 조회하거나 수행할 수 있습니다.

태그를 만들고 사용하는 방법: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: 자동화된 도구를 사용하여 네트워크 리소스 구성 모니터링 및 변경 내용 검색

**지침**: Azure 활동 로그를 사용하여 네트워크 리소스 구성을 모니터링하고 Azure Database for PostgreSQL 인스턴스와 관련된 네트워크 리소스에 대한 변경을 검색합니다. Azure Monitor 내에서 중요한 네트워크 리소스가 변경되면 트리거되는 경고를 만듭니다.

Azure 활동 로그 이벤트를 확인하고 검색하는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor에서 경고를 만드는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

## <a name="logging-and-monitoring"></a>로깅 및 모니터링

*자세한 내용은 [보안 그룹: 로깅 및 모니터링](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)을 참조하세요.*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: 승인된 시간 동기화 원본 사용

**지침**: Microsoft는 로그의 타임스탬프에 대해 Azure Database for PostgreSQL과 같은 Azure 리소스에 사용되는 시간 원본을 유지 관리합니다.


**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: 중앙 보안 로그 관리 구성

**지침**: 진단 설정 및 서버 로그를 사용하도록 설정하고, 로그를 수집하여 Azure Database for PostgreSQL 인스턴스에서 생성된 보안 데이터를 집계합니다. Azure Monitor 내에서 Log Analytics 작업 영역을 사용하여 분석을 쿼리 및 수행하고, Azure Storage 계정을 장기/보관 스토리지에 사용할 수 있습니다. 또는 데이터를 사용하도록 설정하여 Azure Sentinel 또는 타사 SIEM에 온보딩할 수 있습니다.

Azure Database for PostgreSQL에 대한 서버 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

Azure Database for PostgreSQL에 대한 감사 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure Sentinel을 온보딩하는 방법: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure 리소스에 대한 감사 로깅 사용

**지침**: 감사, 보안 및 리소스 로그에 액세스하기 위해 Azure Database for PostgreSQL 인스턴스에서 진단 설정을 사용하도록 설정합니다. 특히 PostgreSQL 감사 로그를 사용하도록 설정해야 합니다. 자동으로 사용할 수 있는 활동 로그에는 이벤트 원본, 날짜, 사용자, 타임스탬프, 원본 주소, 대상 주소 및 기타 유용한 요소가 포함됩니다. Azure 활동 로그 진단 설정을 사용하도록 설정하고, 로그를 동일한 Log Analytics 작업 영역 또는 스토리지 계정에 보낼 수도 있습니다.

Azure Database for PostgreSQL에 대한 서버 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

Azure Database for PostgreSQL에 대한 감사 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure 활동 로그에 대한 진단 설정을 구성하는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: Customer

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: 운영 체제에서 보안 로그 수집

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.


**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="25-configure-security-log-storage-retention"></a>2.5: 보안 로그 스토리지 보존 기간 구성

**지침**: Azure Monitor 내에서 조직의 준수 규정에 따라 Azure Database for PostgreSQL 로그를 보관하는 데 사용되는 Log Analytics 작업 영역에 대한 보존 기간을 설정합니다. Azure Storage 계정을 장기/보관 스토리지에 사용합니다.

Log Analytics 작업 영역에 대한 로그 보존 기간 매개 변수를 설정하는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

Azure Storage 계정에 리소스 로그 저장: https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: 로그 모니터링 및 검토

**지침**: 비정상 동작에 대한 Azure Database for PostgreSQL 인스턴스의 로그를 분석하고 모니터링합니다. Azure Monitor의 Log Analytics를 사용하여 로그를 검토하고 로그 데이터에 대한 쿼리를 수행합니다. 또는 데이터를 사용하도록 설정하여 Azure Sentinel 또는 타사 SIEM에 온보딩할 수 있습니다.

Azure Sentinel을 온보딩하는 방법: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Log Analytics에 대한 자세한 내용: https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor에서 사용자 지정 쿼리를 수행하는 방법: https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: 비정상 활동에 대한 경고 사용

**지침**: Azure Database for PostgreSQL용 Advanced Threat Protection을 사용하도록 설정합니다. Advanced Threat Protection은 비정상적이며 잠재적으로 유해한 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상 활동을 탐지합니다.

또한 서버 로그 및 진단 설정을 PostgreSQL에 사용하도록 설정하고 로그를 Log Analytics 작업 영역에 보낼 수 있습니다. SOAR(보안 오케스트레이션 자동화 응답) 솔루션을 제공하므로 Log Analytics 작업 영역을 Azure Sentinel에 온보딩합니다. 이를 통해 플레이북(자동화된 솔루션)을 만들어 보안 문제를 수정하는 데 사용할 수 있습니다.

Azure Database for PostgreSQL용 Advanced Threat Protection을 사용하도록 설정하는 방법: https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

Azure Database for PostgreSQL에 대한 서버 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

Azure Database for PostgreSQL에 대한 감사 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure 활동 로그에 대한 진단 설정을 구성하는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

Azure Sentinel을 온보딩하는 방법: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: 맬웨어 방지 로깅 중앙 집중화

**지침**: 해당 없음. Azure Database for PostgreSQL은 맬웨어 방지 관련 로그를 처리하거나 생성하지 않습니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="29-enable-dns-query-logging"></a>2.9: DNS 쿼리 로깅 사용

**지침**: 해당 없음. Azure Database for PostgreSQL은 DNS 관련 로그를 처리하거나 생성하지 않습니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="210-enable-command-line-audit-logging"></a>2.10: 명령줄 감사 로깅 사용

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

## <a name="identity-and-access-control"></a>ID 및 Access Control

*자세한 내용은 [보안 그룹: ID 및 액세스 제어](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)를 참조하세요.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: 관리 계정의 인벤토리 유지 관리

**지침**: Azure Database for PostgreSQL 인스턴스의 컨트롤 플레인(예: Azure Portal)에 대한 관리 액세스 권한이 있는 사용자 계정의 인벤토리를 유지 관리합니다. 또한 Azure Database for PostgreSQL 인스턴스의 데이터 평면(데이터베이스 자체 내)에 대한 액세스 권한이 있는 관리 계정의 인벤토리를 유지 관리합니다. (PostgreSQL 서버를 만들 때 관리자 사용자에 대한 자격 증명을 제공합니다. 이 관리자는 추가 PostgreSQL 사용자를 만드는 데 사용할 수 있습니다.)

Azure Database for PostgreSQL은 기본 제공 역할 기반 액세스 제어를 지원하지 않지만, 특정 리소스 공급자 작업을 기반으로 하는 사용자 지정 역할을 만들 수 있습니다.

Azure 구독에 대한 사용자 지정 역할 이해: https://docs.microsoft.com/azure/role-based-access-control/custom-roles 

Azure Database for PostgreSQL 리소스 공급자 작업 이해: https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdbforpostgresql 

Azure Database for PostgreSQL에 대한 액세스 관리 이해: https://docs.microsoft.com/azure/postgresql/concepts-security#access-management

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: 기본 암호 변경(해당하는 경우)

**지침**: Azure Active Directory 및 Azure Database for PostgreSQL에는 기본 암호에 대한 개념이 없습니다.

Azure Database for PostgreSQL 리소스 자체를 만들 때는 Azure에서 강력한 암호를 사용하는 관리자를 만들도록 강제합니다. 그러나 PostgreSQL 인스턴스가 만들어지면 계정을 만든 첫 번째 서버 관리자 계정을 사용하여 추가 사용자를 만들고 해당 사용자에게 관리 액세스 권한을 부여할 수 있습니다. 이러한 계정을 만드는 경우 각 계정마다 서로 다른 강력한 암호를 구성해야 합니다.

Azure Database for PostgreSQL에 대한 추가 계정을 만드는 방법: https://docs.microsoft.com/azure/postgresql/howto-create-users

관리자 암호를 업데이트하는 방법: https://docs.microsoft.com/azure/postgresql/howto-create-manage-server-portal#update-admin-password

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: 전용 관리 계정 사용

**지침**: Azure Database for PostgreSQL 인스턴스에 액세스할 수 있는 전용 관리 계정을 사용하는 방법에 대한 표준 운영 절차를 만듭니다. Azure Security Center ID와 액세스 관리를 사용하여 관리 계정 수를 모니터링합니다. 

Azure Security Center ID 및 액세스 이해: https://docs.microsoft.com/azure/security-center/security-center-identity-access 

Azure Database for PostgreSQL에서 관리 사용자를 만드는 방법 이해: https://docs.microsoft.com/azure/postgresql/howto-create-users#the-server-admin-account


**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory에서 SSO(Single Sign-On) 사용

**지침**: 데이터베이스에 직접 구성된 사용자 이름/암호를 사용하는 방법 및 Azure AD(Active Directory) ID를 사용하고 연결할 Azure AD 토큰을 활용하는 방법 모두를 사용하여 Azure Database for PostgreSQL에 로그인할 수 있습니다. Azure AD 토큰을 사용하는 경우 Azure AD 사용자, Azure AD 그룹 또는 데이터베이스에 연결하는 Azure AD 애플리케이션과 같은 다양한 방법이 지원됩니다.

이와는 달리 PostgreSQL에 대한 컨트롤 플레인 액세스는 REST API를 통해 사용할 수 있으며 SSO를 지원합니다. 인증을 위해 요청에 대한 권한 부여 헤더를 Azure Active Directory에서 가져오는 JSON Web Token으로 설정합니다.

Azure Database for PostgreSQL 인증에 Azure Active Directory 사용: https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

Azure Database for PostgreSQL REST API 이해: https://docs.microsoft.com/rest/api/postgresql/

Azure AD를 사용한 SSO 이해: https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: 모든 Azure Active Directory 기반 액세스에 다단계 인증 사용

**지침**: Azure Active Directory MFA(Multi-Factor Authentication)를 사용하도록 설정하고, Azure Security Center ID 및 액세스 관리 추천 사항을 따릅니다. Azure AD 토큰을 활용하여 데이터베이스에 로그인하는 경우 데이터베이스 로그인에 대한 다단계 인증을 요구할 수 있습니다.

Azure에서 MFA를 사용하도록 설정하는 방법: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Database for PostgreSQL 인증에 Azure Active Directory 사용: https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

Azure Security Center 내에서 ID 및 액세스를 모니터링하는 방법: https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: 모든 관리 작업에 전용 컴퓨터(Privileged Access Workstation) 사용

**지침**: Azure 리소스에 로그인하여 구성하도록 구성된 MFA(Multi-Factor Authentication)를 통해 PAW(Privileged Access Workstation)를 사용합니다.

Privileged Access Workstation에 대한 자세한 정보: https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure에서 MFA를 사용하도록 설정하는 방법: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: 관리 계정의 의심스러운 활동에 대한 로그 및 경고

**지침**: 의심스러운 활동에 대한 경고를 생성하기 위해 Azure Database for PostgreSQL용 Advanced Threat Protection을 사용하도록 설정합니다.

또한 환경에서 의심스럽거나 안전하지 않은 활동이 발생하는 경우 로그와 경고를 생성하려면 Azure AD(Active Directory) PIM(Privileged Identity Management)을 사용할 수 있습니다.

Azure AD 위험 검색을 사용하여 위험한 사용자 동작에 대한 경고 및 보고서를 봅니다.

Azure Database for PostgreSQL용 Advanced Threat Protection을 설정하는 방법: https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

PIM(Privileged Identity Management)을 배포하는 방법: https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD 위험 검색 이해: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: 승인된 위치에서만 Azure 리소스 관리

**지침**: 조건부 액세스 명명된 위치를 사용하여 IP 주소 범위 또는 국가/지역의 특정 논리적 그룹화에서만 포털 및 Azure Resource Manager에 액세스할 수 있도록 허용합니다.

Azure에서 명명된 위치를 구성하는 방법: https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Azure Active Directory 사용

**지침**: Azure AD(Active Directory)를 중앙 인증 및 권한 부여 시스템으로 사용합니다. Azure AD는 강력한 암호화를 저장 데이터 및 전송 중 데이터에 사용하여 데이터를 보호합니다. 또한 Azure AD는 사용자 자격 증명을 솔트하고, 해시하고, 안전하게 저장합니다.

Azure Database for PostgreSQL에 로그인하려면 Azure AD를 사용하고 Azure AD 토큰을 사용하여 연결하는 것이 좋습니다. Azure AD 토큰을 사용하는 경우 Azure AD 사용자, Azure AD 그룹 또는 데이터베이스에 연결하는 Azure AD 애플리케이션과 같은 다양한 방법이 지원됩니다.

Azure AD 자격 증명은 PostgreSQL 관리자 계정을 제어하는 관리 평면 수준(예: Azure Portal)에서 관리하는 데 사용할 수도 있습니다.

Azure Database for PostgreSQL 인증에 Azure Active Directory 사용: https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: 정기적으로 사용자 액세스 검토 및 조정

**지침**: Azure Active Directory 로그를 검토하여 Azure Database for PostgreSQL 관리 역할이 있는 계정을 포함할 수 있는 부실 계정을 검색할 수 있습니다. 또한 Azure ID 액세스 검토를 사용하여 그룹 멤버 자격, Azure Database for PostgreSQL에 액세스하는 데 사용할 수 있는 엔터프라이즈 애플리케이션에 대한 액세스 및 역할 할당을 효율적으로 관리할 수 있습니다. 적절한 사용자만 계속 액세스할 수 있도록 사용자 액세스를 90일마다 정기적으로 검토해야 합니다.

Azure AD 보고 이해 https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure ID 액세스 검토를 사용하는 방법: https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

PostgreSQL 사용자 및 할당된 역할 검토: https://www.postgresql.org/docs/current/database-roles.html

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: 비활성화된 계정에 대한 액세스 시도 모니터링

**지침**: 모든 로그를 Log Analytics 작업 영역에 보내기 위해 진단 설정을 Azure Database for PostgreSQL 및 Azure Active Directory에 사용하도록 설정합니다. Log Analytics 내에서 원하는 경고(예: 인증 시도 실패)를 구성합니다.

Azure Database for PostgreSQL에 대한 서버 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

Azure Database for PostgreSQL에 대한 감사 로그를 구성하고 액세스하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure 활동 로그를 Azure Monitor에 통합하는 방법: https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: Customer

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: 계정 로그인 동작 편차에 대한 경고

**지침**: 의심스러운 활동에 대한 경고를 생성하기 위해 Azure Database for PostgreSQL용 Advanced Threat Protection을 사용하도록 설정합니다.

Azure Active Directory의 ID 보호 및 위험 검색 기능을 사용하여 탐지된 의심스러운 작업에 대해 자동화된 대응을 구성할 수 있습니다. 조직의 보안 대응을 구현하기 위해 Azure Sentinel을 통해 자동화된 대응을 사용하도록 설정할 수 있습니다.

추가 조사를 위해 로그를 Azure Sentinel로 수집할 수도 있습니다.

Azure Database for PostgreSQL용 Advanced Threat Protection을 설정하는 방법: https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

Azure AD ID 보호 개요: https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection

Azure AD 위험한 로그인을 확인하는 방법: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

Azure Sentinel을 온보딩하는 방법: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: 지원 시나리오에서 관련 고객 데이터에 대한 액세스 권한을 Microsoft에 제공

**지침**: 현재 사용할 수 없음. Customer Lockbox는 아직 Azure Database for PostgreSQL에서 지원되지 않습니다.

Customer Lockbox 지원 서비스 목록: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: Customer

## <a name="data-protection"></a>데이터 보호

*자세한 내용은 [보안 그룹: 데이터 보호](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)를 참조하세요.*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: 중요한 정보의 인벤토리 유지 관리

**지침**: 태그를 사용하여 중요한 정보를 저장하거나 처리하는 Azure Database for PostgreSQL 인스턴스를 추적할 수 있도록 지원합니다.

태그를 만들고 사용하는 방법: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: 중요한 정보를 저장하거나 처리하는 시스템 격리

**지침**: 개발, 테스트 및 프로덕션을 위한 별도의 구독 및/또는 관리 그룹을 구현합니다. Private Link, 서비스 엔드포인트 및/또는 방화벽 규칙의 조합을 사용하여 Azure Database for PostgreSQL 인스턴스에 대한 네트워크 액세스를 격리하고 제한합니다.

추가 Azure 구독을 만드는 방법: https://docs.microsoft.com/azure/billing/billing-create-subscription

관리 그룹을 만드는 방법: https://docs.microsoft.com/azure/governance/management-groups/create

Azure Database for PostgreSQL에 대한 Private Link를 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

Azure Database for PostgreSQL에서 VNet 서비스 엔드포인트 및 VNet 규칙을 만들고 관리하는 방법: https://docs.microsoft.com/azure/postgresql/howto-manage-vnet-using-portal

Azure Database for PostgreSQL 방화벽 규칙을 구성하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: 중요한 정보에 대한 무단 전송 모니터링 및 차단

**지침**: Azure 가상 머신을 사용하여 Azure Database for PostgreSQL 인스턴스에 액세스하는 경우 데이터 반출 가능성을 완화하기 위해 Private Link, PostgreSQL 네트워크 구성, 네트워크 보안 그룹 및 서비스 태그를 활용합니다.

Microsoft는 Azure Database for PostgreSQL에 대한 기본 인프라를 관리하고, 고객 데이터의 손실 또는 노출을 방지하기 위해 엄격한 제어를 구현했습니다.

Azure Database for PostgreSQL 데이터의 반출을 완화하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link

Azure의 고객 데이터 보호 이해: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Security Center 모니터링**: 예

**책임**: 공유됨

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: 전송 중인 모든 중요한 정보 암호화

**지침**: Azure Database for PostgreSQL은 이전에 SSL(Secure Sockets Layer)이라고 알려진 TLS(전송 계층 보안)를 사용하여 PostgreSQL 서버를 클라이언트 애플리케이션에 연결할 수 있도록 지원합니다. 데이터베이스 서버와 클라이언트 애플리케이션 간에 TLS 연결을 적용하면 서버와 애플리케이션 간의 데이터 스트림을 암호화하여 "메시지 가로채기(man in the middle)" 공격으로부터 보호할 수 있습니다. Azure Portal에서 기본적으로 모든 Azure Database for PostgreSQL 인스턴스에 대해 "SSL 연결 적용"을 사용하도록 설정되어 있는지 확인합니다.

Azure Database for PostgreSQL에 지원되는 TLS 버전은 현재 TLS 1.0, TLS 1.1, TLS 1.2입니다.

Azure Database for PostgreSQL에 대한 전송 중 암호화를 구성하는 방법: https://docs.microsoft.com/azure/postgresql/concepts-ssl-connection-security

**Azure Security Center 모니터링**: 예

**책임**: 공유됨

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: 활성 검색 도구를 사용하여 중요한 데이터 식별

**지침**: Azure Database for PostgreSQL에서는 아직 데이터 식별, 분류 및 손실 방지 기능을 사용할 수 없습니다. 규정 준수를 위해 필요한 경우 타사 솔루션을 구현합니다.

Microsoft에서 관리하는 기본 플랫폼의 경우 Microsoft는 모든 고객 콘텐츠를 중요한 것으로 간주하고, 고객 데이터 손실 및 노출을 방지하기 위해 모든 노력을 다하고 있습니다. Azure 내에서 고객 데이터를 안전하게 유지하기 위해 Microsoft는 강력한 데이터 보호 제어 및 기능 모음을 구현하고 유지 관리합니다.

Azure의 고객 데이터 보호 이해: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: 공유됨

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Azure RBAC를 사용하여 리소스에 대한 액세스 제어

**지침**: Azure RBAC(역할 기반 액세스 제어)를 사용하여 Azure Database for PostgreSQL 컨트롤 플레인(예: Azure Portal)에 대한 액세스를 제어합니다. 데이터 평면 액세스(데이터베이스 자체 내)의 경우 SQL 쿼리를 사용하여 사용자를 만들고 사용자 권한을 구성합니다. RBAC는 데이터베이스 내의 사용자 권한에 영향을 주지 않습니다.

Azure에서 RBAC를 구성하는 방법: https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

SQL을 사용하여 Azure Database for PostgreSQL에 대한 사용자 액세스를 구성하는 방법: https://docs.microsoft.com/azure/postgresql/howto-create-users

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: 호스트 기반 데이터 손실 방지를 사용하여 액세스 제어 적용

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

Microsoft는 Azure Database for PostgreSQL에 대한 기본 인프라를 관리하고, 고객 데이터의 손실 또는 노출을 방지하기 위해 엄격한 제어를 구현했습니다.

Azure의 고객 데이터 보호 이해: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: 중요한 저장 정보 암호화

**지침**: Azure Database for PostgreSQL 서비스는 미사용 데이터의 스토리지 암호화를 위해 FIPS 140-2 유효성 검사 암호화 모듈을 사용합니다. 백업이 포함된 데이터는 디스크에서 암호화되며, 쿼리를 실행하는 동안 만든 임시 파일은 제외됩니다. 이 서비스는 Azure 스토리지 암호화에 포함된 AES 256비트 암호화를 사용하며, 키는 시스템에서 관리됩니다. 스토리지 암호화는 항상 켜져 있고 해제할 수 없습니다.

CMK(고객 관리형 키)를 Azure Database for PostgreSQL 단일 서버에 사용하는 데이터 암호화를 사용하면 저장 데이터 보호에 대한 고유한 키(BYOK)를 가져올 수 있습니다. 현재 이 기능을 사용하려면 액세스를 요청해야 합니다. 이렇게 하려면 다음에 문의하세요.

AskAzureDBforPostgreSQL@service.microsoft.com입니다.

Azure Database for PostgreSQL의 저장 데이터 암호화 이해: https://docs.microsoft.com/azure/postgresql/concepts-security

고객 관리형 키를 사용한 Azure Database for PostgreSQL의 저장 데이터 암호화 이해: https://docs.microsoft.com/azure/postgresql/concepts-data-encryption-postgresql


**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: 중요한 Azure 리소스에 대한 변경 내용 로그 및 경고

**지침**: Azure Monitor에서 Azure 활동 로그를 사용하여 Azure Database for PostgreSQL의 프로덕션 인스턴스 및 기타 중요하거나 관련된 리소스에 변경되는 경우에 대한 경고를 만듭니다.

Azure 활동 로그 이벤트에 대한 경고를 만드는 방법: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

## <a name="vulnerability-management"></a>취약성 관리

*자세한 내용은 [보안 그룹: 취약성 관리](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)를 참조하세요.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: 자동화된 취약성 검사 도구 실행

**지침**: 현재 사용할 수 없음. Azure Security Center는 Azure Database for PostgreSQL에 대한 취약성 평가를 아직 지원하지 않습니다.

Azure Security Center의 Azure PaaS 서비스에 대한 기능 적용 범위: https://docs.microsoft.com/azure/security-center/features-paas

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: Customer

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: 자동화된 운영 체제 패치 관리 솔루션 배포

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: 자동화된 타사 소프트웨어 패치 관리 솔루션 배포

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: 연속 취약성 검사 비교

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: 위험 등급 프로세스를 사용하여 검색된 취약성의 수정 우선 순위 지정

**지침**: Microsoft는 Azure Database for PostgreSQL을 지원하는 기본 시스템에서 취약성 관리를 수행합니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

## <a name="inventory-and-asset-management"></a>인벤토리 및 자산 관리

*자세한 내용은 [보안 그룹: 인벤토리 및 자산 관리](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)를 참조하세요.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure 자산 검색 사용

**지침**: Azure Resource Graph를 사용하여 구독 내의 모든 리소스(Azure Database for PostgreSQL 인스턴스 포함)를 쿼리하고 검색합니다. 테넌트에서 적절한 권한(읽기)이 있는지 확인하고, 모든 Azure 구독 및 구독 내의 리소스를 열거할 수 있습니다.

Azure Resource Graph를 사용하여 쿼리를 만드는 방법: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure 구독을 확인하는 방법: https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC 이해: https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: 자산 메타데이터 유지 관리

**지침**: 메타데이터를 제공하는 Azure Database for PostgreSQL 인스턴스 및 기타 관련 리소스에 태그를 적용하여 논리적인 분류 체계로 구성합니다.

태그를 만들고 사용하는 방법: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: 권한 없는 Azure 리소스 삭제

**지침**: 해당하는 경우 태그 지정, 관리 그룹 및 별도의 구독을 사용하여 Azure Database for PostgreSQL 인스턴스 및 관련 리소스를 구성하고 추적합니다. 정기적으로 인벤토리를 조정하고, 구독에서 권한 없는 리소스가 적시에 삭제되도록 합니다.

추가 Azure 구독을 만드는 방법: https://docs.microsoft.com/azure/billing/billing-create-subscription

관리 그룹을 만드는 방법: https://docs.microsoft.com/azure/governance/management-groups/create

태그를 만들고 사용하는 방법: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: 승인된 Azure 리소스 및 소프트웨어 타이틀의 인벤토리 유지 관리

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스와 Azure 전체를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: 승인되지 않은 Azure 리소스 모니터링

**지침**: Azure Policy에서 다음 기본 제공 정책 정의를 사용하여 고객 구독에서 만들 수 있는 리소스 종류를 제한합니다.

- 허용되지 않는 리소스 종류

- 허용되는 리소스 유형

또한 Azure Resource Graph를 사용하여 구독 내에서 리소스를 쿼리/검색합니다.

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph를 사용하여 쿼리를 만드는 방법: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: 컴퓨팅 리소스 내에서 승인되지 않은 소프트웨어 애플리케이션 모니터링

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: 승인되지 않은 Azure 리소스 및 소프트웨어 애플리케이션 제거

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스와 Azure 전체를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="68-use-only-approved-applications"></a>6.8: 승인된 애플리케이션만 사용

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="69-use-only-approved-azure-services"></a>6.9: 승인된 Azure 서비스만 사용

**지침**: Azure Policy에서 다음 기본 제공 정책 정의를 사용하여 고객 구독에서 만들 수 있는 리소스 종류를 제한합니다.

- 허용되지 않는 리소스 종류

- 허용되는 리소스 유형

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Policy를 사용하여 특정 리소스 종류를 거부하는 방법: https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="610-implement-approved-application-list"></a>6.10: 승인된 애플리케이션 목록 구현

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: 사용자가 스크립트를 통해 Azure Resource Manager와 상호 작용하는 기능 제한

**지침**: "Microsoft Azure 관리" 앱에 대한 "액세스 차단"을 구성하여 사용자가 Azure Resource Manager와 상호 작용하는 기능을 제한하려면 Azure 조건부 액세스를 사용합니다. 이렇게 하면 중요한 정보가 포함된 Azure Database for PostgreSQL 인스턴스와 같이 보안 수준이 높은 환경 내에서 리소스를 만들고 변경하지 못하도록 방지할 수 있습니다.

Azure Resource Manager에 대한 액세스를 차단하도록 조건부 액세스를 구성하는 방법: https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: 사용자가 컴퓨팅 리소스 내에서 스크립트를 실행하는 기능 제한

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: 위험 수준이 높은 애플리케이션을 물리적 또는 논리적으로 분리

**지침**: 해당 없음. 이 추천 사항은 Azure App Service 또는 컴퓨팅 리소스에서 실행되는 웹 애플리케이션을 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

## <a name="secure-configuration"></a>보안 구성

*자세한 내용은 [보안 그룹: 보안 구성](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)을 참조하세요.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: 모든 Azure 리소스에 대한 보안 구성 설정

**지침**: Azure Policy를 사용하여 Azure Database for PostgreSQL 인스턴스에 대한 표준 보안 구성을 정의하고 구현합니다. "Microsoft.DBforPostgreSQL" 네임스페이스에서 Azure Policy 별칭을 사용하여 Azure Database for PostgreSQL 인스턴스에 대한 네트워크 구성을 감사하거나 적용하는 사용자 지정 정책을 만듭니다. Azure Database for PostgreSQL 인스턴스와 관련된 다음과 같은 기본 제공 정책 정의를 사용할 수도 있습니다.

- TLS 연결 적용을 PostgreSQL 데이터베이스 서버에 사용하도록 설정해야 합니다.

- PostgreSQL 데이터베이스 서버에 대해 로그 연결을 사용하도록 설정해야 합니다.

사용 가능한 Azure 정책 별칭을 확인하는 방법: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: 보안 운영 체제 구성 설정

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: 보안 Azure 리소스 구성 유지 관리

**지침**: [거부] 및 [존재하지 않으면 배포] Azure 정책을 사용하여 보안 설정을 Azure 리소스 전체에 적용합니다.

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Policy 효과 이해: https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: 보안 운영 체제 구성 유지 관리

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure 리소스 구성을 안전하게 저장

**지침**: 사용자 지정 Azure Policy 정의를 Azure Database for PostgreSQL 인스턴스 및 관련 리소스에 사용하는 경우 Azure Repos를 사용하여 코드를 안전하게 저장하고 관리합니다.

코드를 Azure DevOps에 저장하는 방법: https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos 설명서: https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: 사용자 지정 운영 체제 이미지를 안전하게 저장

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: 시스템 구성 관리 도구 배포

**지침**: "Microsoft. DBforPostgreSQL" 네임스페이스에서 Azure Policy 별칭을 사용하여 시스템 구성을 경고, 감사 및 적용하는 사용자 지정 정책을 만듭니다. 또한 정책 예외를 관리하는 프로세스와 파이프라인을 개발합니다.

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: 운영 체제용 시스템 구성 관리 도구 배포

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure 서비스에 대한 자동화된 구성 모니터링 구현

**지침**: "Microsoft. DBforPostgreSQL" 네임스페이스에서 Azure Policy 별칭을 사용하여 시스템 구성을 경고, 감사 및 적용하는 사용자 지정 정책을 만듭니다. Azure Policy의 [감사], [거부] 및 [없는 경우 배포] 정책을 사용하여 Azure Database for PostgreSQL 인스턴스 및 관련 리소스에 대한 구성을 자동으로 적용합니다.

Azure Policy를 구성하고 관리하는 방법: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: 운영 체제에 대한 자동화된 구성 모니터링 구현

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 해당 없음

### <a name="711-manage-azure-secrets-securely"></a>7.11: 안전하게 Azure 비밀 관리

**지침**: Azure Database for PostgreSQL 인스턴스에 액세스하는 데 사용되는 Azure App Service에서 실행되는 Azure Virtual Machines 또는 웹 애플리케이션의 경우 Azure Key Vault와 함께 관리되는 서비스 ID를 사용하여 Azure Database for PostgreSQL 비밀 관리를 간소화하고 보호합니다. Key Vault 일시 삭제를 사용하도록 설정되어 있는지 확인합니다.

Azure 관리 ID와 통합하는 방법: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault를 만드는 방법: https://docs.microsoft.com/azure/key-vault/quick-create-portal

관리 ID를 사용하여 Key Vault 인증을 제공하는 방법: https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: 안전하게 자동으로 ID 관리

**지침**: Azure Database for PostgreSQL 서버는 데이터베이스에 액세스하기 위한 Azure Active Directory 인증을 지원합니다.  Azure Database for PostgreSQL 서버를 만드는 동안 관리자에 대한 자격 증명을 제공합니다. 이 관리자는 추가 데이터베이스 사용자를 만드는 데 사용할 수 있습니다.  

Azure Database for PostgreSQL 서버에 액세스하는 데 사용되는 Azure App Service에서 실행되는 Azure Virtual Machines 또는 웹 애플리케이션의 경우 Azure Key Vault와 함께 관리되는 서비스 ID를 사용하여 Azure Database for PostgreSQL 서버에 대한 자격 증명을 저장하고 검색합니다. Key Vault 일시 삭제를 사용하도록 설정되어 있는지 확인합니다.

관리 ID를 사용하여 Azure AD(Active Directory)에서 자동으로 관리되는 ID를 Azure 서비스에 제공합니다. 관리 ID를 사용하면 코드에 자격 증명 없이 Key Vault를 포함하여 Azure AD 인증을 지원하는 모든 서비스에 인증할 수 있습니다.

관리 ID를 구성하는 방법: https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure 관리 ID와 통합하는 방법: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: 의도하지 않은 자격 증명 노출 제거

**지침**: 자격 증명 스캐너를 구현하여 코드 내에서 자격 증명을 식별합니다. 또한 자격 증명 스캐너는 검색된 자격 증명을 더 안전한 위치(예: Azure Key Vault)로 이동하도록 추천합니다.

자격 증명 스캐너를 설정하는 방법: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

## <a name="malware-defense"></a>맬웨어 방어

*자세한 내용은 [보안 그룹: 맬웨어 방어](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)를 참조하세요.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: 중앙 관리형 맬웨어 방지 소프트웨어 사용

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

Microsoft 맬웨어 방지는 Azure 서비스(예: Azure App Service)를 지원하는 기본 호스트에서 사용하도록 설정되어 있지만 고객 콘텐츠에서 실행되지 않습니다.


**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: 비 컴퓨팅 Azure 리소스에 업로드할 파일 미리 검사

**지침**: Microsoft 맬웨어 방지는 Azure 서비스(예: Azure Database for PostgreSQL)를 지원하는 기본 호스트에서 사용하도록 설정되어 있지만 고객 콘텐츠에서 실행되지 않습니다.

비 컴퓨팅 Azure 리소스(예: App Service, Data Lake Storage, Blob Storage, Azure Database for PostgreSQL 등)로 업로드되는 콘텐츠를 미리 검사합니다. Microsoft는 이러한 인스턴스의 데이터에 액세스할 수 없습니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: 공유됨

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: 맬웨어 방지 소프트웨어 및 서명이 업데이트되었는지 확인

**지침**: 해당 없음. 이 추천 사항은 컴퓨팅 리소스를 위한 것입니다.

Microsoft 맬웨어 방지는 Azure 서비스(예: Azure Database for PostgreSQL)를 지원하는 기본 호스트에서 사용하도록 설정되어 있지만 고객 콘텐츠에서 실행되지 않습니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: Microsoft

## <a name="data-recovery"></a>데이터 복구

*자세한 내용은 [보안 그룹: 데이터 복구](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)를 참조하세요.*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: 자동화된 정기 백업 보장

**지침**: Azure Database for PostgreSQL은 데이터 파일과 트랜잭션 로그를 백업합니다. 지원되는 최대 스토리지 크기에 따라 전체 및 차등 백업(최대 4TB 스토리지 서버) 또는 스냅샷 백업(최대 16TB 스토리지 서버)을 수행합니다. 이러한 백업을 사용하면 서버를 구성된 백업 보존 기간 내의 특정 시점으로 복원할 수 있습니다. 기본 백업 보존 기간은 7일입니다. 필요에 따라 최대 35일까지 구성할 수 있습니다. 모든 백업은 AES 256비트 암호화를 사용하여 암호화됩니다.

Azure Database for PostgreSQL에서 서버를 백업하는 방법: https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

Azure Database for PostgreSQL 초기 구성 이해: https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-portal

**Azure Security Center 모니터링**: 해당 없음

**책임**: 공유됨

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: 전체 시스템 백업 수행 및 고객 관리형 키 백업

**지침**: Azure Database for PostgreSQL은 자동으로 서버 백업을 만들어 사용자의 선택에 따라 로컬 중복 또는 지역 중복 스토리지에 저장합니다. 백업을 사용하여 특정 시점의 서버를 복원할 수 있습니다. 백업 및 복원은 실수로 인한 손상이나 삭제로부터 데이터를 보호하므로 비즈니스 연속성 전략의 필수적인 부분입니다.

Azure Key Vault를 사용하여 Azure Database for PostgreSQL 인스턴스에 대한 자격 증명을 저장하는 경우 자동화된 키 백업을 정기적으로 수행해야 합니다.

Azure Database for PostgreSQL에서 서버를 백업하는 방법: https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

Key Vault 키를 백업하는 방법: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Security Center 모니터링**: 현재 사용할 수 없음

**책임**: 공유됨

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: 고객 관리형 키를 포함한 모든 백업의 유효성 검사

**지침**: Azure Database for PostgreSQL에서 복원을 수행하면 원래 서버의 백업에서 새 서버가 만들어집니다. 사용할 수 있는 두 가지 유형의 복원으로 특정 시점 복원 및 지리적 복원이 있습니다. 특정 시점 복원은 백업 중복 옵션을 통해 사용할 수 있으며, 새 서버를 원본 서버와 동일한 지역에 만듭니다. 지리적 복원은 서버를 지역 중복 스토리지로 구성하고, 이 서버를 다른 지역으로 복원할 수 있는 경우에만 사용할 수 있습니다.

예상 복구 시간은 데이터베이스 크기, 트랜잭션 로그 크기, 네트워크 대역폭 및 동일한 지역에서 동시에 복구되는 데이터베이스의 총 수를 포함한 여러 요소에 따라 달라집니다. 복구 시간은 일반적으로 12시간 미만입니다.

Azure Database for PostgreSQL 인스턴스의 복원을 정기적으로 테스트합니다.

Azure Database for PostgreSQL에서 서버를 백업하는 방법: https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: 백업 및 고객 관리형 키 보호 보장

**지침**: Azure Database for PostgreSQL은 전체, 차등 및 트랜잭션 로그 백업을 수행합니다. 이러한 백업을 사용하면 서버를 구성된 백업 보존 기간 내의 특정 시점으로 복원할 수 있습니다. 기본 백업 보존 기간은 7일입니다. 필요에 따라 최대 35일까지 구성할 수 있습니다. 모든 백업은 AES 256비트 암호화를 사용하여 암호화됩니다.

Azure Database for PostgreSQL의 백업 및 복원 이해: https://docs.microsoft.com/azure/postgresql/concepts-backup

**Azure Security Center 모니터링**: 예

**책임**: Customer

## <a name="incident-response"></a>사고 대응

*자세한 내용은 [보안 그룹: 인시던트 대응](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)을 참조하세요.*

### <a name="101-create-an-incident-response-guide"></a>10.1: 인시던트 대응 지침 만들기

**지침**: 조직에 대한 인시던트 대응 지침을 작성합니다. 탐지에서 인시던트 사후 검토까지의 인시던트 처리/관리 단계뿐만 아니라 담당자의 모든 역할도 정의하는 인시던트 대응 계획이 작성되어 있는지 확인합니다.

Azure Security Center 내에서 워크플로 자동화를 구성하는 방법: https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

사용자 고유의 보안 인시던트 대응 프로세스를 구축하는 방법에 대한 지침: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft 보안 대응 센터의 인시던트 분석: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

고객이 NIST의 컴퓨터 보안 인시던트 처리 가이드를 활용하여 고유한 인시던트 대응 계획 수립을 지원할 수도 있음: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: 인시던트 점수 매기기 및 우선 순위 지정 절차 만들기

**지침**: Security Center에서 심각도를 각 경고에 할당하여 먼저 조사해야 하는 경고에 대한 우선 순위를 지정합니다. 심각도는 Security Center에서 경고를 실행하는 데 사용된 결과 또는 분석의 신뢰도 및 경고가 발생된 활동의 배후에 악의적인 의도가 있었음에 대한 신뢰 수준을 기준으로 합니다.

또한 구독(예: 프로덕션, 비 프로덕션)을 명확하게 표시하고 Azure 리소스를 명확하게 식별하고 분류하는 명명 시스템을 만듭니다.

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: 보안 대응 프로시저 테스트

**지침**: 시스템의 인시던트 대응 기능을 정기적으로 테스트합니다. 약점과 결함을 식별하고 필요에 따라 계획을 수정합니다.

NIST 게시물: IT 계획 및 기능에 대한 테스트, 학습 및 연습 프로그램에 대한 가이드 참조: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: 보안 인시던트 연락처 세부 정보 제공 및 보안 인시던트에 대한 경고 알림 구성

**지침**: MSRC(Microsoft 보안 대응 센터)에서 불법적이거나 권한이 없는 당사자가 고객 데이터에 액세스했다고 검색하는 경우 Microsoft에서 보안 인시던트 연락처 정보를 사용하여 사용자에게 연락합니다.  문제가 해결되었는지 확인하기 위해 사후에 인시던트를 검토합니다.

Azure Security Center 보안 연락처를 설정하는 방법: https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: 보안 경고를 인시던트 대응 시스템에 통합

**지침**: 연속 내보내기 기능을 사용하여 Azure Security Center 경고 및 추천 사항을 내보냅니다. 연속 내보내기를 사용하면 경고 및 추천 사항을 수동으로 또는 지속적으로 내보낼 수 있습니다. Azure Security Center 데이터 커넥터를 사용하여 경고를 Sentinel로 스트림할 수 있습니다.

연속 내보내기를 구성하는 방법: https://docs.microsoft.com/azure/security-center/continuous-export

경고를 Azure Sentinel로 스트림하는 방법: https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: 보안 경고에 대한 대응 자동화

**지침**: Azure Security Center의 워크플로 자동화 기능을 사용하여 "Logic Apps"를 통해 보안 경고 및 추천 사항에 대한 대응을 자동으로 트리거합니다.

워크플로 자동화 및 Logic Apps를 구성하는 방법: https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Security Center 모니터링**: 해당 없음

**책임**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>침투 테스트 및 레드 팀 연습

*자세한 내용은 [보안 그룹: 침투 테스트 및 레드 팀 연습](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)을 참조하세요.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure 리소스에 대한 침투 테스트를 정기적으로 수행 및 모든 중요한 보안 결과를 60일 이내에 수정

**지침**: 다음 Microsoft 시행 규칙에 따라 침투 테스트에서 Microsoft 정책을 위반하지 않는지 확인합니다. https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

다음 백서에서는 Microsoft에서 관리하는 클라우드 인프라, 서비스 및 애플리케이션에 대한 Microsoft의 Red Teaming 및 라이브 사이트 침투 테스트 전략 및 실행에 대한 자세한 내용을 확인할 수 있습니다. https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Security Center 모니터링**: 해당 없음

**책임**: 공유됨

## <a name="next-steps"></a>다음 단계

- [Azure 보안 벤치마크](https://docs.microsoft.com/azure/security/benchmarks/overview)를 참조하세요.
- [Azure 보안 기준](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)에 대해 자세히 알아보세요.
