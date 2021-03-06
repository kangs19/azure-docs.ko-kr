---
title: API 버전
titleSuffix: Azure Cognitive Search
description: .NET SDK의 Azure Cognitive Search REST Api 및 클라이언트 라이브러리에 대 한 버전 정책입니다.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: a7179f88f507f0deedc79e7ae49988c8b5a32f86
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85830097"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Cognitive Search의 API 버전

Azure Cognitive Search는 정기적으로 기능 업데이트를 롤업 합니다. 항상 그렇지는 않지만 경우에 따라 이러한 업데이트에는 이전 버전과 호환성을 유지하기 위해 API의 새 버전이 필요하기도 합니다. 새 버전을 게시하면 코드에서 검색 서비스 업데이트를 통합 하는 시기와 방법을 제어할 수 있습니다.

규칙에 따라 Azure Cognitive Search 팀은 새 API 버전을 사용 하도록 코드를 업그레이드 하는 데 몇 가지 노력이 수반 될 수 있으므로 필요한 경우에만 새 버전을 게시 합니다. API의 일부를 변경하여 더 이상 이전 버전과 호환되지 않는 경우에만 새 버전이 필요합니다. 그러한 변경은 기존 기능의 수정 또는 기존 API 노출 영역을 변경하는 새로운 기능으로 인해 발생할 수 있습니다.

SDK 업데이트와 동일한 규칙이 적용됩니다. Azure Cognitive Search SDK는 해당 버전의 세 부분 (주, 부 및 빌드 번호 (예: 1.1.0))을 포함 하는 [의미 체계 버전 관리](https://semver.org/) 규칙을 따릅니다. 이전 버전과 호환되지 않는 변경의 경우에만 SDK의 새로운 주 버전이 릴리스됩니다. 호환성을 유지하는 기능 업데이트의 경우 부 버전이, 버그 수정의 경우 빌드 버전이 증가합니다.

> [!NOTE]
> Azure Cognitive Search 서비스 인스턴스는 최신 버전을 포함 하 여 여러 REST API 버전을 지원 합니다. 더 이상 최신 버전이 아닌 버전을 계속 사용할 수는 있지만 코드를 마이그레이션하여 최신 버전을 사용하는 것이 좋습니다. REST API를 사용하는 경우 api-version 매개 변수를 통해 모든 요청에 API 버전을 지정해야 합니다. .NET SDK를 사용하는 경우 사용 중인 SDK의 버전에 따라 해당하는 REST API 버전이 결정됩니다. 이전 버전의 SDK를 사용하는 경우 최신 API 버전을 지원하기 위해 서비스가 업그레이드된 경우에도 변경 내용 없이 해당 코드를 계속 실행할 수 있습니다.

## <a name="rest-apis"></a>REST API

이 표에서는 Search Service REST API의 현재 버전과 이전에 릴리스된 버전의 버전 기록을 제공 합니다. 현재 안정적이 고 미리 보기 버전에 대 한 설명서가 게시 됩니다.

| 버전&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | 상태 | 이전 버전과의 호환성 문제 |
|-------------|--------|------------------------------|
| [관리 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) | 일반 공급 | Endpoint protection의 고급 기능을 통해 안정적인 최신 릴리스의 관리 REST Api 새 서비스에 대 한 개인 끝점, 개인 링크 지원 및 네트워크 규칙을 추가 합니다. |
| [관리 2019-10-01-미리 보기](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) | 미리 보기  | 버전 번호에도 불구 하 고 여전히 관리 REST Api의 현재 미리 보기 버전입니다. 지금은 미리 보기 기능이 없습니다. 모든 미리 보기 기능은 최근에 일반 공급으로 전환 되었습니다. |
| 관리 2015-08-19  | Stable| 관리 REST Api의 첫 번째 일반적으로 사용할 수 있는 버전입니다. 서비스 프로 비전, 확장 및 api 키 관리를 제공 합니다. |
| 관리 2015-08-19-미리 보기 | 미리 보기| 관리 REST Api의 첫 번째 미리 보기 버전입니다. |
| [검색 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/index)| Stable | 검색 검색 REST Api의 최신 안정적인 릴리스 (관련성 평가의 발전) |
| [검색 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview)| 미리 보기 | 안정적인 버전과 연결 된 미리 보기 버전입니다. |
| 검색 2019-05-06 | Stable | 복합 형식을 추가 합니다. |
| 검색 2019-05-06-미리 보기 | 미리 보기 | 안정적인 버전과 연결 된 미리 보기 버전입니다. |
| 검색 2017-11-11 | Stable  | 기술력과 및 AI 보강를 추가 합니다. |
| 검색 2017-11-11-미리 보기 | 미리 보기 | 안정적인 버전과 연결 된 미리 보기 버전입니다. |
| 검색 2016-09-01 |Stable | 인덱서 추가|
| 검색 2016-09-01-미리 보기 | 미리 보기 | 안정적인 버전과 연결 된 미리 보기 버전입니다.|
| 검색 2015-02-28 | Stable  | 첫 번째 일반 공급 릴리스.  |
| 검색 2015-02-28-미리 보기 | 미리 보기 | 안정적인 버전과 연결 된 미리 보기 버전입니다. |
| 검색 2014-10-20-미리 보기 | 미리 보기 | 두 번째 공개 미리 보기입니다. |
| 검색 2014-07-31-미리 보기 | 미리 보기 | 첫 번째 공개 미리 보기입니다. |

## <a name="azure-sdk-for-net"></a>Azure SDK for .NET

NuGet.org에서 패키지 버전 기록을 사용할 수 있습니다. 이 표에서는 각 패키지 페이지에 대 한 링크를 제공 합니다.

| SDK 버전 | 상태 | 설명 |
|-------------|--------|------------------------------|
| [**Azure.Search.Documents 1.0.0-preview. 4**](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | 미리 보기 | Azure .NET SDK의 새 클라이언트 라이브러리로, 2020 년 5 월에 출시 되었습니다. REST 2020-06-30 API 버전을 대상으로 합니다.|
| [**10.0 검색**](https://www.nuget.org/packages/Microsoft.Azure.Search/) | 출시 2019 년 5 월 출시 될 수 있습니다. 는 REST 2019-05-06 API 버전을 대상으로 합니다.|
| [**8.0-미리 보기**](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | Preview, 4 월 2019 릴리스. 는 REST 2019-05-06-Preview API 버전을 대상으로 합니다.|
| [**3.0.0를 검색 합니다.**](https://docs.microsoft.com/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Stable | 관리 REST api-version = 2015-08-19를 대상으로 합니다. |

## <a name="azure-sdk-for-java"></a>Java용 Azure SDK

| SDK 버전 | 상태 | 설명  |
|-------------|--------|------------------------------|
| [**Java SearchManagementClient 1.35.0**](https://docs.microsoft.com/java/api/overview/azure/search/management?view=azure-java-stable) | Stable | 관리 REST api-version = 2015-08-19를 대상으로 합니다.|

## <a name="azure-sdk-for-python"></a>Python용 Azure SDK

| SDK 버전 | 상태 | 설명  |
|-------------|--------|------------------------------|
| [**Python azure-관리-검색 1.0**](https://docs.microsoft.com/python/api/overview/azure/search?view=azure-python) | Stable | 관리 REST api-version = 2015-08-19를 대상으로 합니다. |