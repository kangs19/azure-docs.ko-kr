---
title: 휴지 상태의 데이터에 대 한 Personalizer 서비스 암호화
titleSuffix: Azure Cognitive Services
description: 휴지 상태의 데이터에 대 한 서비스 암호화를 Personalizer 합니다.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 10eb627a340b45c93b2cfb2973e294d8d5d7c7e5
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307826"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>휴지 상태의 데이터에 대 한 Personalizer 서비스 암호화

Personalizer 서비스는 데이터를 클라우드에 보관할 때 데이터를 자동으로 암호화 합니다. Personalizer 서비스 암호화는 데이터를 보호 하 고 조직의 보안 및 규정 준수 약정을 충족 하는 데 도움을 줍니다.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> 고객 관리 키는 E0 가격 책정 계층 에서만 사용할 수 있습니다. 고객 관리 키를 사용 하는 기능을 요청 하려면 [Personalizer Service 고객이 관리 하는 키 요청 양식을](https://aka.ms/cogsvc-cmk)작성 하 고 제출 합니다. 요청 상태를 다시 들으려면 영업일 3-5 영업일이 소요 됩니다. 요청에 따라 큐에 배치 되 고 공간을 사용할 수 있게 되 면 승인 될 수 있습니다. Personalizer 서비스에서 CMK를 사용 하도록 승인한 후에는 새 Personalizer 리소스를 만들고이를 가격 책정 계층으로 선택 해야 합니다. E0 가격 책정 계층을 사용 하 여 Personalizer 리소스를 만든 후 Azure Key Vault를 사용 하 여 관리 되는 id를 설정할 수 있습니다.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>다음 단계

* [Personalizer Service 고객이 관리 하는 키 요청 양식](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault에 대 한 자세한 정보](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
