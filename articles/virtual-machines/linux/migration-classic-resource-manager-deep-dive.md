---
title: 클래식에서 Azure Resource Manager 마이그레이션 기술 심층 살펴보기
description: 클래식 배포 모델에서 Azure Resource Manager에 대 한 리소스의 플랫폼 지원 마이그레이션에 대 한 기술 심층 소개
author: tanmaygore
manager: vashan
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: a5277e23d92dd026aa19e278532869747709e646
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78944725"
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>클래식에서 Azure Resource Manager로의 플랫폼 지원 마이그레이션에 대한 기술 정보

> [!IMPORTANT]
> 현재 IaaS Vm의 90%가 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)를 사용 하 고 있습니다. 2020 년 2 월 28 일부 터 클래식 Vm은 더 이상 사용 되지 않으며 2023 년 3 월 1 일에 완전히 사용 중지 됩니다. 이 사용 중단 및 [영향](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)에 대 한 [자세한 내용을 알아보세요]( https://aka.ms/classicvmretirement) .

Azure 클래식 배포 모델에서 Azure Resource Manager 배포 모델로 마이그레이션하는 방법을 자세하게 살펴보겠습니다. Azure 플랫폼에서 두 가지 배포 모델 간에 리소스를 마이그레이션하는 방법을 더욱 잘 이해할 수 있도록 리소스 및 기능 수준에서 리소스를 검토합니다. 자세한 내용은 서비스 발표 문서 [클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)을 읽어보세요.

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a>다음 단계

* [클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션 개요](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [클래식에서 Azure Resource Manager로 IaaS 리소스의 마이그레이션 계획](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell을 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI를 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [클래식에서 Azure Resource Manager로의 IaaS 리소스 마이그레이션을 지원하기 위한 커뮤니티 도구](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [가장 일반적인 마이그레이션 오류 검토](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [클래식에서 Azure Resource Manager로의 IaaS 리소스 마이그레이션과 관련된 가장 자주 묻는 질문 검토](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
