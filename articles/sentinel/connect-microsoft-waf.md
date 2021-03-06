---
title: Azure 센티널에 웹 응용 프로그램 방화벽 데이터 연결
description: Microsoft 웹 응용 프로그램 방화벽 데이터를 Azure 센티널에 연결 하는 방법을 알아봅니다.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77588181"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Microsoft 웹 응용 프로그램 방화벽에서 데이터 연결



Azure 애플리케이션 게이트웨이의 Microsoft 웹 응용 프로그램 방화벽 (WAF)에서 로그를 스트리밍할 수 있습니다. 이 WAF는 SQL 삽입 및 교차 사이트 스크립팅 같은 일반적인 웹 취약점 으로부터 응용 프로그램을 보호 하 고 가양성을 줄이기 위해 규칙을 사용자 지정할 수 있도록 합니다. Microsoft 웹 응용 프로그램 방화벽 로그를 Azure 센티널로 스트리밍하려면 다음 지침을 따르세요.


## <a name="prerequisites"></a>사전 요구 사항

- 기존 application gateway 리소스

## <a name="connect-to-microsoft-web-application-firewall"></a>Microsoft 웹 응용 프로그램 방화벽에 연결

Microsoft 웹 응용 프로그램 방화벽이 이미 있는 경우 기존 게이트웨이 리소스가 있는지 확인 합니다.
Microsoft 웹 응용 프로그램 방화벽을 배포 하 고 데이터를 가져오면 경고 데이터를 쉽게 Azure 센티널로 스트리밍할 수 있습니다.
    
1. Azure 센티널 포털에서 **데이터 커넥터**를 선택 합니다.
1. 데이터 커넥터 페이지에서 **Waf** 타일을 선택 합니다.
1. [Application Gateway 리소스로](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways)이동한   후 waf를 선택 합니다.
    1. **진단 설정**을 선택합니다.
    1. 테이블에서 **+ 진단 설정 추가** 를 선택 합니다.
    1. **진단 설정** 페이지에서 **이름을** 입력 하 고 **Log Analytics 보내기를**선택 합니다.
    1. **Log Analytics 작업 영역** 에서 Azure 센티널 작업 영역을 선택 합니다.
    1. 분석 하려는 로그 유형을 선택 합니다. ApplicationGatewayAccessLog 및 ApplicationGatewayFirewallLog를 권장 합니다.
1. Microsoft 웹 응용 프로그램 방화벽 경고에 대 한 Log Analytics에서 관련 스키마를 사용 하려면 **Azurediagnostics**를 검색 합니다.

## <a name="next-steps"></a>다음 단계
이 문서에서는 Microsoft 웹 응용 프로그램 방화벽을 Azure 센티널에 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- [데이터에 대한 가시성을 얻고 재적 위협을 확인](quickstart-get-visibility.md)하는 방법을 알아봅니다.
- [Azure Sentinel을 사용하여 위협 검색](tutorial-detect-threats-built-in.md)을 시작합니다.
