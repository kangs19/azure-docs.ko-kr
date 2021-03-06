---
title: WhiteNoise 패키지를 사용하여 차등 프라이버시 구현
titleSuffix: Azure Machine Learning
description: 차등 프라이버시가 무엇인지 알아보고, WhiteNoise 패키지를 사용하여 데이터 프라이버시를 보호하는 차등 비공개 시스템을 구현하는 방법을 알아봅니다.
author: luisquintanilla
ms.author: luquinta
ms.date: 05/03/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.openlocfilehash: aa4fe715c18e582448ee7f642a6a75947356ab61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84982665"
---
# <a name="preserve-data-privacy-by-using-differential-privacy-and-the-whitenoise-package"></a>WhiteNoise 패키지와 차등 프라이버시를 사용하여 데이터 프라이버시 보호

차등 프라이버시가 무엇인지 알아보고, WhiteNoise 패키지가 차등 비공개 시스템을 구현하는 데 어떻게 유용한지 알아봅니다.

조직에서 수집하여 분석에 사용하는 데이터의 양이 증가하면서 개인 정보 보호와 보안에 대한 우려도 증가하고 있습니다. 분석에는 데이터가 필요합니다. 일반적으로 모델을 학습하는 데 사용하는 데이터가 많을 수록 정확성이 높아집니다. 이러한 분석에 개인 정보를 사용하는 경우에는 데이터를 사용하는 내내 비공개 상태를 유지하는 것이 특히 중요합니다.

> [!NOTE]
> 도구 키트의 이름을 바꾸고 새 이름을 소개 하는 주입니다. 

## <a name="how-differential-privacy-works"></a>차등 프라이버시 작동 원리

차등 프라이버시는 개인의 데이터를 비공개로 안전하게 보호하는 데 유용한 일련의 시스템과 방법입니다.

> [!div class="mx-imgBorder"]
> ![차등 프라이버시 프로세스](./media/concept-differential-privacy/differential-privacy-process.jpg)

기존 시나리오에서는 원시 데이터가 파일 및 데이터베이스에 저장됩니다. 사용자가 데이터를 분석할 때는 일반적으로 원시 데이터를 사용합니다. 이는 개인의 프라이버시를 침해할 수 있기 때문에 문제가 됩니다. 차등 프라이버시는 사용자가 개별 데이터 요소를 식별할 수 없도록 데이터에 "노이즈" 또는 임의성을 추가하여 이 문제를 해결하려고 합니다. 적어도 이러한 시스템은 타당한 거부를 제공합니다.

차등 비공개 시스템 데이터는 **쿼리**라는 요청을 통해 공유됩니다. 사용자가 데이터에 대한 쿼리를 제출하면 **프라이버시 메커니즘**이라고 알려진 작업이 요청된 데이터에 노이즈를 추가합니다. 프라이버시 메커니즘은 원시 데이터 대신 *데이터의 근사값*을 반환합니다. 이러한 프라이버시 보호 결과는 **보고서**에 나타납니다. 보고서는 두 가지 부분 즉, 컴퓨팅한 실제 데이터와 데이터가 생성된 방식에 대한 설명으로 구성됩니다.

## <a name="differential-privacy-metrics"></a>차등 프라이버시 메트릭

차등 프라이버시는 사용자가 보고서를 무제한 생성하여 실수로 중요한 데이터가 공개되는 가능성을 방지하려고 합니다. **엡실론**이라고 알려진 값은 보고서의 노이즈 또는 비공개 정도를 측정합니다. 엡실론은 노이즈나 프라이버시와 반비례 관계가 있습니다. 엡실론이 낮을수록 데이터의 노이즈(및 비공개) 정도가 높습니다.

엡실론 값은 음수가 아닙니다. 1 미만의 값은 완전 타당한 거부를 제공합니다. 1을 초과하는 값이 높을수록 실제 데이터가 노출될 위험이 높습니다. 차등 비공개 시스템을 구현할 때는 엡실론 값이 0과 1 사이인 보고서를 생성하는 것이 좋습니다.

엡실론과 직접 상관 관계가 있는 또 다른 값은 **델타**입니다. 델타는 보고서가 완전 비공개가 아닐 확률의 척도입니다. 델타가 높을수록 엡실론이 높습니다. 두 값은 상관 관계가 있기 때문에 엡실론이 더 자주 사용됩니다.

## <a name="privacy-budget"></a>프라이버시 예산

다수의 쿼리가 허용되는 시스템에서 프라이버시를 보장하기 위해 차등 프라이버시는 속도 제한을 정의합니다. 이러한 제한을 **프라이버시 예산**이라고 합니다. 재식별화(reidentification) 위험을 제한하기 위해, 프라이버시 예산에 대개 1~3 사이의 엡실론 금액이 할당됩니다. 프라이버시 예산은 보고서가 생성될 때 개별 보고서의 엡실론 값뿐만 아니라 모든 보고서에 대한 집계를 추적합니다. 프라이버시 예산이 소진되거나 격감하면 사용자는 데이터에 더 이상 액세스할 수 없습니다.  

## <a name="reliability-of-data"></a>데이터의 신뢰성

프라이버시 보호가 목표이기는 하지만 데이터의 유용성과 신뢰성에는 상충 관계가 있습니다. 데이터 분석에서 정확도는 샘플링 오차로 인해 야기된 불확실성의 척도라고 생각할 수 있습니다. 불확실성은 특정 범위에 속하는 경향이 있습니다. 차등 프라이버시 측면에서 **정확도**는 데이터의 신뢰성을 측정하며, 이것은 프라이버시 메커니즘에 의해 야기된 불확실성에 영향을 받습니다. 즉, 노이즈나 프라이버시 수준이 높을수록 엡실론, 정확도 및 신뢰성이 낮은 데이터로 변환됩니다. 데이터의 비공개 정도가 높아도 신뢰할 수 없기 때문에 사용될 가능성이 낮습니다.

## <a name="implementing-differentially-private-systems"></a>차등 비공개 시스템 구현

차등 비공개 시스템을 구현하기는 어렵습니다. WhiteNoise는 글로벌 차등 비공개 시스템을 구축하는 다양한 구성 요소가 포함된 오픈 소스 프로젝트입니다. WhiteNoise는 다음과 같은 최상위 구성 요소로 이루어집니다.

- 코어
- 시스템

### <a name="core"></a>핵심

핵심 라이브러리에는 차등 비공개 시스템을 구현하기 위해 다음과 같은 프라이버시 메커니즘이 포함되어 있습니다.

|구성 요소  |설명  |
|---------|---------|
|분석     | 임의 계산에 대한 그래프 설명입니다. |
|유효성 검사기     | 분석에 차등 비공개를 적용하는 데 필요한 조건을 확인하고 도출하기 위한 도구 세트가 포함된 Rust 라이브러리입니다.          |
|런타임     | 분석을 실행할 수단입니다. 참조 런타임은 Rust로 작성되지만 데이터 요구에 따라 SQL 및 Spark와 같은 계산 프레임워크를 사용하여 런타임을 작성할 수 있습니다.        |
|바인딩     | 분석을 빌드할 언어 바인딩 및 도우미 라이브러리입니다. 현재 WhiteNoise는 Python 바인딩을 제공합니다. |

### <a name="system"></a>시스템

시스템 라이브러리는 테이블 형식 및 관계형 데이터 작업을 위해 다음과 같은 도구와 서비스를 제공합니다.

|구성 요소  |Description  |
|---------|---------|
|데이터 액세스     | SQL 쿼리를 가로채서 처리하여 보고서를 생성하는 라이브러리입니다. 이 라이브러리는 Python을 사용하여 구현되며 다음과 같은 ODBC 및 DBAPI 데이터 소스를 지원합니다.<ul><li>PostgreSQL</li><li>SQL Server</li><li>Spark</li><li>Preston</li><li>Pandas</li></ul>|
|서비스     | 공유 데이터 소스에 대한 요청이나 쿼리를 처리하는 REST 엔드포인트를 제공하는 실행 서비스입니다. 이 서비스는 서로 다른 델타 및 엡실론 값이 포함된 요청(이기종 요청이라고도 함)에서 작동하는 차등 프라이버시 모듈을 구성할 수 있도록 설계되었습니다. 이 참조 구현은 상호 관련된 데이터에 대한 쿼리가 추가로 미치는 영향을 처리합니다. |
|평가기     | 프라이버시 침해, 정확도 및 편향을 확인하는 확률적 평가기입니다. 평가기는 다음 테스트를 지원합니다. <ul><li>프라이버시 테스트 - 보고서가 차등 프라이버시 조건을 준수하는지 여부를 확인합니다.</li><li>정확도 테스트 - 보고서의 신뢰성이 95% 신뢰 수준에서 상한 및 하한 내에 있는지 여부를 측정합니다.</li><li>유틸리티 테스트 - 프라이버시를 최대화하면서 보고서의 신뢰도 범위가 데이터에 충분히 가까운지 확인합니다.</li><li>편향 테스트 - 반복 쿼리에 대한 보고서 분포를 측정하여 불균형이 없도록 합니다.</li></ul> |

## <a name="next-steps"></a>다음 단계

Azure Machine Learning에서 [데이터 개인 정보를 유지](how-to-differential-privacy.md) 합니다.

WhiteNoise의 구성 요소에 대해 자세히 알아보려면 [WhiteNoise Core 패키지](https://github.com/opendifferentialprivacy/whitenoise-core), [WhiteNoise System package](https://github.com/opendifferentialprivacy/whitenoise-system) 및 [WhiteNoise 샘플](https://github.com/opendifferentialprivacy/whitenoise-samples)에 대 한 GitHub 리포지토리를 참조 하세요.