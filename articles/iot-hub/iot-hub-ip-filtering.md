---
title: Azure IoT Hub IP 연결 필터 | Microsoft Docs
description: 특정 IP 주소에서 Azure IoT hub로 연결을 차단하도록 IP 필터링을 사용하는 방법입니다. 개별 또는 IP 주소 범위에서 연결을 차단할 수 있습니다.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/25/2020
ms.author: robinsh
ms.openlocfilehash: 742706f4daa518faf06e5c8b735e679f345f1279
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849871"
---
# <a name="use-ip-filters"></a>IP 필터 사용

보안은 Azure IoT Hub를 기반으로 하는 모든 IoT 솔루션의 중요한 측면입니다. 경우에 따라 디바이스가 보안 구성의 일부로 연결할 수 있는 IP 주소를 명시적으로 지정해야 합니다. *IP 필터* 기능을 사용하면 특정 IPv4 주소에서 들어오는 트래픽을 거부하거나 수락하는 규칙을 구성할 수 있습니다.

## <a name="when-to-use"></a>사용 시기

특정 IP 주소에 대해 IoT Hub 엔드포인트를 차단하는 것이 유용한 두 가지 사용 사례가 있습니다.

* IoT Hub가 지정된 범위의 IP 주소에서 오는 트래픽만 수신하고 그 밖의 트래픽은 거부합니다. 예를 들어 IoT Hub를 [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services)와 사용하여 IoT Hub와 온-프레미스 인프라 간의 프라이빗 연결을 만드는 경우입니다.

* IoT Hub 관리자에 의해 의심스러운 것으로 식별된 IP 주소에서 오는 트래픽을 거부해야 합니다.

## <a name="how-filter-rules-are-applied"></a>필터 규칙이 적용되는 방식

IP 필터 규칙은 IoT Hub 서비스 수준에 적용됩니다. 따라서 IP 필터 규칙은 지원되는 모든 프로토콜을 사용하는 디바이스 및 백 엔드 앱의 모든 연결에 적용됩니다. 그러나 [기본 제공 Event Hub 호환 엔드포인트](iot-hub-devguide-messages-read-builtin.md)에서 직접(IoT Hub 연결 문자열을 통하지 않음) 읽는 클라이언트는 IP 필터 규칙에 바인딩되지 않습니다. 

IoT Hub의 거부 IP 규칙에 일치하는 IP 주소에서 오는 모든 연결 시도는 권한 없음 401 상태 코드 및 설명을 수신합니다. 응답 메시지는 IP 규칙을 언급하지 않습니다. IP 주소를 거부하면 다른 Azure 서비스(예: Azure Stream Analytics, Azure Virtual Machines 또는 Azure Portal의 Device Explorer)가 IoT 허브와 상호 작용하는 것을 막을 수 있습니다.

> [!NOTE]
> ASA(Azure Stream Analytics)를 사용하여 IP 필터링을 사용하도록 설정한 상태에서 IoT 허브의 메시지를 읽어야 하는 경우 이벤트 허브와 호환되는 IoT 허브 이름 및 엔드포인트를 사용하여 수동으로 ASA에 [Event Hubs 스트림 입력](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs)을 추가합니다.

## <a name="default-setting"></a>기본 설정

기본적으로 IoT Hub에 대한 포털의 **IP 필터** 그리드는 비어있습니다. 이러한 기본 설정은 허브가 모든 IP 주소의 연결을 수락한다는 것을 의미합니다. 이러한 기본 설정은 0.0.0.0/0 IP 주소 범위를 수락하는 규칙과 같습니다.

IP 필터 설정 페이지로 이동하려면 **네트워킹**, **공용 액세스**를 차례로 선택한 다음, **선택한 IP 범위**를 선택합니다.

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-default.png" alt-text="IoT Hub 기본 IP 필터 설정":::

## <a name="add-or-edit-an-ip-filter-rule"></a>IP 필터 규칙 추가 또는 편집

IP 필터 규칙을 추가하려면 **+ IP 필터 규칙 추가**를 선택합니다.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-add-rule.png" alt-text="IP 필터 규칙을 IoT 허브에 추가":::

**IP 필터 규칙 추가**를 선택한 후 필드를 입력합니다.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-after-selecting-add.png" alt-text="IP 필터 규칙 추가를 선택한 후":::

* IP 필터 규칙의 **이름**을 제공합니다. 이름은 최대 128자 길이의 대/소문자를 구분하는 고유한 영숫자 문자열이어야 합니다. ASCII 7 비트 영숫자 및 `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}`만 허용됩니다.

* 단일 IPv4 주소 또는 CIDR 표기법으로 IP 주소 블록을 제공합니다. 예를 들어 CIDR 표기법 192.168.100.0/22는 192.168.100.0에서 192.168.103.255까지 IPv4 주소 1024개를 나타냅니다.

* IP 필터 규칙에 대한 **작업**으로 **허용** 또는 **차단**을 선택합니다.

필드를 채운 후 **저장**을 선택하여 규칙을 저장합니다. 업데이트가 진행 중임을 알리는 경고가 표시됩니다.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png" alt-text="IP 필터 규칙 저장에 대한 알림":::

최대 10개의 IP 필터 규칙에 도달하면 **추가** 옵션이 비활성화됩니다.

기존 규칙을 편집하려면 변경하려는 데이터를 선택하고 변경 내용을 적용한 다음, **저장**을 선택하여 편집 내용을 저장합니다.

## <a name="delete-an-ip-filter-rule"></a>IP 필터 규칙 삭제

IP 필터 규칙을 삭제하려면 해당 행에서 휴지통 아이콘을 선택하고 **저장**을 선택합니다. 규칙이 제거되고 변경 내용이 저장됩니다.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-delete-rule.png" alt-text="IoT Hub IP 필터 규칙 삭제":::

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Azure CLI를 사용하여 IP 필터 검색 및 업데이트

IoT Hub의 IP 필터는 [Azure  CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)를 통해 검색 및 업데이트할 수 있습니다.

IoT Hub의 현재 IP 필터를 검색하려면 다음을 실행합니다.

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

그러면 JSON 개체로 돌아가고 기존 IP 필터가 `properties.ipFilterRules` 키 아래에 나열됩니다.

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

IoT Hub에 대한 새 IP 필터를 추가하려면 다음을 실행합니다.

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

IoT Hub의 기존 IP 필터를 제거하려면 다음을 실행합니다.

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

`<ipFilterIndexToRemove>`는 IoT Hub의`properties.ipFilterRules`에서 IP 필터의 순서에 해당합니다.

## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Azure PowerShell을 사용하여 IP 필터 검색 및 업데이트

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

IoT Hub의 IP 필터는 [Azure PowerShell](/powershell/azure/overview)을 통해 검색 및 설정할 수 있습니다.

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>REST를 사용하여 IP 필터 규칙 업데이트

또한 Azure 리소스 공급자의 REST 엔드포인트를 사용하여 IoT 허브의 IP 필터를 검색 및 수정할 수도 있습니다. [createorupdate 메서드](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate)의 `properties.ipFilterRules`를 참조하세요.

## <a name="ip-filter-rule-evaluation"></a>IP 필터 규칙 평가

IP 필터 규칙은 순서대로 적용되며 IP 주소와 일치하는 첫 번째 규칙이 수락 또는 거부 작업을 결정합니다.

예를 들어 192.168.100.0/22 범위의 주소를 수락하고 그 외의 주소는 거부하려는 경우 그리드에 있는 첫 번째 규칙이 주소 범위 192.168.100.0/22를 수락해야 합니다. 다음 규칙은 0.0.0.0/0 범위를 사용하여 모든 주소를 거부해야 합니다.

행의 시작 부분에 있는 세 개의 세로 점을 클릭하고 끌어서 놓기를 사용하여 그리드에서 IP 필터 규칙의 순서를 변경할 수 있습니다.

새 IP 필터 규칙 순서를 저장하려면 **저장**을 클릭합니다.

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-rule-order.png" alt-text="IoT Hub IP 필터 규칙의 순서 변경":::

## <a name="next-steps"></a>다음 단계

IoT Hub의 기능을 추가로 탐색하려면 다음을 참조하세요.

* [IoT Hub 메트릭](iot-hub-metrics.md)
