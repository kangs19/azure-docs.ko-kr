---
title: Azure 센티널의 Advanced 다단계 공격 감지
description: Azure 센티널의 Fusion 기술을 사용 하 여 경고 피로를 줄이고 고급 다단계 공격 감지를 기반으로 하는 조치 가능한 인시던트를 만듭니다.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 87ca322cbdfdd8a53a3ecefcb120a961ea1bb936
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77587926"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Azure 센티널의 Advanced 다단계 공격 감지


> [!IMPORTANT]
> Azure 센티널의 일부 Fusion 기능은 현재 공개 미리 보기로 제공 됩니다.
> 이러한 기능은 서비스 수준 계약 없이 제공 되며 프로덕션 워크 로드에는 권장 되지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.



기계 학습을 기반으로 하는 Fusion 기술을 사용 하 여 Azure 센티널은 kill 체인의 다양 한 단계에서 관찰 되는 비정상 동작 및 의심 스러운 활동을 결합 하 여 다단계 공격을 자동으로 감지할 수 있습니다. 그런 다음 Azure 센티널은 catch 하기가 매우 어려운 인시던트를 생성 합니다. 이러한 인시던트는 둘 이상의 경고 또는 활동을 나타냅니다. 이러한 인시던트는 낮은 볼륨, 높은 충실도 및 높은 심각도로 설계 되었습니다.

사용자 환경에 맞게 사용자 지정 되어 있으며,이 검색은 거짓 긍정 요금을 줄일 뿐만 아니라 제한 되거나 누락 된 정보를 사용 하 여 공격을 검색할 수도 있습니다.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>고급 다단계 공격 검색 구성

이 검색은 기본적으로 Azure 센티널에서 사용 하도록 설정 됩니다. 상태를 확인 하거나 다른 솔루션을 사용 하 여 여러 경고를 기반으로 인시던트를 만들기 때문에 사용 하지 않도록 설정 하려면 다음 지침을 따르십시오.

1. 아직 수행 하지 않은 경우 [Azure Portal](https://portal.azure.com)에 로그인 합니다.

2. **Azure 센티널**  >  **구성**  >  **분석** 으로 이동 합니다.

3. **활성 규칙** 을 선택 하 고 **이름** 열에서 **Advanced 다단계 Attack Detection** 을 찾습니다. **상태** 열을 확인 하 여이 검색이 사용 되는지 여부를 확인 합니다.

4. 상태를 변경 하려면이 항목을 선택 하 고 **Advanced 다단계 Attack 검색** 블레이드에서 **편집**을 선택 합니다.

5. **규칙 만들기 마법사** 블레이드에서 상태 변경이 자동으로 선택 되므로 **다음: 검토**를 선택 하 고 **저장**을 선택 합니다. 

규칙 템플릿은 advanced 다단계 공격 감지에는 적용 되지 않습니다.

> [!NOTE]
> Azure 센티널은 현재 30 일간의 기록 데이터를 사용 하 여 기계 학습 시스템을 학습 합니다. 이 데이터는 machine learning 파이프라인을 통과할 때 Microsoft의 키를 사용 하 여 항상 암호화 됩니다. 그러나 Azure 센티널 작업 영역에서 CMK를 사용 하도록 설정한 경우에는 Azure [CMK (고객 관리 키)](customer-managed-keys.md) 를 사용 하 여 학습 데이터를 암호화 하지 않습니다. Fusion에서 옵트아웃 (opt out) 하려면 **Azure 센티널**   \>  **구성**   \>  **분석 \> 활성 규칙 \> 고급 다단계 공격 감지** 로 이동 하 고 **상태** 열에서 **사용 안 함을 선택 합니다.**

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Palo Alto Networks 및 Microsoft Defender ATP를 사용 하 여 Fusion

이러한 시나리오는 보안 분석가에서 사용 하는 두 가지 기본 로그 (Palo Alto Networks의 방화벽 로그 및 Microsoft Defender ATP의 끝점 검색 로그)를 결합 합니다. 아래 나열 된 모든 시나리오에서 외부 IP 주소와 관련 된 의심 스러운 활동이 끝점에서 검색 된 후 외부 IP 주소에서 방화벽으로 다시의 비정상 트래픽이 발생 합니다. Palo Alto 로그에서 Azure 센티널은 [위협 로그](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs)를 집중적으로 다루며, 위협이 허용 되는 경우 (의심 스러운 데이터, 파일, 홍수, 패킷, 검색, 스파이웨어, url, 바이러스, 취약점, 산, wildfires) 트래픽이 의심 스러운 것으로 간주 됩니다.

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Palo Alto Networks 방화벽으로 플래그가 지정 된 비정상 트래픽 뒤에 익명화 서비스에 대 한 네트워크 요청입니다.

이 시나리오에서 Azure 센티널은 먼저 Microsoft Defender Advanced Threat Protection이 비정상 활동을 야기 하는 익명화 서비스에 대 한 네트워크 요청을 검색 한 경고를 검색 합니다. 계정 {account name}에서 SID ID {sid} {time} (으)로 시작 되었습니다. 연결에 대 한 나가는 IP 주소는 {IndividualIp}입니다.
그런 다음 Palo Alto Networks 방화벽에서 비정상적인 활동을 {TimeGenerated}에 검색 했습니다. 이는 네트워크에서 악의적인 트래픽이 네트워크 트래픽에 대 한 대상 IP 주소를 {DestinationIP}로 입력 했음을 나타냅니다.

이 시나리오는 현재 공개 미리 보기로 제공 됩니다.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell에서 의심 스러운 네트워크 연결을 수행한 후 Palo Alto Networks 방화벽으로 플래그가 지정 된 비정상적인 트래픽이 발생 했습니다.

이 시나리오에서 Azure 센티널은 먼저 Microsoft Defender Advanced Threat Protection이 Palo Alto 네트워크 방화벽에서 감지한 비정상적인 활동을 위해 PowerShell에서 의심 스러운 네트워크 연결을 검색 했음을 감지한 경고를 검색 합니다. {Account name} 계정에 SID ID {sid} ({time})로 시작 되었습니다. 연결에 대 한 나가는 IP 주소는 {IndividualIp}입니다. 그런 다음 Palo Alto Networks 방화벽에서 비정상적인 활동을 {TimeGenerated}에 검색 했습니다. 이는 악의적인 트래픽이 네트워크에 들어간 것을 나타냅니다. 네트워크 트래픽에 대 한 대상 IP 주소가 {DestinationIP}입니다.

이 시나리오는 현재 공개 미리 보기로 제공 됩니다.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Palo Alto Networks 방화벽으로 플래그가 지정 된 비정상 트래픽 다음에 무단 액세스 시도 기록을 사용 하 여 IP에 대 한 아웃 바운드 연결

이 시나리오에서 Azure 센티널은 Microsoft Defender Advanced Threat Protection이 Palo Alto Networks 방화벽에서 비정상적인 활동을 감지 하는 무단 액세스 시도 기록을 사용 하 여 IP 주소에 대 한 아웃 바운드 연결을 검색 하는 경고를 검색 합니다. {Account name} 계정에 SID ID {sid} ({time})로 시작 되었습니다. 연결에 대 한 나가는 IP 주소는 {IndividualIp}입니다. 그러면 Palo Alto Networks 방화벽에서 비정상적인 활동이 {TimeGenerated}에 검색 되었습니다. 이는 악의적인 트래픽이 네트워크에 들어간 것을 나타냅니다. 네트워크 트래픽에 대 한 대상 IP 주소가 {DestinationIP}입니다.

이 시나리오는 현재 공개 미리 보기로 제공 됩니다.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Id 보호 및 Microsoft Cloud App Security를 사용한 Fusion

Azure 센티널은 고급 다단계 공격 감지를 사용 하 여 Azure Active Directory Identity Protection 및 Microsoft Cloud App Security에서 변칙 이벤트를 결합 하는 다음과 같은 시나리오를 지원 합니다.

- [비정상적 위치로 이동 불가능 한 후 비정상적인 Office 365 활동](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [익숙하지 않은 위치에 대 한 로그인 활동 후 비정상적인 Office 365 활동](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [감염 된 장치에서의 로그인 활동 후 비정상적인 Office 365 활동](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [익명 IP 주소와 비정상적인 Office 365 활동의 로그인 활동](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [누출 된 자격 증명을 사용 하 고 비정상적인 Office 365 작업을 수행한 사용자의 로그인 활동](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

[Azure AD ID 보호 데이터 커넥터](connect-azure-ad-identity-protection.md) 와 [Cloud App Security](connect-cloud-app-security.md) 커넥터가 구성 되어 있어야 합니다.

다음 설명에서 Azure 센티널은이 페이지에 표시 되는 데이터의 실제 값을 대괄호로 묶인 변수로 표시 합니다. 예를 들어, 대신 계정의 실제 표시 이름과가 아닌 \<*account name*> 실제 숫자가 \<*number*> 있습니다.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>비정상적 위치로 이동 불가능 한 후 비정상적인 Office 365 활동

Microsoft Cloud App Security에서 생성 된 Azure AD ID 보호 및 비정상적인 Office 365 경고의 불규칙 한 위치 경고에 불가능 한 이동을 결합 하는 7 개의 가능한 Azure 센티널 인시던트가 있습니다.

- **Office 365 사서함 반출으로 이어지는 불규칙 한 위치로 이동 불가능**
    
    이 경고는 \<*account name*> \<*location*> 사용자의 받은 편지함에 의심 스러운 받은 편지함 전달 규칙이 설정 된, 불규칙 하지 않은 위치로 이동 하는 경우의 로그인 이벤트를 나타냅니다.
    
    이는 계정이 손상 된 것을 나타낼 수 있으며, 사서함이 조직의 정보를 탈취 하는 데 사용 되 고 있음을 나타낼 수 있습니다. 사용자가 \<*account name*> 모든 수신 전자 메일을 외부 주소로 전달 하는 받은 편지함 전달 규칙을 만들거나 업데이트 했습니다 \<*email address*> .

- **의심 스러운 클라우드 앱 관리 활동으로 이어지는 불규칙 한 위치로 이동 불가능**
    
    이 경고는 \<*account name*> 비정상적인 위치로 이동할 수 없는 경우의 로그인 이벤트를 나타냅니다 \<*location*> .
    
    그런 다음 \<*account name*> \<*number*> 단일 세션에서 관리 활동을 통해 수행 되는 계정입니다.

- **대량 파일 삭제를 위한 비정상적 위치로 이동 불가능**
    
    이 경고는 \<*account name*> 비정상적인 위치인에의 한 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 삭제 했습니다.

- **대량 파일 다운로드에 대 한 비정상적 위치로 이동 불가능**
    
    이 경고는 \<*account name*> 비정상적인 위치로 이동할 수 없는 경우의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음, 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유한 파일에 대해 다운로드 됩니다.

- **Office 365 가장으로 이어지는 불규칙 위치로 이동할 수 없습니다.**
    
    이 경고는 \<*account name*> 비정상적인 위치로 이동할 수 없는 경우의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 계정에서 \<*account name*> \<*number of activities*> 단일 세션의 가장 많은 양의 가장 작업을 수행 했습니다.

- **대용량 파일 공유로 이어지는 불규칙 한 위치로 이동 불가능**
    
    이 경고는 \<*account name*> 비정상적인 위치로 이동할 수 없는 경우의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 통해 공유 되는 계정입니다.

- **클라우드 앱에서 랜 섬 웨어로 이어지는 불규칙 한 위치로 이동 불가능**
    
    이 경고는 \<*account name*> 비정상적인 위치로 이동할 수 없는 경우의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음, \<*account name*> \<*number of*> 파일을 업로드 하 고 총 파일을 삭제 했습니다 \<*number of*> . 
    
    이 활동 패턴은 잠재적인 랜 섬 웨어 공격을 나타냅니다.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>익숙하지 않은 위치에 대 한 로그인 활동 후 비정상적인 Office 365 활동

Microsoft Cloud App Security에 의해 생성 된 Azure AD ID 보호 및 비정상적인 Office 365 경고에서 익숙하지 않은 위치 경고에 대 한 로그인 활동을 결합 하는 7 개의 가능한 Azure 센티널 인시던트가 있습니다.

- **Exchange Online 사서함 반출으로 이어지는 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는에서 로그인 이벤트를 표시 하며 \<*account name*> \<*location*> , 익숙하지 않은 위치와 의심 스러운 수신함 전달 규칙이 사용자의 받은 편지함에 설정 된 것을 나타냅니다.
    
    이는 계정이 손상 된 것을 나타낼 수 있으며, 사서함이 조직의 정보를 탈취 하는 데 사용 되 고 있음을 나타낼 수 있습니다. 사용자가 \<*account name*> 모든 수신 전자 메일을 외부 주소로 전달 하는 받은 편지함 전달 규칙을 만들거나 업데이트 했습니다 \<*email address*> . 

- **의심 스러운 클라우드 앱 관리 활동으로 이어지는 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 관리 활동을 통해 수행 되는 계정입니다.

- **대량 파일 삭제를 위한 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 삭제 했습니다.

- **대량 파일 다운로드를 위한 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음, 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유한 파일에 대해 다운로드 됩니다.

- **Office 365 가장으로 이어지는 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> .
    
    그런 다음, \<*account name*> \<*number of*> 단일 세션에서 다른 계정에 대해 가장 된 계정입니다.

- **대량 파일 공유에 대 한 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 통해 공유 되는 계정입니다.

- **클라우드 앱에서 랜 섬 웨어로 이어지는 익숙하지 않은 위치의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익숙하지 않은 위치인에서의 로그인 이벤트를 나타냅니다 \<*location*> . 
    
    그런 다음, \<*account name*> \<*number of*> 파일을 업로드 하 고 총 파일을 삭제 했습니다 \<*number of*> . 
    
    이 활동 패턴은 잠재적인 랜 섬 웨어 공격을 나타냅니다.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>감염 된 장치에서의 로그인 활동 후 비정상적인 Office 365 활동

Microsoft Cloud App Security에 의해 생성 된 Azure AD ID 보호 및 비정상 Office 365 경고에서 감염 된 장치 경고와의 로그인 활동을 결합 하는 7 개의 가능한 Azure 센티널 인시던트가 있습니다.

- **Office 365 사서함 반출으로 이어지는 감염 된 장치에서 로그인 이벤트**
    
    이 경고는 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 표시 한 \<*account name*> 다음, 사용자의 수신함에 의심 스러운 수신함 전달 규칙이 설정 된 것을 나타냅니다.
    
    이는 계정이 손상 된 것을 나타낼 수 있으며, 사서함이 조직의 정보를 탈취 하는 데 사용 되 고 있음을 나타낼 수 있습니다. 사용자가 \<*account name*> 모든 수신 전자 메일을 외부 주소로 전달 하는 받은 편지함 전달 규칙을 만들거나 업데이트 했습니다 \<*email address*> . 

- **의심 스러운 클라우드 앱 관리 활동으로 이어지는 감염 된 장치에서 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다.
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 관리 활동을 통해 수행 되는 계정입니다.

- **대량 파일 삭제를 위한 감염 된 장치에서의 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다. 
    
    그런 다음 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 삭제 했습니다.

- **대량 파일 다운로드를 위한 감염 된 장치에서의 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다. 
    
    그런 다음, 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유한 파일에 대해 다운로드 됩니다.

- **Office 365 가장으로 이어지는 감염 된 장치에서 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다. 
    
    그런 다음, \<*account name*> \<*number of*> 단일 세션에서 다른 계정에 대해 가장 된 계정입니다.

- **대량 파일 공유에 대 한 감염 된 장치에서의 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다. 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 통해 공유 되는 계정입니다.

- **클라우드 앱에서 랜 섬 웨어를 주도하는 감염 된 장치에서의 로그인 이벤트**
    
    이 경고는 \<*account name*> 맬웨어의 감염 가능성이 있는 장치에서의 로그인 이벤트를 나타냅니다. 
    
    그런 다음, \<*account name*> \<*number of*> 파일을 업로드 하 고 총 파일을 삭제 했습니다 \<*number of*> . 
    
    이 활동 패턴은 잠재적인 랜 섬 웨어 공격을 나타냅니다.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>익명 IP 주소와 비정상적인 Office 365 활동의 로그인 활동

Microsoft Cloud App Security에서 생성 된 Azure AD ID 보호 및 비정상 Office 365 경고의 익명 IP 주소 경고와의 로그인 활동을 결합 하는 7 개의 가능한 Azure 센티널 인시던트가 있습니다.

- **Office 365 사서함 반출으로 이어지는 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 익명 프록시 IP 주소에서의 로그인 이벤트를 표시 한 \<*account name*> \<*IP address*> 다음, 사용자의 수신함에 의심 스러운 수신함 전달 규칙이 설정 된 것을 나타냅니다.
    
    이는 계정이 손상 된 것을 나타낼 수 있으며, 사서함이 조직의 정보를 탈취 하는 데 사용 되 고 있음을 나타낼 수 있습니다. 사용자가 \<*account name*> 모든 수신 전자 메일을 외부 주소로 전달 하는 받은 편지함 전달 규칙을 만들거나 업데이트 했습니다 \<*email address*> . 

- **의심 스러운 클라우드 앱 관리 활동으로 이어지는 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 관리 활동을 통해 수행 되는 계정입니다.

- **대량 파일 삭제로 이어지는 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 삭제 했습니다.

- **대량 파일 다운로드에 대 한 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음, 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유한 파일에 대해 다운로드 됩니다.

- **Office 365 가장으로 이어지는 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음, \<*account name*> \<*number of*> 단일 세션에서 다른 계정에 대해 가장 된 계정입니다.

- **대량 파일 공유로 향하는 익명 IP 주소의 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 통해 공유 되는 계정입니다.

- **익명 IP 주소에서 클라우드 앱의 랜 섬 웨어로 로그인 이벤트**
    
    이 경고는 \<*account name*> 익명 프록시 IP 주소에서의 로그인 이벤트를 나타냅니다 \<*IP address*> . 
    
    그런 다음, \<*account name*> \<*number of*> 파일을 업로드 하 고 총 파일을 삭제 했습니다 \<*number of*> . 
    
    이 활동 패턴은 잠재적인 랜 섬 웨어 공격을 나타냅니다.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>누출 된 자격 증명을 사용 하 고 비정상적인 Office 365 작업을 수행한 사용자의 로그인 활동

Microsoft Cloud App Security에서 생성 된 Azure AD ID 보호 및 비정상 Office 365 경고의 누수 된 자격 증명 경고와 함께 사용자의 로그인 활동을 결합 하는 7 개의 가능한 Azure 센티널 인시던트가 있습니다.

- **Office 365 사서함 반출을 사용 하는 누출 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는 \<*account name*> 사용자의 수신함에서 의심 스러운 받은 편지함 전달 규칙을 사용 하 여 누수 된 자격 증명을 사용한 로그인 이벤트를 나타냅니다. 
    
    이는 계정이 손상 된 것을 나타낼 수 있으며, 사서함이 조직의 정보를 탈취 하는 데 사용 되 고 있음을 나타낼 수 있습니다. 사용자가 \<*account name*> 모든 수신 전자 메일을 외부 주소로 전달 하는 받은 편지함 전달 규칙을 만들거나 업데이트 했습니다 \<*email address*> . 

- **의심 스러운 자격 증명이 있는 사용자의 로그인 이벤트로 의심 되는 클라우드 앱 관리 활동**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다.
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 관리 활동을 통해 수행 되는 계정입니다.

- **대량 파일 삭제를 초래 하는 누출 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다.
    
    그런 다음 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 삭제 했습니다.

- **대량 파일 다운로드를 위한 누출 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다.
    
    그런 다음, 계정이 \<*account name*> \<*number of*> 단일 세션에서 고유한 파일에 대해 다운로드 됩니다.

- **Office 365 가장에 대 한 유출 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다. 
    
    그런 다음, \<*account name*> \<*number of*> 단일 세션에서 다른 계정에 대해 가장 된 계정입니다.

- **대용량 파일 공유에 대 한 유출 된 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다.
    
    그런 다음 \<*account name*> \<*number of*> 단일 세션에서 고유 파일을 통해 공유 되는 계정입니다.

- **클라우드 앱의 랜 섬 웨어에 유출 된 자격 증명이 있는 사용자의 로그인 이벤트**
    
    이 경고는의 로그인 이벤트가 \<*account name*> 유출 된 자격 증명을 사용 했음을 나타냅니다. 
    
    그런 다음, \<*account name*> \<*number of*> 파일을 업로드 하 고 총 파일을 삭제 했습니다 \<*number of*> . 
    
    이 활동 패턴은 잠재적인 랜 섬 웨어 공격을 나타냅니다.

## <a name="next-steps"></a>다음 단계

이제 고급 다단계 공격 검색에 대해 자세히 알아보았습니다. 다음 빠른 시작을 사용 하 여 데이터 및 잠재적 위협에 대 한 가시성을 얻는 방법에 대해 알아볼 수 있습니다. [Azure 센티널 시작](quickstart-get-visibility.md).

사용자를 위해 생성 된 인시던트를 조사할 준비가 된 경우 다음 자습서: [Azure 센티널로 인시던트 조사](tutorial-investigate-cases.md)를 참조 하세요.

