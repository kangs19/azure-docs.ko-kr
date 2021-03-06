---
title: Azure Functions 신뢰할 수 있는 이벤트 처리
description: Azure Functions에서 누락 이벤트 허브 메시지 방지
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: cshoe
ms.openlocfilehash: fe5efd2bf4c235688aad90ae37b54268d290540c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84676134"
---
# <a name="azure-functions-reliable-event-processing"></a>Azure Functions 신뢰할 수 있는 이벤트 처리

이벤트 처리는 서버를 사용 하지 않는 아키텍처와 관련 된 가장 일반적인 시나리오 중 하나입니다. 이 문서에서는 메시지 손실을 방지 하기 위해 Azure Functions를 사용 하 여 신뢰할 수 있는 메시지 프로세서를 만드는 방법을 설명 합니다.

## <a name="challenges-of-event-streams-in-distributed-systems"></a>분산 시스템에서 발생 하는 이벤트 스트림의 과제

초당 100 이벤트의 일정 한 속도로 이벤트를 전송 하는 시스템을 고려 합니다. 이 속도에서 몇 분 이내에 여러 병렬 함수 인스턴스는 초당 들어오는 100 이벤트를 소비할 수 있습니다.

그러나 다음과 같은 최적이 아닌 조건이 있을 수 있습니다.

- 이벤트 게시자가 손상 된 이벤트를 보내는 경우는 어떻게 되나요?
- 함수 인스턴스에 처리 되지 않은 예외가 발생 하면 어떻게 되나요?
- 다운스트림 시스템이 오프 라인 상태가 되 면 어떻게 되나요?

응용 프로그램의 처리량을 유지 하는 동안 이러한 상황을 어떻게 처리 하나요?

큐를 사용 하면 신뢰할 수 있는 메시징을 자연스럽 게 제공 합니다. 함수 트리거와 페어링된 경우 함수는 큐 메시지에 대 한 잠금을 만듭니다. 처리가 실패 하면 다른 인스턴스에서 처리를 다시 시도할 수 있도록 잠금이 해제 됩니다. 그러면 메시지가 성공적으로 평가 되거나 포이즌 큐에 추가 될 때까지 처리가 계속 됩니다.

단일 큐 메시지가 다시 시도 주기에 남아 있는 경우에도 다른 병렬 실행은 계속 해 서 남은 메시지를 큐에서 제거 합니다. 결과적으로 전체 처리량은 잘못 된 메시지 하나에 의해 거의 영향을 받지 않습니다. 그러나 저장소 큐는 순서를 보장 하지 않으며 Event Hubs에 필요한 처리량이 높은 요구에 맞게 최적화 되지 않습니다.

이와 대조적으로 Azure Event Hubs는 잠금 개념을 포함 하지 않습니다. 높은 처리량, 여러 소비자 그룹 및 재생 기능과 같은 기능을 허용 하기 위해 Event Hubs 이벤트는 비디오 플레이어 처럼 동작 합니다. 파티션 당 스트림의 단일 지점에서 이벤트를 읽습니다. 포인터를 통해 해당 위치에서 전달 하거나 뒤로 읽을 수 있지만 이벤트를 처리할 수 있도록 포인터를 이동 하도록 선택 해야 합니다.

스트림에서 오류가 발생 하면 포인터가 동일한 지점에 유지 되도록 결정 하면 포인터가 고급이 될 때까지 이벤트 처리가 차단 됩니다. 즉, 단일 이벤트를 처리 하는 문제를 처리 하기 위해 포인터가 중지 된 경우 처리 되지 않은 이벤트가 누적 시작 됩니다.

Azure Functions는 성공 또는 실패에 관계 없이 스트림의 포인터를 이동 하 여 교착 상태를 방지 합니다. 포인터는 계속 진행 되므로 함수에서 오류를 적절 하 게 처리 해야 합니다.

## <a name="how-azure-functions-consumes-event-hubs-events"></a>Azure Functions에서 Event Hubs 이벤트를 사용 하는 방법

다음 단계를 진행 하는 동안 이벤트 허브 이벤트를 사용 Azure Functions 합니다.

1. 포인터는 이벤트 허브의 각 파티션에 대해 Azure Storage에 생성 되 고 유지 됩니다.
2. 새 메시지를 받은 경우 (기본적으로 일괄 처리에서) 호스트는 메시지 일괄 처리를 사용 하 여 함수 트리거를 시도 합니다.
3. 함수에서 예외를 사용 하거나 사용 하지 않고 실행을 완료 하면 포인터가 이동 하 고 검사점이 저장소 계정에 저장 됩니다.
4. 조건으로 인해 함수 실행을 완료할 수 없는 경우 호스트에서 포인터를 처리 하지 못합니다. 포인터가 고급이 아닌 경우에는 나중에 동일한 메시지가 처리 되는 것을 확인 합니다.
5. 2-4 단계 반복

이 동작을 수행 하면 다음과 같은 몇 가지 중요 한 사항이 나타납니다.

- *처리 되지 않은 예외로 인해 메시지가 손실 될 수 있습니다.* 예외가 발생 하는 실행은 계속 해 서 포인터를 진행 합니다.
- *함수는 한 번 이상 배달 되도록 보장 합니다.* 코드와 종속 시스템 [은 동일한 메시지를 두 번 받을 수 있다는 사실을 고려해](./functions-idempotent.md)야 할 수 있습니다.

## <a name="handling-exceptions"></a>예외 처리

일반적으로 모든 함수는 가장 높은 수준의 코드에서 [try/catch 블록](./functions-bindings-error-pages.md) 을 포함 해야 합니다. 특히 Event Hubs 이벤트를 사용 하는 모든 함수에는 `catch` 블록이 있어야 합니다. 이렇게 하면 예외가 발생 하면 catch 블록은 포인터가 진행 되기 전에 오류를 처리 합니다.

### <a name="retry-mechanisms-and-policies"></a>다시 시도 메커니즘 및 정책

일부 예외는 본래 일시적 이며 나중에 작업을 다시 시도 하는 경우 다시 표시 되지 않습니다. 첫 번째 단계는 항상 작업을 다시 시도 하는 것입니다. 재시도 처리 규칙을 직접 작성할 수 있지만 많은 도구를 사용할 수 있는 것이 일반적입니다. 이러한 라이브러리를 사용 하 여 처리 순서를 유지 하는 데 도움이 되는 강력한 다시 시도 정책을 정의할 수 있습니다.

함수에 오류 처리 라이브러리를 도입 하면 기본 및 고급 재시도 정책을 모두 정의할 수 있습니다. 예를 들어 다음 규칙에 설명 된 워크플로를 따르는 정책을 구현할 수 있습니다.

- 메시지를 세 번 삽입 합니다 (다시 시도 사이에 지연이 있을 수 있음).
- 모든 다시 시도의 최종 결과가 실패 인 경우 스트림에 대 한 처리를 계속할 수 있도록 메시지를 큐에 추가 합니다.
- 그러면 손상 되거나 처리 되지 않은 메시지가 나중에 처리 됩니다.

> [!NOTE]
> [Polly](https://github.com/App-vNext/Polly)는 응용 프로그램에 대한 C# 복원력 및 일시적인 오류 처리 라이브러리의 한 가지 예입니다.

미리 컴파일된 c # 클래스 라이브러리를 사용 하는 경우 [예외 필터](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-catch) 를 사용 하 여 처리 되지 않은 예외가 발생할 때마다 코드를 실행할 수 있습니다.

예외 필터를 사용 하는 방법을 보여 주는 샘플은 [AZURE WEBJOBS SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) 리포지토리에서 사용할 수 있습니다.

## <a name="non-exception-errors"></a>예외가 아닌 오류

오류가 없는 경우에도 일부 문제가 발생 합니다. 예를 들어 실행 중에 발생 하는 오류를 살펴보겠습니다. 이 경우 함수가 실행을 완료 하지 못하면 오프셋 포인터가 진행 되지 않습니다. 포인터가 이동 하지 않으면 실행이 실패 한 후 실행 되는 모든 인스턴스는 동일한 메시지를 계속 읽습니다. 이 경우 "최소 한 번"의 보장을 제공 합니다.

모든 메시지가 한 번 이상 처리 되도록 보장 하는 것은 일부 메시지가 두 번 이상 처리 될 수 있다는 것을 의미 합니다. 함수 앱은 이러한 가능성을 인식 해야 하며 [멱 등 성의 원칙](./functions-idempotent.md)을 중심으로 빌드해야 합니다.

## <a name="stop-and-restart-execution"></a>실행 중지 및 다시 시작

몇 가지 오류가 허용 될 수 있지만 앱에 심각한 오류가 발생 하는 경우 어떻게 되나요? 시스템이 정상 상태에 도달할 때까지 이벤트 트리거를 중지 하는 것이 좋습니다. 일반적으로 회로 차단기 패턴을 사용 하 여 처리를 일시 중지할 수 있습니다. 회로 차단기 패턴을 사용 하면 앱이 이벤트 프로세스의 "회로를 중단" 하 고 나중에 다시 시작할 수 있습니다.

이벤트 프로세스에서 회로 차단기를 구현 하는 데 필요한 두 가지 부분은 다음과 같습니다.

- 회로의 상태를 추적 하 고 모니터링 하기 위해 모든 인스턴스에 걸친 공유 상태
- 회로 상태를 관리할 수 있는 마스터 프로세스 (열림 또는 닫힘)

구현 세부 정보는 다를 수 있지만 인스턴스 간에 상태를 공유 하려면 저장소 메커니즘이 필요 합니다. Azure Storage, Redis cache 또는 함수 컬렉션에서 액세스할 수 있는 다른 계정에 상태를 저장 하도록 선택할 수 있습니다.

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) 또는 지 [속성 함수](./durable/durable-functions-overview.md) 는 워크플로 및 회로 상태를 관리 하는 자연스럽 게 적합 합니다. 다른 서비스도 작동할 수 있지만이 예제에서는 논리 앱이 사용 됩니다. 논리 앱을 사용 하 여 회로 차단기 패턴을 구현 하는 데 필요한 제어를 제공 하는 함수 실행을 일시 중지 하 고 다시 시작할 수 있습니다.

### <a name="define-a-failure-threshold-across-instances"></a>인스턴스 간 오류 임계값 정의

이벤트를 동시에 처리 하는 여러 인스턴스를 고려 하려면 회로 상태를 모니터링 하기 위해 공유 외부 상태를 유지 해야 합니다.

구현 하도록 선택할 수 있는 규칙은 다음을 적용할 수 있습니다.

- 모든 인스턴스에 대해 30 초 이내에 100 개가 넘는 최종 오류가 발생 한 경우 회로를 중단 하 고 새 메시지의 트리거를 중지 합니다.

구현 세부 정보는 요구 사항에 따라 다를 수 있지만 일반적으로 다음과 같은 시스템을 만들 수 있습니다.

1. 저장소 계정 (Azure Storage, Redis 등)에 대 한 오류를 기록 합니다.
1. 새 실패가 기록 될 때 롤링 횟수를 검사 하 여 임계값이 충족 되는지 확인 합니다 (예: 지난 30 초 동안 100 이상).
1. 임계값이 충족 되 면 시스템에 회로를 중단 하도록 지시 하는 이벤트를 Azure Event Grid 내보냅니다.

### <a name="managing-circuit-state-with-azure-logic-apps"></a>Azure Logic Apps를 사용 하 여 회로 상태 관리

다음 설명에서는 함수 앱이 처리 되지 않도록 하는 Azure 논리 앱을 만드는 한 가지 방법을 보여 줍니다.

Azure Logic Apps는 다른 서비스에 대 한 기본 제공 커넥터, 기능 상태 저장 오케스트레이션 및 회로 상태를 관리 하기 위한 자연 스러운 선택입니다. 회로를 중단 해야 하는 것을 감지한 후 다음 워크플로를 구현 하는 논리 앱을 빌드할 수 있습니다.

1. Event Grid 워크플로를 트리거하고 azure 함수 (Azure 리소스 커넥터 사용)를 중지 합니다.
1. 워크플로를 다시 시작 하는 옵션이 포함 된 알림 전자 메일 보내기

전자 메일 수신자는 회로의 상태를 조사 하 고, 해당 하는 경우 알림 전자 메일의 링크를 통해 회로를 다시 시작할 수 있습니다. 워크플로는 함수를 다시 시작할 때 마지막 이벤트 허브 검사점에서 메시지를 처리 합니다.

이 방법을 사용 하면 메시지가 손실 되지 않고, 모든 메시지가 순서 대로 처리 되며, 필요에 따라 회로를 중단할 수 있습니다.

## <a name="resources"></a>리소스

- [안정적인 이벤트 처리 샘플](https://github.com/jeffhollan/functions-csharp-eventhub-ordered-processing)
- [Azure 지 속성 엔터티 회로 차단기](https://github.com/jeffhollan/functions-durable-actor-circuitbreaker)

## <a name="next-steps"></a>다음 단계

자세한 내용은 다음 자료를 참조하세요.

- [Azure Functions 오류 처리](./functions-bindings-error-pages.md)
- [Event Grid를 사용하여 업로드된 이미지 크기 자동 조정](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json&tabs=dotnet)
- [Azure Logic Apps와 통합하는 함수 만들기](./functions-twitter-email.md)
