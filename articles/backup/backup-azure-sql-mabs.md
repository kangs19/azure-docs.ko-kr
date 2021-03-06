---
title: Azure Backup Server를 사용 하 여 SQL Server 백업
description: 이 문서에서는 MABS (Microsoft Azure Backup 서버)를 사용 하 여 SQL Server 데이터베이스를 백업 하는 구성에 대해 알아봅니다.
ms.topic: conceptual
ms.date: 03/24/2017
ms.openlocfilehash: 2bb172ca36f3f932fdaaf5b71e8fa183c04d1510
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84194194"
---
# <a name="back-up-sql-server-to-azure-by-using-azure-backup-server"></a>Azure Backup Server를 사용 하 여 Azure에 SQL Server 백업

이 문서는 MABS (Microsoft Azure Backup 서버)를 사용 하 여 SQL Server 데이터베이스의 백업을 설정 하는 데 도움이 됩니다.

SQL Server 데이터베이스를 백업 하 고 Azure에서 복구 하려면 다음을 수행 합니다.

1. Azure에서 SQL Server 데이터베이스를 보호 하기 위한 백업 정책을 만듭니다.
1. Azure에서 주문형 백업 복사본을 만듭니다.
1. Azure에서 데이터베이스를 복구 합니다.

## <a name="before-you-start"></a>시작하기 전에

시작 하기 전에 [Azure Backup Server를 설치 하 고 준비](backup-azure-microsoft-azure-backup.md)했는지 확인 합니다.

## <a name="create-a-backup-policy"></a>백업 정책 만들기

Azure에서 SQL Server 데이터베이스를 보호 하려면 먼저 백업 정책을 만듭니다.

1. Azure Backup Server에서 **보호** 작업 영역을 선택 합니다.
1. 보호 그룹을 만들려면 **새로** 만들기를 선택 합니다.

    ![Azure Backup Server에서 보호 그룹 만들기](./media/backup-azure-backup-sql/protection-group.png)
1. 시작 페이지에서 보호 그룹 만들기에 대 한 지침을 검토 합니다. **다음**을 선택합니다.
1. 보호 그룹 종류에 대해 **서버**를 선택 합니다.

    ![서버 보호 그룹 유형 선택](./media/backup-azure-backup-sql/pg-servers.png)
1. 백업 하려는 데이터베이스가 있는 SQL Server 인스턴스를 확장 합니다. 해당 서버에서 백업할 수 있는 데이터 원본이 표시 됩니다. **모든 SQL 공유** 를 확장 하 고 백업 하려는 데이터베이스를 선택 합니다. 이 예에서는 ReportServer $ MSDPM2012 및 ReportServer $ MSDPM2012TempDB를 선택 합니다. **새로 만들기**를 선택합니다.

    ![SQL Server 데이터베이스 선택](./media/backup-azure-backup-sql/pg-databases.png)
1. 보호 그룹의 이름을 지정한 다음 **온라인 보호**를 선택 합니다.

    ![데이터 보호 방법 선택-단기 디스크 보호 또는 온라인 Azure 보호](./media/backup-azure-backup-sql/pg-name.png)
1. **단기 목표 지정** 페이지에서 디스크에 대 한 백업 위치를 만드는 데 필요한 입력을 포함 합니다.

    이 예에서는 **보존 범위** 를 *5 일로*설정 합니다. 백업 **동기화 빈도** 는 *15 분*마다 한 번으로 설정 됩니다. **빠른 전체 백업** 은 *오후 8:00*시로 설정 됩니다.

    ![백업 보호의 단기 목표 설정](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 이 예에서 백업 지점은 매일 8:00 PM에 생성 됩니다. 이전 날의 8:00 PM 백업 지점 이후 수정 된 데이터는 전송 됩니다. 이 프로세스를 **빠른 전체 Backup**이라고 합니다. 트랜잭션 로그는 15 분 마다 동기화 되지만 오후 9:00 시에 데이터베이스를 복구 해야 하는 경우 마지막 빠른 전체 백업 지점에서 로그를 재생 하 여 (이 예제에서는 8:00 PM) 지점을 만듭니다.
   >
   >

1. **새로 만들기**를 선택합니다. MABS는 사용 가능한 전체 저장소 공간을 보여 줍니다. 또한 잠재적인 디스크 공간 사용률을 보여 줍니다.

    ![MABS에서 디스크 할당 설정](./media/backup-azure-backup-sql/pg-storage.png)

    기본적으로 MABS는 데이터 원본 (SQL Server 데이터베이스) 당 하나의 볼륨을 만듭니다. 볼륨은 초기 백업 복사본에 사용 됩니다. 이 구성에서 LDM (논리 디스크 관리자)은 MABS 보호를 300 데이터 원본 (SQL Server 데이터베이스)으로 제한 합니다. 이 제한을 해결하려면 **DPM 스토리지 풀에 데이터 공동 배치**를 선택합니다. 이 옵션을 사용 하는 경우 MABS는 여러 데이터 원본에 단일 볼륨을 사용 합니다. 이렇게 설정 하면 MABS에서 최대 2000 SQL Server 데이터베이스를 보호할 수 있습니다.

    **볼륨 자동 증가**를 선택 하는 경우 프로덕션 데이터가 증가 함에 따라 mabs가 증가 된 백업 볼륨을 고려해 야 할 수 있습니다. **볼륨 자동 증가**를 선택 하지 않으면 mabs는 백업 저장소를 보호 그룹의 데이터 원본으로 제한 합니다.
1. 관리자는이 초기 백업을 **네트워크를 통해 자동으로** 전송 하 고 전송 시간을 선택 하도록 선택할 수 있습니다. 또는 백업을 **수동으로** 전송 하도록 선택 합니다. **다음**을 선택합니다.

    ![MABS에서 복제본 만들기 방법 선택](./media/backup-azure-backup-sql/pg-manual.png)

    초기 백업 복사본을 사용 하려면 전체 데이터 원본 (SQL Server 데이터베이스)을 전송 해야 합니다. 백업 데이터는 프로덕션 서버 (SQL Server 컴퓨터)에서 MABS로 이동 합니다. 이 백업이 크면 네트워크를 통해 데이터를 전송 하면 대역폭이 정체 될 수 있습니다. 이러한 이유로 관리자는 이동식 미디어를 사용 하 여 초기 백업을 **수동으로**전송 하도록 선택할 수 있습니다. 또는 지정 된 시간에 **네트워크를 통해 데이터를 자동으로** 전송할 수 있습니다.

    초기 백업이 완료 되 면 백업은 초기 백업 복사본에 대해 증분 방식으로 계속 됩니다. 증분 백업은 크기가 작으며 네트워크를 통해 간편하게 전송될 수 있습니다.
1. 일관성 확인을 실행할 시간을 선택 합니다. **다음**을 선택합니다.

    ![일관성 확인을 실행할 시기를 선택 하십시오.](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS는 백업 지점의 무결성에 대 한 일관성 확인을 실행할 수 있습니다. 프로덕션 서버 (이 예에서는 SQL Server 컴퓨터)에서 백업 파일의 체크섬과 MABS의 해당 파일에 대 한 백업 된 데이터를 계산 합니다. 검사가 충돌을 발견 하면 MABS의 백업 된 파일이 손상 된 것으로 간주 됩니다. MABS는 체크섬 불일치에 해당 하는 블록을 전송 하 여 백업 된 데이터를 수정 합니다. 일관성 확인은 성능 집약적인 작업 이므로 관리자는 일관성 확인을 예약 하거나 자동으로 실행 하도록 선택할 수 있습니다.
1. Azure에서 보호할 데이터 원본을 선택 합니다. **다음**을 선택합니다.

    ![Azure에서 보호할 데이터 원본 선택](./media/backup-azure-backup-sql/pg-sqldatabases.png)
1. 관리자 인 경우 조직의 정책에 맞는 백업 일정 및 보존 정책을 선택할 수 있습니다.

    ![일정 및 보존 정책 선택](./media/backup-azure-backup-sql/pg-schedule.png)

    이 예제에서 백업은 매일 오후 12:00 시와 오후 8:00에 수행 됩니다.

    > [!TIP]
    > 빠른 복구를 위해 디스크에 몇 가지 단기 복구 지점이 유지 됩니다. 이러한 복구 지점은 운영 복구에 사용됩니다. Azure는 좋은 오프 사이트 위치 역할을 하며 높은 Sla 및 보장 된 가용성을 제공 합니다.
    >
    > Data Protection Manager (DPM)을 사용 하 여 로컬 디스크 백업이 완료 된 후에 Azure 백업을 예약할 수 있습니다. 이 연습을 수행 하면 최신 디스크 백업이 Azure에 복사 됩니다.
    >

1. 보존 정책 일정을 선택합니다. 보존 정책의 작동 방법에 대 한 자세한 내용은 [Azure Backup를 사용 하 여 테이프 인프라 교체](backup-azure-backup-cloud-as-tape.md)를 참조 하세요.

    ![MABS에서 보존 정책 선택](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    이 예제에서:

    * 백업은 매일 오후 12:00 시와 오후 8:00에 수행 됩니다. 180 일 동안 유지 됩니다.
    * 토요일 12:00 PM의 백업은 104 주 동안 보관 됩니다.
    * 매월 마지막 토요일 12:00 PM의 백업은 60 개월 동안 유지 됩니다.
    * 3 월의 마지막 토요일 12:00 PM에서의 백업은 10 년 동안 유지 됩니다.

    보존 정책을 선택한 후 **다음**을 선택 합니다.
1. 초기 백업 복사본을 Azure에 전송 하는 방법을 선택 합니다.

    * **네트워크를 통해 자동으로** 옵션은 백업 일정에 따라 데이터를 Azure로 전송 합니다.
    * **오프 라인 백업**에 대 한 자세한 내용은 [오프 라인 백업 개요](offline-backup-overview.md)를 참조 하세요.

    전송 메커니즘을 선택한 후 **다음**을 선택 합니다.
1. **요약** 페이지에서 정책 세부 정보를 검토 합니다. 그런 다음 **그룹 만들기**를 선택 합니다. **닫기** 를 선택 하 고 **모니터링** 작업 영역에서 작업 진행률을 볼 수 있습니다.

    ![보호 그룹 만들기 진행률](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="create-on-demand-backup-copies-of-a-sql-server-database"></a>SQL Server 데이터베이스의 주문형 백업 복사본 만들기

첫 번째 백업이 발생 하면 복구 지점이 생성 됩니다. 예약 실행을 대기 하는 대신 복구 지점 만들기를 수동으로 트리거할 수 있습니다.

1. 보호 그룹에서 데이터베이스 상태가 **양호**인지 확인 합니다.

    ![데이터베이스 상태를 표시 하는 보호 그룹](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
1. 데이터베이스를 마우스 오른쪽 단추로 클릭 한 다음 **복구 지점 만들기**를 선택 합니다.

    ![온라인 복구 지점 만들기를 선택 합니다.](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
1. 드롭다운 메뉴에서 **온라인 보호**를 선택 합니다. 그런 다음 **확인** 을 선택 하 여 Azure에서 복구 지점 만들기를 시작 합니다.

    ![Azure에서 복구 지점 만들기 시작](./media/backup-azure-backup-sql/sqlbackup-azure.png)
1. **모니터링** 작업 영역에서 작업 진행률을 볼 수 있습니다.

    ![모니터링 콘솔에서 작업 진행률 보기](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure에서 SQL Server 데이터베이스 복구

Azure에서 SQL Server 데이터베이스와 같은 보호 된 엔터티를 복구 하려면 다음을 수행 합니다.

1. DPM 서버 관리 콘솔을 엽니다. **복구** 작업 영역으로 이동 하 여 DPM에서 백업 하는 서버를 확인 합니다. 데이터베이스 (이 예에서는 ReportServer $ MSDPM2012)를 선택 합니다. **온라인**으로 끝나는 **복구 시간** 을 선택 합니다.

    ![복구 지점 선택](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
1. 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **복구**를 선택 합니다.

    ![Azure에서 데이터베이스 복구](./media/backup-azure-backup-sql/sqlbackup-recover.png)
1. DPM에서는 복구 지점에 대한 세부 정보를 보여줍니다. **새로 만들기**를 선택합니다. 데이터베이스를 덮어쓰려면 복구 형식 **SQL Server의 원본 인스턴스에 복구**를 선택합니다. **다음**을 선택합니다.

    ![데이터베이스를 원래 위치로 복구](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    이 예에서는 DPM을 사용 하 여 다른 SQL Server 인스턴스 또는 독립 실행형 네트워크 폴더에 데이터베이스를 복구할 수 있습니다.
1. **복구 옵션 지정** 페이지에서 복구 옵션을 선택할 수 있습니다. 예를 들어, **네트워크 대역폭 사용 제한을** 선택 하 여 복구에 사용 되는 대역폭을 제한할 수 있습니다. **다음**을 선택합니다.
1. **요약** 페이지에 현재 복구 구성이 표시 됩니다. **복구**를 선택 합니다.

    복구 상태에는 복구 중인 데이터베이스가 표시 됩니다. **닫기** 를 선택 하 여 마법사를 닫고 **모니터링** 작업 영역에서 진행률을 확인할 수 있습니다.

    ![복구 프로세스를 시작 합니다.](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    복구가 완료 되 면 복원 된 데이터베이스는 응용 프로그램과 일치 합니다.

### <a name="next-steps"></a>다음 단계

자세한 내용은 [AZURE BACKUP FAQ](backup-azure-backup-faq.md)를 참조 하세요.
