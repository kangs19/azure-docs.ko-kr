---
title: 방화벽 또는 프록시 서버에서 Azure Portal URL 수신 허용 목록
description: 이러한 Url을 프록시 서버 바이패스에 추가 하 여 Azure Portal 및 해당 서비스와 통신
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 04/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 67ccf6e2f113c0ec72cfcbccedf88017e452ec4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85361662"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>방화벽 또는 프록시 서버에서 Azure Portal URL 수신 허용 목록

온-프레미스 보안 장치를 구성 하 여 Azure Portal Url에 대 한 보안 제한을 우회할 수 있습니다. 이 구성은 로컬 또는 광역 네트워크와 Azure 클라우드 간의 성능 및 연결을 향상 시킬 수 있습니다.

네트워크 관리자는 일반적으로 프록시 서버, 방화벽 또는 기타 장치를 배포 합니다. 이러한 장치를 사용 하면 사용자가 인터넷에 액세스 하는 방법을 안전 하 게 제어할 수 있습니다. 사용자를 보호 하도록 설계 된 규칙은 경우에 따라 합법적인 비즈니스 관련 인터넷 트래픽을 차단 하거나 늦출 수 있습니다. 이 트래픽은 사용자와 Azure 간의 통신을 포함 합니다. 네트워크와 Azure Portal 및 해당 서비스 간의 연결을 최적화 하려면 safelist에 Azure Portal Url을 추가 하는 것이 좋습니다.

## <a name="azure-portal-urls-for-proxy-bypass"></a>프록시 바이패스에 대 한 Azure Portal Url

Azure Portal에 대해 safelist URL 끝점은 조직이 배포 되는 Azure 클라우드와 관련이 있습니다. 이러한 끝점에 대 한 네트워크 트래픽이 제한을 무시 하도록 허용 하려면 클라우드를 선택 합니다. 그런 다음 Url 목록을 프록시 서버 또는 방화벽에 추가 합니다.

#### <a name="public-cloud"></a>[공용 클라우드](#tab/public-cloud)

```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloud"></a>[미국 정부 클라우드](#tab/us-government-cloud)

```
*.applicationinsights.us
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloud"></a>[중국 정부 클라우드](#tab/china-government-cloud)

```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> 이러한 끝점에 대 한 트래픽은 HTTP (80) 및 HTTPS (443)에 표준 TCP 포트를 사용 합니다.
>
>
