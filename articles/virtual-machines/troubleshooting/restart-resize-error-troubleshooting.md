---
title: Azure의 VM 재시작 또는 크기 조정 문제 | Microsoft Docs
description: Azure의 기존 Virtual Machine을 재시작하거나 크기 조정하여 Resource Manager 배포 문제 해결
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a6532558107463311c4225b9855bc4cd3f19eed9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75965620"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Azure에서 기존 Windows VM 재시작 또는 크기 조정 관련 배포 문제 해결
중지된 Azure Virtual Machine(VM)을 시작하거나, 기존 Azure AM의 크기를 조정하려다 접하는 일반적인 오류는 할당 오류입니다. 이런 오류는 클러스터나 지역에 사용할 수 있는 리소스가 없거나 요청한 VM 크기를 지원할 수 없을 때 발생합니다.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>활동 로그 선택
문제 해결을 시작하려면 문제와 관련된 오류를 파악하기 위해 활동 로그를 수집합니다. 다음 링크에는 프로세스에 대한 자세한 내용이 포함되어 있습니다.

[배포 작업 보기](../../azure-resource-manager/templates/deployment-history.md)

[활동 로그를 보고 Azure 리소스 관리](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>문제: 중지된 VM 시작 시 오류
중지된 VM을 시작하려는데 할당 오류가 발생했습니다.

### <a name="cause"></a>원인
중지된 VM을 시작하기 위한 요청은 클라우드 서비스를 호스트하는 원본 클러스터에서 시도되어야 합니다. 하지만, 클러스터에 요청을 수행하는 데 사용할 여유 공간이 없습니다.

### <a name="resolution"></a>해결 방법
* 가용성 집합의 VM을 모두 중지하고 각각의 VM을 다시 시작합니다.
  
  1. 리소스 **그룹**리소스  >  *그룹 리소스를*클릭 하 여  >  **Resources**  >  *가용성 집합에서*  >  **Virtual Machines**  >  *가상 컴퓨터를*  >  **중지**Virtual Machines 합니다.
  2. VM을 모두 중지한 후에, 중지된 각각의 VM을 선택하고 시작을 클릭합니다.
* 나중에 다시 시작 요청을 다시 시도합니다.

## <a name="issue-error-when-resizing-an-existing-vm"></a>문제: 기존 VM 재시작 시 오류
기존 VM의 크기를 조정하려는데 할당 오류가 발생했습니다.

### <a name="cause"></a>원인
VM 크기를 조정하기 위한 요청은 클라우드 서비스를 호스트하는 원본 클러스터에서 시도되어야 합니다. 하지만, 클러스터가 요청한 VM 크기를 지원하지 않습니다.

### <a name="resolution"></a>해결 방법
* 더 작은 VM 크기를 사용하여 요청을 다시 시도합니다.
* 요청한 VM의 크기를 변경할 수 없으면:
  
  1. 가용성 집합의 VM을 모두 중지합니다.
     
     * 리소스 **그룹**리소스  >  *그룹 리소스를*클릭 하 여  >  **Resources**  >  *가용성 집합에서*  >  **Virtual Machines**  >  *가상 컴퓨터를*  >  **중지**Virtual Machines 합니다.
  2. VM을 모두 중지한 후에, 원하는 VM을 더 크게 조정합니다.
  3. 크기가 조정된 VM을 선택하고 **시작**을 클릭한 다음 중지된 각각의 VM을 시작합니다.

## <a name="next-steps"></a>다음 단계
Azure에서 새 Windows VM을 만들 때 문제가 발생하면 [Azure에서 새 Windows 가상 머신 생성 관련 배포 문제 해결](../windows/troubleshoot-deployment-new-vm.md)을 참조하세요.

