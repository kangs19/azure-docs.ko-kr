---
title: 개념-Azure 스프링 클라우드 서비스에 대 한 보안 제어
description: Azure 스프링 클라우드 서비스에 기본 제공 되는 보안 제어를 사용 합니다.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/23/2020
ms.openlocfilehash: 8d002fae52fec1fafb2ad8e63bd8e3b779a1537c
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85984826"
---
# <a name="security-controls-for-azure-spring-cloud-service"></a>Azure Spring Cloud Service의 보안 컨트롤
보안 제어는 Azure 스프링 클라우드 서비스에 기본 제공 됩니다.

보안 제어는 보안 취약성을 방지, 탐지 및 대응할 수 있는 서비스의 기능에 기여하는 Azure 서비스의 품질 또는 기능입니다.  각 컨트롤에 대해 *예* 또는 *아니요* 를 사용 하 여 서비스에 대 한 현재 준비 여부를 나타냅니다.  서비스에 적용 되지 않는 컨트롤에는 *N/A* 를 사용 합니다. 

**데이터 보호 보안 제어**

| 보안 컨트롤 | 예/아니요 | 메모 | 문서화 |
|:-------------|:-------|:-------------------------------|:----------------------|
| 미사용 서버 쪽 암호화: Microsoft 관리형 키 | 예 | 사용자가 업로드 한 원본 및 아티팩트, 구성 서버 설정, 앱 설정 및 영구 저장소의 데이터는 Azure Storage에 저장 되며, 미사용 콘텐츠를 자동으로 암호화 합니다.<br><br>구성 서버 캐시, 업로드 된 원본에서 빌드된 런타임 이진 파일 및 응용 프로그램 수명 중에 응용 프로그램 로그는 미사용 콘텐츠를 자동으로 암호화 하는 Azure 관리 디스크에 저장 됩니다.<br><br>사용자가 업로드 한 원본에서 빌드된 컨테이너 이미지는 미사용 이미지 콘텐츠를 자동으로 암호화 하는 Azure Container Registry에 저장 됩니다. | [미사용 데이터에 대한 Azure Storage 암호화](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)<br><br>[Azure Managed Disks의 서버 쪽 암호화](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption)<br><br>[Azure Container Registry의 컨테이너 이미지 스토리지](https://docs.microsoft.com/azure/container-registry/container-registry-storage) |
| 일시적인 암호화 | 예 | 사용자 앱 공용 끝점은 기본적으로 인바운드 트래픽에 대해 HTTPS를 사용 합니다. |  |
| API 호출 암호화 | 예 | Azure 스프링 클라우드 서비스를 구성 하기 위한 관리 호출은 HTTPS를 통한 Azure Resource Manager 호출을 통해 수행 됩니다. | [Azure 리소스 관리자](https://docs.microsoft.com/azure/azure-resource-manager/) |

**네트워크 액세스 보안 제어**

| 보안 컨트롤 | 예/아니요 | 메모 | 문서화 |
|:-------------|:-------|:-------------------------------|:----------------------|
| 서비스 태그 | 예 | **AzureSpringCloud** service 태그를 사용 하 여 Azure 스프링 클라우드 응용 프로그램에 대 한 트래픽을 허용 하는 [네트워크 보안 그룹](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) 또는 [azure 방화벽](https://docs.microsoft.com/azure/firewall/service-tags)에서 아웃 바운드 네트워크 액세스 제어를 정의 합니다.<br><br>*참고:* 현재 2020/07/07 이후 생성 된 새 Azure 스프링 클라우드 서비스 인스턴스만 **AzureSpringCloud** service 태그를 지원 합니다. | [서비스 태그](https://docs.microsoft.com/azure/virtual-network/service-tags-overview) |
