---
title: 파일 포함
description: 포함 파일
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 11469d992e0f5669cd3fc1e3864627dd0b8ae23d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81263349"
---
다음은 일반 공급의 키 제한을 요약 한 것입니다.

### <a name="sku-ingress-rates-and-capacities"></a>SKU 수신 속도 및 용량

S1 및 S2 SKU 수신 속도 및 용량은 새 Time Series Insights 환경을 구성할 때 유연성을 제공 합니다. SKU 용량은 이벤트의 수 또는 저장 된 바이트 수에 따라 매일의 수신 요금을 나타냅니다. 수신은 *분당*측정 되며, **제한은** 토큰 버킷 알고리즘을 사용 하 여 적용 됩니다. 수신은 1kb 블록 단위로 측정 됩니다. 예를 들어 0.8 실제 이벤트는 하나의 이벤트로 측정 되 고 2.6 KB 이벤트는 세 개의 이벤트로 계산 됩니다.

| S1 SKU 용량 | 수신 율 | 최대 저장소 용량
| --- | --- | --- |
| 1 | 일별 1gb (100만 이벤트) | 매달 30GB(3천만 이벤트) |
| 10 | 일별 10gb (1000만 이벤트) | 매달 300GB(3억 이벤트) |

| S2 SKU 용량 | 수신 율 | 최대 저장소 용량
| --- | --- | --- |
| 1 | 일별 10gb (1000만 이벤트) | 매달 300GB(3억 이벤트) |
| 10 | 하루 100 GB (1억 이벤트) | 매달 3TB(30억 이벤트) |

> [!NOTE]
> 용량은 연속해서 크기가 조정되므로 용량 2의 S1 SKU는 일일 2GB(2백만) 이벤트 수신 속도 및 매달 60GB(6천만 이벤트)를 지원합니다.

S2 SKU 환경은 매월 매우 많은 이벤트를 지원 하 고 수신 용량이 매우 높습니다.

| SKU  | 월별 이벤트 수  | 분당 이벤트 수 | 분당 이벤트 크기  |
|---------|---------|---------|---------|---------|
| S1     |   3천만   |  720    |  720KB   |
 |S2     |   3억   | 7,200   | 7,200KB  |

### <a name="property-limits"></a>속성 제한

GA 속성 제한은 선택 된 SKU 환경에 따라 달라 집니다. 제공 된 이벤트 속성에는 해당 JSON, CSV 및 [Time Series Insights 탐색기](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart)내에서 볼 수 있는 차트 열이 있습니다.

| SKU | 최대 속성 |
| --- | --- |
| S1 | 600 속성 (열) |
| S2 | 800 속성 (열) |

### <a name="event-sources"></a>이벤트 원본

인스턴스당 최대 2 개의 이벤트 원본이 지원 됩니다. 

* [이벤트 허브 원본을 추가](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)하는 방법에 대해 알아봅니다.
* [IoT hub 원본을](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub)구성 합니다.

### <a name="api-limits"></a>API 제한

Time Series Insights 일반 가용성에 대 한 REST API 제한은 [REST API 참조 설명서](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits)에 지정 되어 있습니다.