---
title: Azure Portal에서 작업 그룹 만들기 및 관리
description: Azure Portal에서 작업 그룹을 만들고 관리하는 방법에 대해 알아봅니다.
author: dkamstra
ms.topic: conceptual
ms.date: 6/5/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: dbc810ad7227d9d47099fe85e89a92c8fa750302
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465255"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure Portal에서 작업 그룹 만들기 및 관리
작업 그룹은 Azure 구독 소유자가 정의한 알림 기본 설정 컬렉션입니다. Azure Monitor 및 Service Health 경고는 작업 그룹을 사용하여 경고가 트리거되었음을 사용자에게 알립니다. 사용자의 요구 사항에 따라 다양한 경고가 동일한 작업 그룹을 사용할 수도 있고 서로 다른 작업 그룹을 사용할 수도 있습니다. 구독에서는 작업 그룹을 2,000개까지 구성할 수 있습니다.

이메일 또는 SMS를 통해 개인에게 알리는 작업을 구성하면 해당 개인이 작업 그룹에 추가되었다는 확인 메시지를 수신합니다.

이 문서에서는 Azure Portal에서 작업 그룹을 만들고 관리하는 방법을 보여 줍니다.

각 작업은 다음과 같은 속성으로 구성됩니다.

* **Name**: 작업 그룹 내의 고유 식별자입니다.  
* **작업 유형**: 실행된 동작. 음성 통화, SMS, 이메일 보내기나, 여러 자동화된 작업 유형 트리거를 예로 들 수 있습니다. 이 문서 뒷부분에 나오는 유형을 참조하세요.
* **세부 정보**: 작업 유형에 따라 달라지는 해당 세부 정보입니다.

Azure 리소스 관리자 템플릿을 사용하여 작업 그룹을 구성하는 방법에 대한 자세한 내용은 [작업 그룹 리소스 관리자 템플릿](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)을 참조하세요.

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure Portal을 사용하여 작업 그룹 만들기

1. [Azure Portal](https://portal.azure.com)에서 **모니터**를 검색하여 선택합니다. **모니터** 창은 모든 모니터링 설정과 데이터를 하나의 보기로 통합합니다.

1. **경고**를 선택한 다음, **작업 관리**를 선택합니다.

    ![작업 관리 단추](./media/action-groups/manage-action-groups.png)
    
1. **작업 그룹 추가**를 선택하고 필드를 입력합니다.

    ![“작업 그룹 추가” 명령](./media/action-groups/add-action-group.png)
    
1. **작업 그룹 이름** 상자에 이름을 입력하고 **약식 이름** 상자에 이름을 입력합니다. 약식 이름은 이 그룹을 사용하여 알림을 보내는 경우 전체 작업 그룹 이름 대신 사용됩니다.

      ![“작업 그룹 추가” 대화 상자](./media/action-groups/action-group-define.png)

1. **구독** 상자가 현재 구독으로 자동으로 채워집니다. 이 구독은 작업 그룹이 저장되는 위치입니다.

1. 작업 그룹이 저장되는 **리소스 그룹**을 선택합니다.

1. 작업 목록을 정의합니다. 각 작업에 대해 다음을 제공합니다.

    1. **Name**: 이 작업에 대한 고유 식별자를 입력합니다.

    1. **작업 유형**: Automation Runbook, Azure 함수, 이메일 Azure Resource Manager 역할, 이메일/SMS/푸시/음성, ITSM, 논리 앱, 보안 웹후크, 웹후크를 선택합니다.

    1. **세부 정보**: 작업 유형에 따라 전화 번호, 메일 주소, 웹후크 URI, Azure 앱, ITSM 연결 또는 Automation Runbook을 입력합니다. ITSM 작업의 경우 **작업 항목** 및 ITSM 도구에 필요한 다른 필드를 추가로 지정합니다.
    
    1. **일반 경고 스키마**: Azure Monitor의 모든 경고 서비스에서 확장 가능하고 통합된 단일 경고 페이로드를 사용하는 이점을 제공하는 [일반 경고 스키마](https://aka.ms/commonAlertSchemaDocs)를 사용하도록 선택할 수 있습니다.

1. **확인**을 선택하여 작업 그룹을 만듭니다.

## <a name="manage-your-action-groups"></a>작업 그룹 관리

작업 그룹을 만든 후 **모니터** 창의 **경고** 방문 페이지에서 **작업 관리**를 선택하여 **작업 그룹**을 볼 수 있습니다. 관리하려는 작업 그룹을 선택합니다.

* 작업을 추가, 편집 또는 제거합니다.
* 작업 그룹을 삭제합니다.

## <a name="action-specific-information"></a>작업별 정보

> [!NOTE]
> 아래의 각 항목에 대한 숫자 제한은 [모니터링에 대한 구독 서비스 제한](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-monitor-limits)을 참조하세요.  

### <a name="automation-runbook"></a>자동화 Runbook
Runbook 페이로드에 대한 제한 사항은 [Azure 구독 서비스 제한](../../azure-resource-manager/management/azure-subscription-service-limits.md)을 참조하세요.

작업 그룹에서 Runbook 작업의 수가 제한될 수 있습니다. 

### <a name="azure-app-push-notifications"></a>Azure 앱 푸시 알림
작업 그룹에서 Azure 앱 작업의 수가 제한될 수 있습니다.

### <a name="email"></a>Email
다음 이메일 주소에서 이메일이 전송됩니다. 이메일 필터링이 적절하게 구성되었는지 확인합니다.
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

작업 그룹에서 이메일 작업의 수가 제한될 수 있습니다. [속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서를 참조하세요.

### <a name="email-azure-resource-manager-role"></a>메일 Azure Resource Manager 역할
구독 역할의 멤버에게 이메일을 보냅니다. 이메일은 해당 역할의 **Azure AD 사용자** 멤버에게만 발송됩니다. Azure AD 그룹 또는 서비스 주체에게는 이메일이 전송되지 않습니다.

작업 그룹에서 이메일 작업의 수가 제한될 수 있습니다. [속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서를 참조하세요.

### <a name="function"></a>함수
[Azure Functions](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app)에서 기존 HTTP 트리거 엔드포인트를 호출합니다.

작업 그룹에서 함수 작업의 수가 제한될 수 있습니다.

### <a name="itsm"></a>ITSM
ITSM 작업에는 ITSM 연결이 필요합니다. [ITSM 연결](../../azure-monitor/platform/itsmc-overview.md)을 만드는 방법에 대해 알아봅니다.

작업 그룹에서 ITSM 작업의 수가 제한될 수 있습니다. 

### <a name="logic-app"></a>논리 앱
작업 그룹에서 논리 앱 작업의 수가 제한될 수 있습니다.

### <a name="secure-webhook"></a>보안 웹후크
작업 그룹 웹후크 작업을 사용하면 Azure Active Directory를 활용하여 작업 그룹과 보호된 웹 API(웹후크 엔드포인트) 간의 연결을 보호할 수 있습니다. 이 기능을 활용하기 위한 전체 워크플로는 아래에 설명되어 있습니다. Azure AD 애플리케이션 및 서비스 주체에 대한 개요는 [Microsoft ID 플랫폼(v2.0) 개요](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)를 참조하세요.

1. 보호된 웹 API에 대한 Azure AD 애플리케이션을 만듭니다. https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview 을 참조하세요.
    - 디먼 앱에서 호출하도록 보호된 API를 구성합니다.
    
1. Azure AD 애플리케이션을 사용하도록 작업 그룹을 설정합니다.

    > [!NOTE]
    > 이 스크립트를 실행하려면 [Azure AD 애플리케이션 관리자 역할](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles)의 멤버여야 합니다.
    
    - PowerShell 스크립트의 Connect-AzureAD 호출을 수정하여 Azure AD 테넌트 ID를 사용합니다.
    - PowerShell 스크립트의 변수 $myAzureADApplicationObjectId 수정 하 여 Azure AD 응용 프로그램의 개체 ID를 사용 합니다.
    - 수정된 스크립트를 실행합니다.
    
1. 작업 그룹 보안 웹후크 작업을 구성합니다.
    - 스크립트에서 $myApp.ObjectId 값을 복사하여 웹후크 작업 정의의 애플리케이션 개체 ID 필드에 입력합니다.
    
    ![보안 웹후크 작업](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>보안 웹후크 PowerShell 스크립트

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>sms
[속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 및 [SMS 경고 동작](../../azure-monitor/platform/alerts-sms-behavior.md)에서 중요한 추가 정보를 참조하세요. 

작업 그룹에서 SMS 작업의 수가 제한될 수 있습니다.

> [!NOTE]
> Azure Portal 작업 그룹 사용자 인터페이스에서 국가/지역 코드를 선택할 수 없는 경우 해당 국가/지역에 대해서는 SMS가 지원되지 않는 것입니다.  국가/지역 코드를 사용할 수 없는 경우 [사용자 의견](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice)에서 사용자의 국가/지역이 추가되도록 투표할 수 있습니다. 그동안에는 작업 그룹에서 해당 국가/지역을 지원하는 타사 SMS 공급자에 대한 웹후크를 호출하도록 하여 문제를 해결합니다.  

지원되는 국가/지역에 대한 가격은 [Azure Monitor 가격 책정 페이지](https://azure.microsoft.com/pricing/details/monitor/)에 나와 있습니다.
  

### <a name="voice"></a>음성
[속도 제한 정보](./../../azure-monitor/platform/alerts-rate-limiting.md) 문서에서 중요한 추가 동작을 참조하세요.

작업 그룹에서 음성 작업의 수가 제한될 수 있습니다.

> [!NOTE]
> Azure Portal 작업 그룹 사용자 인터페이스에서 국가/지역 코드를 선택할 수 없는 경우 해당 국가/지역에 대해서는 음성 통화가 지원되지 않는 것입니다. 국가/지역 코드를 사용할 수 없는 경우 [사용자 의견](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice)에서 사용자의 국가/지역이 추가되도록 투표할 수 있습니다.  그동안에는 작업 그룹에서 해당 국가/지역을 지원하는 타사 음성 통화 공급자에 대한 웹후크를 호출하도록 하여 문제를 해결합니다.  

지원되는 국가/지역에 대한 가격은 [Azure Monitor 가격 책정 페이지](https://azure.microsoft.com/pricing/details/monitor/)에 나와 있습니다.

### <a name="webhook"></a>웹후크
웹 후크는 다음 규칙을 사용 하 여 처리 됩니다.
- Webhook 호출은 최대 3 회 시도 됩니다.
- 제한 시간 내에 응답이 수신 되지 않거나 다음 HTTP 상태 코드 중 하나가 반환 되 면 (408, 429, 503 또는 504) 호출을 다시 시도 합니다.
- 첫 번째 호출은 응답을 위해 10 초를 기다립니다.
- 두 번째와 세 번째 시도는 응답에 대해 30 초 동안 대기 합니다.
- 3 번의 webhook 호출이 실패 한 후에는 작업 그룹에서 15 분 동안 끝점을 호출 하지 않습니다.

원본 IP 주소 범위
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 13.106.57.197
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

이러한 IP 주소에 대한 변경 내용에 대한 업데이트를 받으려면 작업 그룹 서비스에 대한 정보 알림을 모니터링하는 Service Health 경고를 구성하는 것이 좋습니다.

작업 그룹에서 웹후크 작업의 수가 제한될 수 있습니다.



## <a name="next-steps"></a>다음 단계
* [SMS 경고 동작](../../azure-monitor/platform/alerts-sms-behavior.md)에 대해 자세히 알아보세요.  
* [활동 로그 경고 웹후크 스키마의 이해](../../azure-monitor/platform/activity-log-alerts-webhook.md)를 확인해 보세요.  
* [ITSM 커넥터](../../azure-monitor/platform/itsmc-overview.md)에 대해 자세히 알아보세요.
* 경고의 [속도 제한](../../azure-monitor/platform/alerts-rate-limiting.md)에 대해 자세히 알아보세요.
* [활동 로그 경고의 개요](../../azure-monitor/platform/alerts-overview.md)를 확인하고 경고를 받는 방법에 대해 알아보세요.  
* [서비스 상태 알림이 게시될 때마다 경고를 구성](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)하는 방법을 알아보세요.
