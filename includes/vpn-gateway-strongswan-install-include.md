---
title: 파일 포함
description: 포함 파일
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ddab6927044ce638e50ff3ad79aa3c35d046c820
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "69520861"
---
다음 구성은 아래 단계에 사용 되었습니다.

  | | |
  |---|---|
  |Computer| Ubuntu Server 18.04|
  |종속성| strongSwan |


다음 명령을 사용하여 필요한 strongSwan 구성을 설치합니다.

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

다음 명령을 사용 하 여 Azure 명령줄 인터페이스를 설치 합니다.

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[Azure CLI를 설치 하는 방법에 대 한 추가 지침](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
