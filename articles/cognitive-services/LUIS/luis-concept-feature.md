---
title: 기능-LUIS
description: 언어 모델에 기능을 추가하여 레이블을 지정하거나 분류하려는 입력을 인식하는 방법에 대한 힌트를 제공합니다.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: fbf39382e418bef9a7d39886076a4100a26ce3e7
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362461"
---
# <a name="machine-learning-features"></a>기계 학습 기능

기계 학습에서 *기능은*   시스템이 관찰 하 고 학습 하는 데이터의 특징 또는 특성입니다.

기계 학습 기능을 통해 개념을 구분 하는 항목을 찾을 수 있는 위치에 대 한 중요 한 LUIS 수 있습니다. LUIS는 사용할 수 있지만 하드 규칙이 아닌 힌트입니다. LUIS는 레이블과 함께 이러한 힌트를 사용 하 여 데이터를 찾습니다.

기능은 f (x) = y와 같은 함수로 설명할 수 있습니다. 예제 utterance에서이 기능은 고유한 특성을 찾을 수 있는 위치를 알려 줍니다. 이 정보를 사용 하 여 스키마를 만들 수 있습니다.

## <a name="types-of-features"></a>기능 유형

LUIS는 구 목록과 모델을 모두 기능으로 지원 합니다.

* 구 목록 기능 
* 기능으로 서의 모델 (의도 또는 엔터티)

기능은 스키마 디자인의 필수 부분으로 간주 되어야 합니다.

## <a name="find-features-in-your-example-utterances"></a>예제 길이 발언의 기능 찾기

LUIS는 언어 기반 응용 프로그램 이므로 텍스트 기반 기능입니다. 구분 하려는 특성을 나타내는 텍스트를 선택 합니다. LUIS의 경우 가장 작은 단위는 *토큰*입니다. 영어의 경우 토큰은 공백이 나 문장 부호가 없는 연속적인 문자 및 숫자 범위입니다.

공백과 문장 부호가 토큰이 아니므로 기능으로 사용할 수 있는 텍스트 단서에 집중 합니다. 단어의 변형 (예:)을 포함 해야 합니다.

* 복수 양식
* 동사 시제
* 약어
* 철자 및 맞춤법 오류

특성을 구분 하기 때문에 텍스트가 다음과 같이 되어야 하는지 확인 합니다.

* 정확한 단어 또는 구와 일치: 정규식 엔터티 또는 목록 엔터티를 엔터티 또는 의도의 기능으로 추가 하는 것이 좋습니다.
* 날짜, 시간 또는 사람 이름과 같은 잘 알려진 개념 일치: 미리 작성 된 엔터티를 엔터티 또는 의도에 대 한 기능으로 사용 합니다.
* 시간에 따른 새로운 예제 배우기: 엔터티 또는 의도에 대 한 기능으로 서 개념의 일부 예에 대 한 문구 목록을 사용 합니다.

## <a name="combine-features"></a>기능 결합

여러 기능을 사용 하 여 특성 또는 개념을 설명할 수 있습니다. 일반적인 페어링은 구 목록 기능과 자주 사용 되는 엔터티 형식을 사용 하는 것입니다.

 * 미리 작성 한 엔터티
 * regular expression 엔터티
 * 목록 엔터티

### <a name="ticket-booking-entity-example"></a>티켓 예약 엔터티 예

첫 번째 예로 비행 예약 의도 및 티켓 예약 엔터티를 사용 하 여 비행을 예약 하는 앱을 고려 합니다.

티켓 예약 엔터티는 비행 대상에 대 한 기계 학습 엔터티입니다. 위치를 추출 하는 데 도움이 되도록 다음 두 가지 기능을 사용 합니다.

* , **평면**, **비행**, **예약**또는 **티켓과** 같은 관련 단어의 구 목록
* 엔터티에 대 한 기능으로 서 미리 작성 된 **geographyV2** 엔터티

### <a name="pizza-entity-example"></a>피자 엔터티 예제

또 다른 예로, 피자 주문 의도가 있는 피자와 피자 엔터티를 포함 하는 피자를 정렬 하는 앱을 생각해 보세요.

피자 엔터티는 피자 세부 정보에 대 한 기계 학습 엔터티입니다. 세부 정보를 추출 하기 위해 다음 두 가지 기능을 사용 하 여 도움을 줍니다.

* 관련 단어의 구 목록 (예: **치즈**, **crust**, **pepperoni**또는 **p apple** )
* 엔터티에 대 한 기능으로 서 미리 작성 된 **숫자** 엔터티입니다.

## <a name="create-a-phrase-list-for-a-concept"></a>개념에 대 한 구 목록 만들기

문구 목록은 개념을 설명 하는 단어 또는 구 목록입니다. 구문 목록은 토큰 수준에서 대/소문자를 구분 하지 않는 일치 항목으로 적용 됩니다.

구 목록을 추가할 때 기능을 **[전역](#global-features)** 으로 설정할 수 있습니다. 전역 기능은 전체 앱에 적용 됩니다.

### <a name="when-to-use-a-phrase-list"></a>구 목록을 사용 해야 하는 경우

LUIS 앱이 일반화 되 고 개념에 대 한 새 항목을 식별 해야 하는 경우 문구 목록을 사용 합니다. 구 목록은 도메인별 어휘와 비슷합니다. 이를 통해 의도 및 엔터티에 대 한 이해의 품질을 향상 시킬 수 있습니다.

### <a name="how-to-use-a-phrase-list"></a>구 목록을 사용 하는 방법

LUIS는 구 목록을 사용 하 여 컨텍스트와 일반화가와 유사 하지만 정확히 일치 하는 항목이 아닌 항목을 식별 하는 것으로 간주 합니다. 다음 단계를 수행 하 여 구 목록을 사용 합니다.

1. Machine learning 엔터티를 사용 하 여 시작 합니다.
    1. 예 길이 발언를 추가 합니다.
    1. 기계 학습 엔터티가 포함 된 레이블입니다.
1. 구 목록 추가:
    1. 비슷한 의미로 단어를 추가 합니다. 가능한 단어나 구를 모두 추가 하지 마세요. 대신, 한 번에 몇 개의 단어나 구를 추가 합니다. 그런 다음 다시 학습 및 publish를 클릭 합니다.
    1. 제안 된 단어를 검토 하 고 추가 합니다.

### <a name="a-typical-scenario-for-a-phrase-list"></a>구 목록에 대 한 일반적인 시나리오

구 목록에서 일반적인 시나리오는 특정 아이디어와 관련 된 단어를 높이는 것입니다.

의료 용어는 의미를 높이기 위해 문구 목록이 필요할 수 있는 좋은 단어의 예입니다. 이러한 용어에는 특정 물리적, 화학, therapeutic 또는 추상 의미가 있을 수 있습니다. LUIS는 용어 목록 없이 주체 도메인에 중요 한 용어를 알지 못합니다.

의료 약관을 추출 하려면:

1. 이러한 길이 발언 내에 예 길이 발언 및 레이블 의료 용어를 만듭니다.
2. 주체 도메인 내에 약관의 예가 포함 된 구 목록을 만듭니다. 이 문구 목록에는 레이블이 지정 된 실제 용어와 동일한 개념을 설명 하는 다른 용어가 포함 되어야 합니다.
3. 구 목록에 사용 되는 개념을 추출 하는 엔터티 또는 하위 엔터티에 문구 목록을 추가 합니다. 가장 일반적인 시나리오는 기계 학습 엔터티의 구성 요소 (자식)입니다. 모든 의도 또는 엔터티에 대해 구 목록을 적용 해야 하는 경우에는 구 목록을 전역 문구 목록으로 표시 합니다. **EnabledForAllModels** 플래그는 API의이 모델 범위를 제어 합니다.

### <a name="token-matches-for-a-phrase-list"></a>구 목록과 일치 하는 토큰

구 목록은 항상 토큰 수준에서 적용 됩니다. 다음 표에서는 **Ann** 단어가 포함 된 구 목록이 해당 순서 대로 동일한 문자의 변형에 적용 되는 방식을 보여 줍니다.


| **Ann** 의 토큰 변형 | 토큰을 찾은 경우 구 목록 일치 |
|--------------------------|---------------------------------------|
| **ANN**<br>**aNN**<br>           | 예-토큰은 **Ann** 입니다.                  |
| **Ann**                    | 예-토큰은 **Ann** 입니다.                  |
| **Anne**                     | 아니요-토큰은 **유가을** 입니다.                  |

<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>모델을 기능으로 사용 하면 다른 모델

모델 (의도 또는 엔터티)을 다른 모델 (의도 또는 엔터티)에 기능으로 추가할 수 있습니다. 기존 의도 또는 엔터티를 기능으로 추가 하 여 레이블이 지정 된 예제를 포함 하는 잘 정의 된 개념을 추가 하 게 됩니다.

모델을 기능으로 추가 하는 경우 기능을 다음과 같이 설정할 수 있습니다.
* **[필수 항목](#required-features)** 입니다. 모델을 예측 끝점에서 반환 하려면 필수 기능을 찾을 수 있어야 합니다.
* **[전역](#global-features)**. 전역 기능은 전체 앱에 적용 됩니다.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>엔터티를 의도에 대 한 기능으로 사용 하는 경우

해당 엔터티의 검색은 의도에 중요 한 경우 엔터티를 기능으로 추가 합니다.

예를 들어 **Bookflight**와 같이 비행을 예약 하기 위한 의도가 고 엔터티가 티켓 정보 (예: 좌석, 원본 및 대상 수) 인 경우 티켓 정보 엔터티를 찾는 것이 **bookflight** 의도의 예측에 상당한 가중치를 추가 해야 합니다.

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>엔터티를 다른 엔터티의 기능으로 사용 하는 경우

엔터티 (a)의 검색이 엔터티 (B)의 예측에 중요 한 경우 엔터티 (A)를 다른 엔터티 (B)에 기능으로 추가 해야 합니다.

예를 들어 배송 주소 엔터티가 주소 하위 엔터티에 포함 된 경우 주소 하위 엔터티를 찾으면 배송 주소 엔터티에 대 한 예측에 상당한 가중치를 추가 합니다.

* 배송 주소 (기계 학습 엔터티):

    * 번 지 수 (subentity)
    * 주소 (subentity)
    * City (subentity)
    * 시/도 (하위 엔터티)
    * 국가/지역 (하위 엔터티)
    * 우편 번호 (subentity)

## <a name="nested-subentities-with-features"></a>기능이 포함 된 중첩 된 하위 엔터티

기계 학습 하위 엔터티는 부모 엔터티에 대 한 개념이 있음을 나타냅니다. 부모는 다른 하위 엔터티 또는 최상위 엔터티입니다. 대상의 값은 부모에 대 한 기능 역할을 합니다.

하위 엔터티에는 구 목록과 모델 (다른 엔터티)이 모두 기능으로 포함 될 수 있습니다.

하위 엔터티에 구 목록이 있으면 개념의 어휘를 증폭 하지만 예측의 JSON 응답에 어떠한 정보도 추가 하지 않습니다.

하위 엔터티에 다른 엔터티의 기능이 있는 경우 JSON 응답은 해당 하는 다른 엔터티의 추출 된 데이터를 포함 합니다.


## <a name="required-features"></a>필요한 기능

모델을 예측 끝점에서 반환 하려면 필수 기능을 찾을 수 있어야 합니다. 들어오는 데이터가 기능에 일치 해야 한다는 것을 알고 있는 경우 필요한 기능을 사용 합니다.

Utterance 텍스트가 필수 기능과 일치 하지 않으면 추출 되지 않습니다.

필수 기능은 비 기계어 학습 엔터티를 사용 합니다.

* 정규식 엔터티
* 목록 엔터티
* 미리 빌드된 엔터티

모델을 데이터에서 찾을 수 있다고 확신 하는 경우 필요에 따라 기능을 설정 합니다. 필요한 기능을 찾을 수 없는 경우 아무것도 반환 하지 않습니다.

배송 주소의 예를 계속 합니다.

배송 주소 (컴퓨터에서 배운 엔터티)

 * 번 지 수 (subentity) 
 * 주소 (subentity) 
 * 주소 이름 (하위 엔터티) 
 * City (subentity) 
 * 시/도 (하위 엔터티) 
 * 국가/지역 (하위 엔터티) 
 * 우편 번호 (subentity)

### <a name="required-feature-using-prebuilt-entities"></a>미리 작성 한 엔터티를 사용 하는 필수 기능

도시, 시/도 및 국가/지역은 일반적으로 닫힌 목록 집합입니다. 즉, 시간이 지남에 따라 크게 변경 되지 않습니다. 이러한 엔터티에는 관련 권장 기능이 있을 수 있으며 이러한 기능은 필수로 표시 될 수 있습니다. 즉, 필요한 기능이 있는 엔터티를 찾을 수 없는 경우 전체 배송 주소가 반환 되지 않습니다.

구/군/시, 시/도 및 국가/지역이 utterance에 있는 경우에는 어떻게 되나요?은 어에는 LUIS가 예상치 못한 것입니다. LUIS의 낮은 신뢰도 점수 때문에 엔터티를 해결 하는 데 도움이 되는 post 처리를 제공 하려는 경우 필요에 따라 기능을 표시 하지 마세요.

배송 주소에 대 한 필수 기능의 또 다른 예는 주소를 [미리](luis-reference-prebuilt-entities.md) 작성 된 필수 숫자로 지정 하는 것입니다. 이를 통해 사용자는 "1 개의 microsoft 방법" 또는 "한 가지 Microsoft 방법"을 입력할 수 있습니다. 두 항목 모두에 대 한 숫자 "1"을 확인 합니다.

### <a name="required-feature-using-list-entities"></a>목록 엔터티를 사용 하는 필수 기능

[목록 엔터티](reference-entity-list.md) 는 동의어와 함께 정식 이름 목록으로 사용 됩니다. 필수 기능으로, utterance에 정식 이름 또는 동의어가 포함 되지 않은 경우 엔터티는 예측 끝점의 일부로 반환 되지 않습니다.

회사에서 제한 된 국가/지역 집합만 제공 한다고 가정 합니다. 고객이 국가/지역을 참조할 수 있는 여러 가지 방법을 포함 하는 목록 엔터티를 만들 수 있습니다. LUIS가 utterance 텍스트 내에서 정확히 일치 하는 항목을 찾지 못하면 목록 엔터티의 필수 기능이 있는 엔터티는 예측에서 반환 되지 않습니다.

|정식 이름|동의어|
|--|--|
|미국|미국<br>U. S .A<br>US<br>USA<br>0|

채팅 봇과 같은 클라이언트 응용 프로그램은 추가 작업에 도움을 요청할 수 있습니다. 이를 통해 고객은 국가/지역 선택이 제한 되어 *필요한*지를 이해할 수 있습니다.

### <a name="required-feature-using-regular-expression-entities"></a>정규식 엔터티를 사용 하는 필수 기능

필수 기능으로 사용 되는 [정규식 엔터티](reference-entity-regular-expression.md) 는 다양 한 텍스트 일치 기능을 제공 합니다.

배송 주소 예제에서는 국가/지역 우편 번호의 구문 규칙을 캡처하는 정규식을 만들 수 있습니다.

## <a name="global-features"></a>전역 기능

가장 일반적으로 사용 되는 기능은 특정 모델에 기능을 적용 하는 것 이지만 전체 응용 프로그램에 적용 하는 기능을 **전역 기능** 으로 구성할 수 있습니다.

전역 기능을 사용 하는 가장 일반적인 방법은 앱에 어휘를 더 추가 하는 것입니다. 예를 들어 고객이 주 언어를 사용 하지만 동일한 utterance 내에서 다른 언어를 사용할 수 있는 경우 보조 언어의 단어를 포함 하는 기능을 추가할 수 있습니다.

사용자가 의도 또는 엔터티에 대해 보조 언어를 사용 하는 것으로 예상 되므로 보조 언어의 단어를 구 목록에 추가 합니다. 구 목록을 전역 기능으로 구성 합니다.

## <a name="best-practices"></a>모범 사례

[모범 사례](luis-concept-best-practices.md)를 알아봅니다.

## <a name="next-steps"></a>다음 단계

* 예측 런타임에서 앱 모델을 [확장](schema-change-prediction-runtime.md) 합니다.
* LUIS 앱에 기능을 추가 하는 방법에 대해 자세히 알아보려면 [기능 추가](luis-how-to-add-features.md) 를 참조 하세요.
