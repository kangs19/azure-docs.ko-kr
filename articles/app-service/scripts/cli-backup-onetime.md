---
title: 'CLI: 앱 백업'
description: Azure CLI를 사용하여 App Service 앱의 배포 및 관리를 자동화하는 방법을 알아봅니다. 이 샘플에서는 앱을 백업하는 방법을 보여줍니다.
author: msangapu-msft
tags: azure-service-management
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/07/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: mvc, seodec18
ms.openlocfilehash: fd310e81c6a86e651e27518acfdefa66980fa938
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80059149"
---
# <a name="back-up-an-app-using-cli"></a>CLI를 사용하여 앱 백업

이 샘플 스크립트는 App Service에 관련 리소스가 있는 앱을 만든 다음, 이에 대한 일회성 백업을 만듭니다. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택하는 경우 Azure CLI 버전 2.0 이상이 필요합니다. 버전을 확인하려면 `az --version`을 실행합니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요.

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-onetime/backup-onetime.sh?highlight=3-7 "Back up an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) | 스토리지 계정을 만듭니다. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) | Azure Storage 컨테이너를 만듭니다. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-generate-sas) | Azure Storage 컨테이너에 대한 SAS 토큰을 생성합니다.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service 계획을 만듭니다. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | App Service 앱을 만듭니다. |
| [`az webapp config backup create`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-create) | App Service 앱에 대한 백업을 만듭니다. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-list) | App Service 앱에 대한 백업 목록을 가져옵니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 App Service CLI 스크립트 샘플은 [Azure App Service 설명서](../samples-cli.md)에서 확인할 수 있습니다.
