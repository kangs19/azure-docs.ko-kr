---
title: 재해 복구 및 저장소 계정 장애 조치(failover)
titleSuffix: Azure Storage
description: Azure Storage 지역 중복 저장소 계정에 대 한 계정 장애 조치 (failover)를 지원 합니다. 계정 장애 조치(failover)의 경우 기본 엔드포인트를 사용할 수 없는 경우 스토리지 계정에 대해 장애 조치(failover) 프로세스를 시작할 수 있습니다.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 4b1abe8efb4baaf260005df1a4ee5b6d1645715a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84169222"
---
# <a name="disaster-recovery-and-storage-account-failover"></a>재해 복구 및 저장소 계정 장애 조치(failover)

Microsoft는 Azure 서비스를 항상 사용할 수 있도록 하기 위해 노력합니다. 그러나 계획되지 않은 서비스 중단이 발생할 수 있습니다. 응용 프로그램에 복원 력이 필요한 경우 Microsoft는 지역 중복 저장소를 사용 하 여 두 번째 지역에 데이터를 복사 하도록 권장 합니다. 또한 고객은 지역 서비스 중단을 처리하기 위해 재해 복구 계획을 설정하는 것이 좋습니다. 재해 복구 계획의 중요한 부분은 기본 엔드포인트를 사용할 수 없는 경우 보조 엔드포인트로의 장애 조치(failover)를 준비하는 것입니다.

Azure Storage 지역 중복 저장소 계정에 대 한 계정 장애 조치 (failover)를 지원 합니다. 계정 장애 조치(failover)의 경우 기본 엔드포인트를 사용할 수 없는 경우 스토리지 계정에 대해 장애 조치(failover) 프로세스를 시작할 수 있습니다. 장애 조치(failover)는 스토리지 계정의 기본 엔드포인트가 되도록 보조 엔드포인트를 업데이트합니다. 장애 조치(failover)가 완료되면 클라이언트는 새 기본 엔드포인트에 쓰기 시작할 수 있습니다.

계정 장애 조치 (failover)는 Azure Resource Manager 배포를 사용 하는 범용 v1, 범용 v2 및 Blob storage 계정 유형에 사용할 수 있습니다. 계정 장애 조치 (failover)는 모든 공용 지역에 대해 지원 되지만 지금은 소 버린 또는 국가별 클라우드에서 사용할 수 없습니다.

이 문서에서는 계정 장애 조치(failover)와 관련된 개념 및 프로세스를 설명하고 고객에게 최소의 영향만 미치고 복구할 수 있게 스토리지 계정을 준비하는 방법을 논의합니다. Azure Portal 또는 PowerShell에서 계정 장애 조치 (failover)를 시작 하는 방법을 알아보려면 [계정 장애 조치 (failover) 시작](storage-initiate-account-failover.md)을 참조 하세요.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="choose-the-right-redundancy-option"></a>적절한 중복 옵션 선택

Azure Storage는 내구성 및 고가용성을 보장 하기 위해 저장소 계정의 여러 복사본을 유지 관리 합니다. 계정에 대해 선택하는 중복 옵션은 필요한 복원력 수준에 따라 달라집니다. 지역 가동 중단 으로부터 보호 하려면 보조 지역에서 읽기 권한 옵션을 사용 하거나 사용 하지 않고 지역 중복 저장소에 대 한 계정을 구성 합니다.  

**GRS (지역 중복 저장소) 또는 GZRS (지역 중복 저장소** )는 수백 마일 이상 떨어져 있는 두 지역에 데이터를 비동기적으로 복사 합니다. 주 지역에서 중단이 발생하는 경우 보조 지역이 데이터의 중복 원본으로 사용됩니다. 장애 조치(failover)를 시작하여 보조 엔드포인트를 기본 엔드포인트으로 변환할 수 있습니다.

**읽기 액세스 지역 중복 저장소 (ra-GRS) 또는 읽기 액세스 지역 중복 저장소 (ra-GZRS)** 는 보조 끝점에 대 한 읽기 액세스의 추가 혜택을 통해 지역 중복 저장소를 제공 합니다. 기본 끝점에서 중단이 발생 하는 경우 보조에 대 한 읽기 액세스를 위해 구성 된 응용 프로그램 및 고가용성을 위해 디자인 된 응용 프로그램은 계속 해 서 보조 끝점에서 읽을 수 있습니다. 응용 프로그램에 대 한 최대 가용성 및 내구성을 위해 RA GZRS을 권장 합니다.

Azure Storage의 중복성에 대 한 자세한 내용은 [중복성 Azure Storage](storage-redundancy.md)을 참조 하세요.

> [!WARNING]
> 지역 중복 스토리지를 사용하면 데이터가 손실될 위험이 있습니다. 데이터는 보조 지역에 비동기적으로 복사 되므로 주 지역에 쓰여진 데이터는 보조 지역에 기록 될 때 사이에 지연이 발생 합니다. 중단이 발생 하면 보조 끝점에 아직 복사 되지 않은 기본 끝점에 대 한 쓰기 작업이 손실 됩니다.

## <a name="design-for-high-availability"></a>고가용성을 위한 디자인

처음부터 고가용성을 위해 애플리케이션을 디자인하는 것이 중요합니다. 애플리케이션을 디자인하고 재해 복구를 계획하기 위한 지침에 대해서는 다음 Azure 리소스를 참조하세요.

- [Azure 용 복원 력 있는 응용 프로그램 디자인](/azure/architecture/framework/resiliency/app-design): azure에서 항상 사용 가능한 응용 프로그램을 설계 하는 주요 개념에 대 한 개요입니다.
- [복원 력 검사 목록](/azure/architecture/checklist/resiliency-per-service): 응용 프로그램에서 고가용성을 위한 최상의 디자인 방법을 구현 하는지 확인 하기 위한 검사 목록입니다.
- [지리적 중복을 사용 하 여 항상 사용 가능한 응용 프로그램 디자인](geo-redundant-design.md): 지역 중복 저장소를 활용 하는 응용 프로그램을 빌드하기 위한 디자인 지침입니다.
- [자습서: Blob storage를 사용 하 여 항상 사용 가능한 응용 프로그램 빌드](../blobs/storage-create-geo-redundant-storage.md): 오류 및 복구를 시뮬레이션 하는 것으로 끝점 간을 자동으로 전환 하는 고가용성 응용 프로그램을 빌드하는 방법을 보여 주는 자습서입니다. 

또한 Azure Storage 데이터에 대해 고가용성을 유지하기 위한 다음 모범 사례도 참조하세요.

- **디스크:** [Azure Backup](https://azure.microsoft.com/services/backup/) 를 사용 하 여 Azure 가상 머신에서 사용 하는 VM 디스크를 백업 합니다. 또한 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/)를 사용하여 지역 재해 발생 시 VM을 보호하는 것이 좋습니다.
- **블록 blob:** [일시 삭제](../blobs/storage-blob-soft-delete.md) 를 설정 하 여 개체 수준 삭제 및 덮어쓰기를 방지 하거나 [AzCopy](storage-use-azcopy.md), [Azure PowerShell](/powershell/module/az.storage/)또는 [Azure 데이터 이동 라이브러리](storage-use-data-movement-library.md)를 사용 하 여 다른 지역에 있는 다른 저장소 계정에 블록 blob을 복사 합니다.
- **파일:** [AzCopy](storage-use-azcopy.md) 또는 [Azure PowerShell](/powershell/module/az.storage/) 를 사용 하 여 다른 지역에 있는 다른 저장소 계정으로 파일을 복사 합니다.
- **테이블:**[AzCopy](storage-use-azcopy.md)를 사용하여 다른 지역에 있는 다른 스토리지 계정으로 테이블 데이터를 내보냅니다.

## <a name="track-outages"></a>중단 추적

고객은 [Azure Service Health 대시보드](https://azure.microsoft.com/status/)를 구독하여 Azure Storage 및 다른 Azure 서비스의 상태를 추적할 수 있습니다.

쓰기 오류의 가능성에 대비하도록 애플리케이션을 디자인하는 것이 좋습니다. 애플리케이션은 주 지역의 가동 중단 가능성을 경고하는 방식으로 쓰기 오류를 공개해야 합니다.

## <a name="understand-the-account-failover-process"></a>계정 장애 조치(failover) 프로세스 이해

고객이 관리 하는 계정 장애 조치 (failover)를 사용 하면 어떤 이유로 든 주 복제본을 사용할 수 없게 되 면 보조 지역으로 전체 저장소 계정을 장애 조치할 수 있습니다. 강제로 보조 지역으로 장애 조치(failover)하는 경우 클라이언트는 장애 조치(failover)가 완료된 후에 보조 엔드포인트에 데이터를 쓰기 시작할 수 있습니다. 장애 조치(failover)에는 일반적으로 약 1시간이 걸립니다.

### <a name="how-an-account-failover-works"></a>계정 장애 조치(failover) 작동 방법

정상적인 경우 클라이언트는 주 지역의 Azure Storage 계정에 데이터를 쓰고 해당 데이터를 보조 지역에 비동기적으로 복사 합니다. 다음 그림은 주 지역을 사용할 수 있는 경우의 시나리오를 보여 줍니다.

![클라이언트가 주 지역의 스토리지 계정에 데이터를 기록합니다.](media/storage-disaster-recovery-guidance/primary-available.png)

어떤 이유로든 기본 엔드포인트를 사용할 수 없는 경우 클라이언트는 스토리지 계정에 더 이상 쓸 수 없습니다. 다음 그림은 기본 엔드포인트를 사용할 수 없지만 복구가 아직 발생하지 않은 시나리오를 보여 줍니다.

![기본 엔드포인트를 사용할 수 없으므로 클라이언트가 데이터를 쓸 수 없습니다.](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

고객은 보조 엔드포인트에 대한 계정 장애 조치(failover)를 시작합니다. 장애 조치(failover) 프로세스는 Azure Storage에서 제공하는 DNS 항목을 업데이트하므로 다음 그림에 나와 있는 것처럼 보조 엔드포인트가 스토리지 계정의 새 기본 엔드포인트가 됩니다.

![고객은 보조 엔드포인트로의 계정 장애 조치(failover)를 시작합니다.](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

DNS 항목이 업데이트 되 고 요청이 새 기본 끝점으로 전달 되 면 지역 중복 계정에 대 한 쓰기 액세스가 복원 됩니다. Blob, 테이블, 큐 및 파일에 대한 기존 스토리지 서비스 엔드포인트는 장애 조치(failover) 후에도 동일하게 유지됩니다.

> [!IMPORTANT]
> 장애 조치(failover)를 완료한 후 스토리지 계정은 새 기본 엔드포인트에서 로컬로 중복되도록 구성됩니다. 새 보조 복제본에 대 한 복제를 다시 시작 하려면 지역 중복에 대 한 계정을 다시 구성 합니다.
>
> 지리적 중복성을 사용 하도록 LRS 계정을 변환 하면 비용이 발생 합니다. 이 비용은 장애 조치 (failover) 후 새 주 지역에서 저장소 계정을 업데이트 하는 데 적용 됩니다.  

### <a name="anticipate-data-loss"></a>데이터 손실 예상

> [!CAUTION]
> 계정 장애 조치(Failover)를 수행하면 일반적으로 데이터가 일부 손실됩니다. 계정 장애 조치(failover)를 시작할 때 진행되는 과정을 이해하는 것이 중요합니다.  

데이터는 주 지역에서 보조 지역으로 비동기적으로 기록 되기 때문에 주 지역에 대 한 쓰기는 보조 지역에 복사 되기 전에 항상 지연이 발생 합니다. 주 지역을 사용할 수 없게 되 면 최신 쓰기가 아직 보조 지역에 복사 되지 않았을 수 있습니다.

강제로 장애 조치(failover)를 수행하면 보조 지역이 새로운 주 지역이 되고 스토리지 계정이 로컬로 중복되도록 구성되므로 주 지역의 모든 데이터가 손실됩니다. 보조 복제본에 이미 복사 된 모든 데이터는 장애 조치 (failover)가 발생할 때 유지 관리 됩니다. 그러나 보조 데이터베이스로 복사 되지 않은 주 데이터베이스에 기록 된 데이터는 영구적으로 손실 됩니다.

**마지막 동기화 시간** 속성은 주 지역의 데이터가 보조 지역에 기록될 것으로 보장되는 가장 최근 시간을 나타냅니다. 마지막 동기화 시간 전에 기록된 모든 데이터는 보조 지역에서 사용할 수 있지만, 마지막 동기화 시간 이후에 기록된 데이터는 보조 지역에 기록되지 않았을 수 있으므로 손실될 수 있습니다. 작동 중단 시 이 속성을 사용하여 계정 장애 조치(failover) 시작으로 인해 발생할 수 있는 데이터 손실 크기를 예상합니다.

모범 사례로, 마지막 동기화 시간을 사용하여 예상되는 데이터 손실을 예측할 수 있도록 애플리케이션을 디자인합니다. 예를 들어, 모든 쓰기 작업을 기록하는 경우 마지막 쓰기 작업의 시간을 마지막 동기화 시간과 비교하여 보조 지역과 동기화되지 않은 쓰기를 확인할 수 있습니다.

**마지막 동기화 시간** 속성을 확인 하는 방법에 대 한 자세한 내용은 [저장소 계정에 대 한 마지막 동기화 시간 속성 확인](last-sync-time-get.md)을 참조 하세요.

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>원래 주 지역으로 장애 복구(Failback)할 때 주의

주 지역에서 보조 지역으로 장애 조치(failover)한 후에 스토리지 계정이 새 주 지역에서 로컬로 중복되도록 구성됩니다. 그런 다음 지역 중복에 대 한 계정을 다시 구성할 수 있습니다. 장애 조치 (failover) 후 지리적 중복성을 위해 계정을 다시 구성 하는 경우 새 주 지역에서 즉시 새 보조 지역으로 데이터를 복사 하기 시작 합니다 .이는 원래 장애 조치 (failover) 전의 기본 영역입니다. 그러나 주 데이터베이스의 기존 데이터가 새 보조 데이터베이스로 완전히 복사 되기까지 다소 시간이 걸릴 수 있습니다.

스토리지 계정이 지역 복제를 위해 다시 구성되면 새 주 지역에서 새 보조 지역으로의 다른 장애 조치(failover)를 시작할 수 있습니다. 이 경우 장애 조치(failover) 전의 원래 주 지역이 다시 주 지역이 되고 로컬로 중복되도록 구성됩니다. 장애 조치(failover) 후 주 지역(원래 보조 지역)의 모든 데이터는 손실됩니다. 장애 복구를 수행 하기 전에 저장소 계정에 있는 대부분의 데이터를 새 보조 데이터베이스로 복사 하지 않은 경우에는 중요 한 데이터가 손실 될 수 있습니다.

주요 데이터 손실을 방지하려면 장애 복구(failback) 전에 **마지막 동기화 시간** 속성 값을 확인합니다. 마지막 동기화 시간을 데이터를 새 주 지역에 마지막으로 기록한 시간과 비교하여 데이터 손실을 예측합니다. 

## <a name="initiate-an-account-failover"></a>계정 장애 조치(failover) 시작

Azure Portal, PowerShell, Azure CLI 또는 Azure Storage 리소스 공급자 API에서 계정 장애 조치(failover)를 시작할 수 있습니다. 장애 조치 (failover)를 시작 하는 방법에 대 한 자세한 내용은 [계정 장애 조치 시작](storage-initiate-account-failover.md)을 참조 하세요.

## <a name="additional-considerations"></a>추가 고려 사항

이 섹션에서 설명 하는 추가 고려 사항을 검토 하 여 장애 조치 (failover)를 강제할 때 응용 프로그램 및 서비스에 어떤 영향을 미치는지 이해 합니다.

### <a name="storage-account-containing-archived-blobs"></a>보관 된 blob을 포함 하는 저장소 계정

보관 된 blob이 있는 저장소 계정은 계정 장애 조치 (failover)를 지원 합니다. 장애 조치 (failover)가 완료 되 면 보관 된 모든 blob을 온라인 계층으로 전환 해야 지역 중복에 대해 계정을 구성할 수 있습니다.

### <a name="storage-resource-provider"></a>스토리지 리소스 공급자

장애 조치 (failover)가 완료 된 후 클라이언트는 새 주 지역에서 Azure Storage 데이터를 다시 읽고 쓸 수 있습니다. 그러나 Azure Storage 리소스 공급자는 장애 조치 (failover) 되지 않으므로 리소스 관리 작업이 주 지역에서 계속 발생 해야 합니다. 주 지역을 사용할 수 없는 경우에는 저장소 계정에 대 한 관리 작업을 수행할 수 없습니다.

Azure Storage 리소스 공급자는 장애 조치 (failover) 되지 않으므로 [위치](/dotnet/api/microsoft.azure.management.storage.models.trackedresource.location) 속성은 장애 조치 (failover)가 완료 된 후 원래 기본 위치를 반환 합니다.

### <a name="azure-virtual-machines"></a>Azure 가상 머신

Azure VM(Virtual Machines)은 계정 장애 조치(failover)의 일부로 장애 조치(failover)되지 않습니다. 주 지역을 사용할 수 없으며 보조 지역으로 장애 조치(failover)할 경우 장애 조치(failover) 후에 VM을 다시 만들어야 합니다. 또한 계정 장애 조치 (failover)와 관련 된 잠재적인 데이터 손실이 있습니다. Azure의 가상 머신과 관련 하 여 다음과 같은 고가용성 및 [재해 복구](../../virtual-machines/virtual-machines-disaster-recovery-guidance.md) 지침을 [사용](../../virtual-machines/windows/manage-availability.md) 하는 것이 좋습니다.

### <a name="azure-unmanaged-disks"></a>Azure 관리되지 않는 디스크

모범 사례로, 관리되지 않는 디스크를 Managed Disks로 변환하는 것이 좋습니다. 그러나 Azure VM에 연결된 관리되지 않는 디스크를 포함하는 계정을 장애 조치(failover)해야 할 경우 장애 조치(failover)를 시작하기 전에 VM을 종료해야 합니다.

관리되지 않는 디스크는 Azure Storage에 페이지 Blob으로 저장됩니다. VM을 Azure에서 실행 중인 경우 VM에 연결된 모든 관리되지 않는 디스크가 임대됩니다. Blob에 임대가 있으면 계정 장애 조치(failover)를 계속할 수 없습니다. 장애 조치(failover)를 수행하려면 다음 단계를 수행합니다.

1. 시작하기 전에 관리되지 않는 디스크, 해당 LUN(논리 단위 번호) 및 해당 디스크가 연결된 VM을 기록해 둡니다. 이렇게 하면 장애 조치(failover) 후에 디스크를 보다 쉽게 다시 연결할 수 있습니다.
2. VM을 종료합니다.
3. VM을 삭제하지만 관리되지 않는 디스크의 VHD 파일은 유지합니다. VM을 삭제한 시간을 기록해 둡니다.
4. VM을 삭제한 이후에 해당하는 **마지막 동기화 시간**이 업데이트될 때까지 기다립니다. 장애 조치(failover)가 발생할 때 보조 엔드포인트가 VHD 파일로 완전히 업데이트되지 않은 경우 새 주 지역에서 VM이 제대로 작동하지 않을 수 있으므로 이 단계가 필요합니다.
5. 계정 장애 조치(failover)를 시작합니다.
6. 계정 장애 조치(failover)가 완료되고 보조 지역이 새 주 지역이 될 때까지 기다립니다.
7. 새 주 지역에서 VM을 만들고 VHD를 다시 연결합니다.
8. 새 VM을 시작합니다.

VM이 종료되면 임시 디스크에 저장된 데이터가 손실됩니다.

## <a name="unsupported-features-and-services"></a>지원 되지 않는 기능 및 서비스

다음 기능 및 서비스는 계정 장애 조치 (failover)에 대해 지원 되지 않습니다.

- Azure 파일 동기화는 스토리지 계정 장애 조치(Failover)를 지원하지 않습니다. Azure 파일 동기화에서 클라우드 엔드포인트로 사용되는 Azure 파일 공유를 포함하는 스토리지 계정은 장애 조치(failover)하지 않아야 합니다. 이러한 계정을 장애 조치(failover)하면 동기화가 더 이상 진행되지 않고, 새로 계층화된 파일의 경우 예기치 않은 데이터 손실이 발생할 수도 있습니다.
- ADLS Gen2 storage 계정 (계층적 네임 스페이스를 사용 하는 계정은 현재 지원 되지 않음)입니다.
- 프리미엄 블록 blob을 포함하는 스토리지 계정은 장애 조치(failover)할 수 없습니다. 프리미엄 블록 blob를 지원하는 스토리지 계정은 현재 지리적 중복을 지원하지 않습니다.
- [웜 불변성 정책](../blobs/storage-blob-immutable-storage.md) 사용 컨테이너를 포함 하는 저장소 계정은 장애 조치 (failover) 할 수 없습니다. 잠금 해제/잠금 시간 기반 보존 또는 법적 보류 정책은 규정 준수를 유지 하기 위해 장애 조치 (failover)를 방지 합니다.

## <a name="copying-data-as-an-alternative-to-failover"></a>장애 조치(failover) 대신 데이터 복사

저장소 계정이 보조에 대 한 읽기 액세스로 구성 된 경우 보조 끝점에서 읽도록 응용 프로그램을 디자인할 수 있습니다. 주 지역에서 중단이 발생할 경우 장애 조치(failover)하지 않으려는 경우 [AzCopy](storage-use-azcopy.md), [Azure PowerShell](/powershell/module/az.storage/) 또는 [Azure 데이터 이동 라이브러리](../common/storage-use-data-movement-library.md)와 같은 도구를 사용하여 보조 지역의 스토리지 계정에 있는 데이터를 영향을 받지 않는 지역의 다른 스토리지 계정으로 복사할 수 있습니다. 그런 다음, 읽기 및 쓰기 가용성을 위해 애플리케이션에서 해당 스토리지 계정을 가리키도록 할 수 있습니다.

> [!CAUTION]
> 계정 장애 조치 (failover)는 데이터 마이그레이션 전략의 일부로 사용해 서는 안 됩니다.

## <a name="microsoft-managed-failover"></a>Microsoft에서 관리하는 장애 조치(failover)

중대한 재해로 인해 지역이 손실되는 극단적인 경우 Microsoft는 지역 장애 조치(failover)를 시작할 수 있습니다. 이 경우에 사용자의 조치가 필요하지 않습니다. Microsoft에서 관리하는 장애 조치(failover)가 완료될 때까지 스토리지 계정에 대한 쓰기 액세스 권한이 없습니다. 저장소 계정이 RA-GRS 또는 RA-GZRS에 대해 구성 된 경우 응용 프로그램은 보조 지역에서 읽을 수 있습니다.

## <a name="see-also"></a>참조

- [지리적 중복성을 사용하여 고가용성 애플리케이션 설계](geo-redundant-design.md)
- [계정 장애 조치(failover) 시작](storage-initiate-account-failover.md)
- [저장소 계정에 대한 마지막 동기화 시간 속성 확인](last-sync-time-get.md)
- [자습서: Blob Storage에서 고가용성 애플리케이션 빌드](../blobs/storage-create-geo-redundant-storage.md)
