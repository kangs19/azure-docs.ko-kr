---
title: 자습서 - Azure에서 vSphere 클러스터 배포
description: AVS(Azure VMWare 솔루션)를 사용하여 Azure에 vSphere 클러스터를 배포하는 방법을 알아봅니다.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: fc753f43563650357cf43c102e94f0057b62a406
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873744"
---
# <a name="tutorial-deploy-an-avs-private-cloud-in-azure"></a>자습서: Azure에서 AVS 프라이빗 클라우드 배포

AVS(Azure VMware 솔루션)는 vSphere 클러스터를 Azure에 배포할 수 있는 기능을 제공합니다. 최소 초기 배포는 세 개의 호스트입니다. 클러스터당 최대 16개의 호스트를 한 번에 하나씩 추가할 수 있습니다. 

AVS에서는 시작 시 온-프레미스 vCenter를 사용하여 프라이빗 클라우드를 관리할 수 없으므로 로컬 vCenter 인스턴스, 가상 네트워크 등에 대한 추가 구성 및 연결을 수행해야 합니다. 이러한 절차 및 관련 사전 요구 사항은 이 자습서 시리즈에서 다룹니다.

이 자습서에서는 다음 작업 방법을 알아봅니다.

> [!div class="checklist"]
> * AVS 프라이빗 클라우드 만들기
> * 배포된 프라이빗 클라우드 확인

## <a name="prerequisites"></a>사전 요구 사항

- 활성 구독이 있는 Azure 계정. [체험 계정을 만듭니다](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- 프라이빗 클라우드를 만들 수 있는 적절한 관리 권한 및 사용 권한
- [자습서: 네트워크 검사 목록](tutorial-network-checklist.md)에서 설명한 대로 적절한 네트워킹이 구성되어 있는지 확인합니다.

## <a name="register-the-resource-provider"></a>리소스 공급자 등록

Azure VMware 솔루션을 사용하려면 먼저 리소스 공급자를 등록해야 합니다. 다음 예제에서는 리소스 공급자를 구독에 등록합니다.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

리소스 공급자를 등록하는 추가 방법은 [리소스 공급자 및 유형](../azure-resource-manager/management/resource-providers-and-types.md)을 참조하세요.

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com)에 로그인합니다.

## <a name="create-a-private-cloud"></a>프라이빗 클라우드 만들기

AVS 프라이빗 클라우드는 [Azure Portal](#azure-portal) 또는 [Azure CLI](#azure-cli)를 사용하여 만들 수 있습니다.

### <a name="azure-portal"></a>Azure portal

Azure Portal에서 **+ 새 리소스 만들기**를 선택합니다. **Marketplace 검색** 텍스트 상자에서 `Azure VMware Solution`을 입력하고, 목록에서 **Azure VMware 솔루션**을 선택합니다. **Azure VMware 솔루션** 창에서 **만들기**를 선택합니다.

**기본** 탭에서 필드 값을 입력합니다. 다음 표에는 속성에 대한 자세한 목록이 나와 있습니다.

| 필드   | 값  |
| ---| --- |
| **구독** | 배포에 사용할 구독입니다.|
| **리소스 그룹** | 프라이빗 클라우드 리소스에 대한 리소스 그룹입니다. |
| **위치** | 위치(예: **미국 동부**)를 선택합니다.|
| **리소스 이름** | AVS 프라이빗 클라우드의 이름입니다. |
| **SKU** | 다음 SKU 값을 선택합니다. AV36 |
| **호스트** | 프라이빗 클라우드 클러스터에 추가할 호스트의 수입니다. 기본값은 3입니다. 이 값은 배포 후에 늘리거나 줄일 수 있습니다.  |
| **vCenter 관리자 암호** | 클라우드 관리자 암호를 입력합니다. |
| **NSX-T 관리자 암호** | NSX-T 관리자 암호를 입력합니다. |
| **주소 블록** | 프라이빗 클라우드의 CIDR 네트워크에 대한 IP 주소 블록을 입력합니다. 예를 들어 10.175.0.0/22입니다. |

:::image type="content" source="./media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="프라이빗 클라우드 만들기" border="true":::

완료되면 **검토 + 만들기**를 선택합니다. 다음 화면에서 입력한 정보를 확인합니다. 정보가 모두 올바르면 **만들기**를 선택합니다.

> [!NOTE]
> 이 단계는 약 두 시간 정도 걸립니다. 

### <a name="azure-cli"></a>Azure CLI

또는 Azure CLI를 사용하여 Azure에서 AVS 프라이빗 클라우드를 만들 수 있습니다. 이렇게 하려면 Azure Cloud Shell을 사용할 수 있습니다. 다음 단계에서는 이 작업을 수행하는 방법을 보여 줍니다.

#### <a name="open-azure-cloud-shell"></a>Azure Cloud Shell 열기

Azure Cloud Shell은 이 항목의 단계를 실행하는 데 무료로 사용할 수 있는 대화형 셸입니다. 공용 Azure 도구가 사전 설치되어 계정에서 사용하도록 구성되어 있습니다.

Cloud Shell을 열려면 코드 블록의 오른쪽 위 모서리에 있는 **사용해 보세요**를 선택하기만 하면 됩니다. 또한 https://shell.azure.com/bash 로 이동하여 별도의 브라우저 탭에서 Cloud Shell을 시작할 수 있습니다. **복사**를 선택하여 코드 블록을 복사하여 Cloud Shell에 붙여넣고, **Enter** 키를 눌러 실행합니다.

#### <a name="create-a-resource-group"></a>리소스 그룹 만들기

[az group create](/cli/azure/group) 명령을 사용하여 리소스 그룹을 만듭니다. Azure 리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다. 다음 예제에서는 *eastus* 위치에 *myResourceGroup*이라는 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>프라이빗 클라우드 만들기

AVS 프라이빗 클라우드를 만들려면 리소스 그룹 이름, 프라이빗 클라우드 이름, 위치, 클러스터 크기를 제공해야 합니다.


|속성  |Description  |
|---------|---------|
|리소스 그룹 이름     | 프라이빗 클라우드를 배포하는 리소스 그룹의 이름입니다.        |
|프라이빗 클라우드 이름     | 프라이빗 클라우드의 이름입니다.        |
|위치     | 프라이빗 클라우드에 사용되는 위치입니다.         |
|클러스터 크기     | 클러스터의 크기입니다. 최솟값은 3입니다.         |
|네트워크 블록     | 프라이빗 클라우드에 사용할 CIDR 범위입니다. Azure 환경뿐만 아니라 온-프레미스 환경에서도 고유하게 설정하는 것이 좋습니다.        |

```azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22
```

## <a name="verify-deployment-was-successful"></a>배포가 성공했는지 확인

생성한 리소스 그룹으로 이동하여 프라이빗 클라우드를 선택합니다. 배포가 완료되면 다음 화면이 표시되고 **성공** 상태가 표시됩니다.

:::image type="content" source="./media/tutorial-create-private-cloud/validate-deployment.png" alt-text="배포된 프라이빗 클라우드 유효성 검사" border="true":::

## <a name="delete-a-private-cloud"></a>프라이빗 클라우드 삭제

더 이상 필요하지 않은 것으로 확인된 AVS 프라이빗 클라우드가 있으면 이를 삭제할 수 있습니다. 프라이빗 클라우드를 삭제하면 모든 클러스터가 해당 구성 요소와 함께 삭제됩니다.

이렇게 하려면 Azure Portal에서 프라이빗 클라우드로 이동하여 **삭제**를 선택합니다. 확인 페이지에서 프라이빗 클라우드 이름으로 확인하고, **예**를 선택합니다.

> [!CAUTION]
> 프라이빗 클라우드 삭제는 되돌릴 수 없는 작업입니다. 프라이빗 클라우드를 삭제하면 실행 중인 모든 워크로드, 구성 요소가 종료되고 공용 IP 주소를 포함하여 모든 프라이빗 클라우드 데이터 및 구성 설정이 삭제되므로 데이터를 복구할 수 없습니다. 

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * AVS 프라이빗 클라우드 만들기
> * 배포된 프라이빗 클라우드 확인

프라이빗 클라우드 클러스터에 대한 로컬 관리 설정의 일부로 프라이빗 클라우드에서 사용할 가상 네트워크를 만드는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Virtual Network 만들기](tutorial-configure-networking.md)
