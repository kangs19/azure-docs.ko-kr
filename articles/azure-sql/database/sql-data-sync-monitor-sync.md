---
title: Azure Monitor 로그를 사용하여 SQL 데이터 동기화 모니터링
description: Azure Monitor 로그를 사용 하 여 SQL 데이터 동기화를 모니터링 하는 방법을 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: 307e501743d01b94cfca3692cc09c05cc90ed3ce
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84343237"
---
# <a name="monitor-sql-data-sync-with-azure-monitor-logs"></a>Azure Monitor 로그를 사용하여 SQL 데이터 동기화 모니터링 

이전에는 SQL 데이터 동기화 활동 로그를 확인하고 오류와 경고를 검색하기 위해 Azure Portal에서 수동으로 SQL 데이터 동기화를 확인하거나 PowerShell 또는 REST API를 사용해야 했습니다. 이 문서의 단계에 따라 데이터 동기화 모니터링 환경을 향상시키는 사용자 지정 솔루션을 구성합니다. 시나리오에 맞게 이 솔루션을 사용자 지정할 수 있습니다.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

SQL 데이터 동기화에 대한 개요는 [Azure의 SQL 데이터 동기화를 사용하여 여러 클라우드 및 온-프레미스 데이터베이스에서 데이터 동기화](sql-data-sync-data-sql-server-sql-database.md)를 참조하세요.

> [!IMPORTANT]
> SQL 데이터 동기화는 현재 Azure SQL Managed Instance를 지원 **하지** 않습니다.

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>모든 동기화 그룹에 대 한 모니터링 대시보드 

더 이상 문제를 찾기 위해 각 동기화 그룹의 로그를 개별적으로 볼 필요가 없습니다. 사용자 지정 Azure Monitor 보기를 사용 하 여 모든 구독에서 모든 동기화 그룹을 한 곳에서 모니터링할 수 있습니다. 이 보기는 SQL 데이터 동기화 고객에게 중요한 정보를 표시합니다.

![데이터 동기화 모니터링 대시보드](./media/sql-data-sync-monitor-sync/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>자동화된 전자 메일 알림

Azure Portal이나 PowerShell 또는 REST API를 통해 로그를 수동으로 확인할 필요가 없습니다. [Azure Monitor 로그](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)를 사용 하면 오류가 발생할 때이를 확인 해야 하는 사람의 전자 메일 주소로 직접 이동 하는 경고를 만들 수 있습니다.

![데이터 동기화 전자 메일 알림](./media/sql-data-sync-monitor-sync/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>이런 모니터링 기능은 어떻게 설정하나요? 

다음 작업을 수행 하 여 한 시간 이내에 SQL 데이터 동기화에 대 한 사용자 지정 Azure Monitor 로그 모니터링 솔루션을 구현 합니다.

세 가지 구성 요소를 구성해야 합니다.

-   SQL 데이터 동기화 로그 데이터를 Azure Monitor 로그에 공급할 PowerShell runbook입니다.

-   전자 메일 알림에 대 한 Azure Monitor 경고입니다.

-   모니터링에 대 한 Azure Monitor 뷰입니다.

### <a name="samples-to-download"></a>다운로드할 샘플

다음 두 가지 샘플을 다운로드합니다.

-   [데이터 동기화 로그 PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [데이터 동기화 Azure Monitor 보기](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>사전 요구 사항

다음 항목을 설정했는지 확인합니다.

-   Azure Automation 계정

-   Log Analytics 작업 영역

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>SQL 데이터 동기화 로그를 가져오는 PowerShell Runbook 

Azure Automation에서 호스트 되는 PowerShell runbook을 사용 하 여 SQL 데이터 동기화 로그 데이터를 가져오고 Azure Monitor 로그에 보냅니다. 샘플 스크립트가 포함되어 있습니다. 필수 구성 요소로서 Azure Automation 계정이 있어야 합니다. 그런 다음 Runbook을 만들고 실행 일정을 예약해야 합니다. 

### <a name="create-a-runbook"></a>Runbook 만들기

Runbook을 만드는 방법에 대한 자세한 내용은 [내 첫 번째 PowerShell Runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell)을 참조하세요.

1.  Azure Automation 계정의 [프로세스 자동화] 아래에서 **Runbook** 탭을 선택합니다.

2.  Runbook 페이지의 왼쪽 위 모서리에 있는 **Runbook 추가** 를 선택 합니다.

3.  **기존 Runbook 가져오기**를 선택합니다.

4.  **Runbook 파일** 아래에서 지정된 `DataSyncLogPowerShellRunbook` 파일을 사용합니다. **Runbook 형식**을 `PowerShell`로 설정합니다. Runbook에 이름을 지정합니다.

5.  **만들기**를 선택합니다. 이제 Runbook이 있습니다.

6.  Azure Automation 계정의 [공유 리소스] 아래에서 **변수** 탭을 선택합니다.

7.  [변수] 페이지에서 **변수 추가**를 선택합니다. Runbook의 마지막 실행 시간을 저장하는 변수를 만듭니다. 여러 개의 Runbook이 있는 경우 각 Runbook마다 하나의 변수가 필요합니다.

8.  변수 이름을 `DataSyncLogLastUpdatedTime`으로 설정하고, Type을 DateTime으로 설정합니다.

9.  Runbook을 선택하고 페이지 맨 위에 있는 편집 단추를 클릭합니다.

10. 계정 및 SQL 데이터 동기화 구성에 필요한 대로 변경합니다. (자세한 내용은 샘플 스크립트를 참조하세요.)

    1.  Azure 정보

    2.  동기화 그룹 정보입니다.

    3.  Azure Monitor는 정보를 기록 합니다. Azure Portal |에서이 정보를 찾습니다. 설정 | 연결 된 원본. Azure Monitor 로그에 데이터를 보내는 방법에 대 한 자세한 내용은 [HTTP 데이터 수집기 API (미리 보기)를 사용 하 여 Azure Monitor 로그에 데이터 전송](../../azure-monitor/platform/data-collector-api.md)을 참조 하세요.

11. [테스트] 창에서 Runbook을 실행합니다. 성공적으로 실행되었는지 확인합니다.

    오류가 있는 경우 최신 PowerShell 모듈을 설치해야 합니다. Automation 계정으로 **모듈 갤러리**에서 최신 PowerShell 모듈을 설치할 수 있습니다.

12. **게시** 클릭

### <a name="schedule-the-runbook"></a>Runbook 일정 예약

Runbook 일정을 예약하려면 다음을 수행합니다.

1.  Runbook의 [리소스] 아래에서 **일정** 탭을 선택합니다.

2.  [일정] 페이지에서 **일정 추가**를 선택합니다.

3.  **Runbook에 일정 연결을**선택 합니다.

4.  **새 일정 만들기를 선택 합니다.**

5.  **되풀이**를 '되풀이'로 설정하고 원하는 간격을 설정합니다. 스크립트와 Azure Monitor 로그에서 동일한 간격을 사용 합니다.

6.  **만들기**를 선택합니다.

### <a name="check-the-automation"></a>자동화 확인

자동화가 예상대로 실행되는지를 모니터링하려면 자동화 계정의 **개요**에서 **모니터링** 아래의 **작업 통계**보기를 찾습니다. 이 보기를 쉽게 볼 수 있도록 대시보드에 고정합니다. 성공적인 Runbook 실행은 "완료"로 표시되고, 실패한 실행은 "실패"로 표시됩니다.

## <a name="create-an-azure-monitor-reader-alert-for-email-notifications"></a>전자 메일 알림에 대 한 Azure Monitor 판독기 경고 만들기

Azure Monitor 로그를 사용 하는 경고를 만들려면 다음 작업을 수행 합니다. 필수 구성 요소로 Azure Monitor 로그가 Log Analytics 작업 영역에 연결 되어 있어야 합니다.

1.  Azure Portal에서 **Log Search**를 선택합니다.

2.  선택한 간격 내에서 동기화 그룹별로 오류와 경고를 선택하는 쿼리를 만듭니다. 예를 들어:

    `DataSyncLog_CL | where LogLevel_s != "Success" | summarize AggregatedValue = count() by bin(TimeGenerated,60m),SyncGroupName_s`

3.  쿼리를 실행한 후 **경고**라고 말하는 벨을 선택합니다.

4.  **경고 발생 기준** 아래에서 **메트릭 측정**을 선택합니다.

    1.  [집계 값]을 **다음 값보다 큼**으로 설정합니다.

    2.  알림을 받기 전에 경과할 임계값을 **다음 값보다 큼** 뒤에 입력합니다. 데이터 동기화에서 일시적인 오류가 발생 합니다. 노이즈를 줄이려면 임계값을 5로 설정 합니다.

5.  **작업** 아래에서 **전자 메일 알림**을 "예"로 설정합니다. 원하는 전자 메일 받는 사람을 입력합니다.

6.  **저장**을 클릭합니다. 이제 오류가 발생하면 지정된 받는 사람이 전자 메일 알림을 받습니다.

## <a name="create-an-azure-monitor-view-for-monitoring"></a>모니터링할 Azure Monitor 보기 만들기

이 단계에서는 지정 된 모든 동기화 그룹을 시각적으로 모니터링 하는 Azure Monitor 보기를 만듭니다. 이 보기에는 다음과 같은 몇 가지 구성 요소가 포함되어 있습니다.

-   모든 동기화 그룹에 있는 오류, 성공 및 경고 수를 보여 주는 개요 타일

-   동기화 그룹당 오류 및 경고 수를 보여 주는 모든 동기화 그룹 타일 - 문제가 없는 그룹은 이 타일에 표시되지 않습니다.

-   오류, 성공 및 경고 수 및 최근 오류 메시지를 표시 하는 각 동기화 그룹의 타일입니다.

Azure Monitor 보기를 구성 하려면 다음 작업을 수행 합니다.

1.  Log Analytics 작업 영역 홈 페이지에서 왼쪽의 더하기를 선택 하 여 **뷰 디자이너**를 엽니다.

2.  뷰 디자이너의 위쪽 표시줄에서 **가져오기**를 선택합니다. 그런 다음 "DataSyncLogOMSView" 샘플 파일을 선택합니다.

3.  샘플 보기는 두 개의 동기화 그룹을 관리하기 위한 것입니다. 이 보기를 시나리오에 맞게 편집합니다. **편집**을 클릭하고 다음과 같이 변경합니다.

    1.  필요에 따라 [갤러리]에서 새 "도넛 및 목록" 개체를 만듭니다.

    2.  각 타일에서 정보를 사용하여 쿼리를 업데이트합니다.

        1.  각 타일에서 TimeStamp_t 간격을 원하는 대로 변경합니다.

        2.  각 동기화 그룹에 대 한 타일에서 동기화 그룹 이름을 업데이트 합니다.

    3.  각 타일에서 필요에 따라 제목을 업데이트합니다.

4.  **저장**을 클릭하면 보기가 준비됩니다.

## <a name="cost-of-this-solution"></a>이 솔루션의 비용

대부분의 경우 이 솔루션은 무료입니다.

**Azure Automation:** 사용량에 따라 Azure Automation 계정에 발생하는 비용이 있을 수 있습니다. 매월 작업 실행 시간의 첫 번째 500분에 대해서는 무료입니다. 대부분의 경우 이 솔루션은 한 달에 500분 미만으로 사용해야 합니다. 요금 부과를 방지하려면 2시간 이상의 간격으로 Runbook을 실행하도록 예약합니다. 자세한 내용은 [Automation 가격](https://azure.microsoft.com/pricing/details/automation/)을 참조하세요.

**Azure Monitor 로그:** 사용량에 따라 Azure Monitor 로그와 관련 된 비용이 있을 수 있습니다. 체험 계층에서는 매일 500MB의 데이터를 수집할 수 있습니다. 대부분의 경우 이 솔루션은 하루에 500MB 미만으로 데이터를 수집해야 합니다. 사용량을 줄이려면 Runbook에 포함된 실패 전용 필터링을 사용합니다. 하루에 500 MB를 초과 하 여 사용 하는 경우 제한에 도달 하면 분석을 중지 하는 위험을 방지 하기 위해 유료 계층으로 업그레이드 합니다. 자세한 내용은 [Azure Monitor 로그 가격 책정](https://azure.microsoft.com/pricing/details/log-analytics/)을 참조 하세요.

## <a name="code-samples"></a>코드 샘플

이 문서에서 설명하는 코드 샘플은 다음 위치에서 다운로드합니다.

-   [데이터 동기화 로그 PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [데이터 동기화 Azure Monitor 보기](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>다음 단계
SQL 데이터 동기화에 대한 자세한 내용은 다음을 참조하세요.

-   개요 - [Azure의 SQL 데이터 동기화를 사용하여 여러 클라우드 및 온-프레미스 데이터베이스에서 데이터 동기화](sql-data-sync-data-sql-server-sql-database.md)
-   데이터 동기화 설정
    - 포털에서- [자습서: Azure SQL Database와 SQL Server 간에 데이터를 동기화 SQL 데이터 동기화 설정](sql-data-sync-sql-server-configure.md)
    - PowerShell 사용
        -  [PowerShell을 사용 하 여 Azure SQL Database에서 여러 데이터베이스 간 동기화](scripts/sql-data-sync-sync-data-between-sql-databases.md)
        -  [PowerShell을 사용 하 여 Azure SQL Database 데이터베이스와 SQL Server 인스턴스의 데이터베이스 간 동기화](scripts/sql-data-sync-sync-data-between-azure-onprem.md)
-   데이터 동기화 에이전트 - [Azure의 SQL 데이터 동기화용 데이터 동기화 에이전트](sql-data-sync-agent-overview.md)
-   모범 사례 - [Azure의 SQL 데이터 동기화에 대한 모범 사례](sql-data-sync-best-practices.md)
-   문제 해결 - [Azure의 SQL 데이터 동기화 문제 해결](sql-data-sync-troubleshoot.md)
-   동기화 스키마 업데이트
    -   Transact-sql을 사용 하 여 [Azure의 SQL 데이터 동기화에서 스키마 변경 내용 복제를 자동화 합니다](sql-data-sync-update-sync-schema.md) .
    -   PowerShell 사용 - [PowerShell을 사용하여 기존 동기화 그룹의 동기화 스키마 업데이트](scripts/update-sync-schema-in-sync-group.md)

SQL Database에 대한 자세한 내용은 다음을 참조하세요.

-   [SQL Database 개요](../../sql-database/sql-database-technical-overview.md)
-   [데이터베이스 수명 주기 관리](https://msdn.microsoft.com/library/jj907294.aspx)
