---
title: 데이터 추출-LUIS
description: 의도 및 엔터티를 사용 하 여 utterance 텍스트에서 데이터를 추출 합니다. Language Understanding에서 추출할 수 있는 데이터의 종류를 알아봅니다 (LUIS).
author: diberry
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 35f015691b15c6451a66509671c7dc2fc72f38e4
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682199"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>의도 및 엔터티를 사용하여 발화 텍스트에서 데이터 추출
LUIS는 사용자의 자연어 발화에서 정보를 가져오는 기능을 제공합니다. 정보는 프로그램, 애플리케이션 또는 챗봇에서 작업을 수행하는 데 사용할 수 있는 방법으로 추출됩니다. 다음 섹션에서는 JSON 예제와 함께 의도 및 엔터티에서 반환되는 데이터에 대해 알아봅니다.

추출 하는 데 가장 어려운 데이터는 정확히 일치 하는 텍스트가 아니므로 기계 학습 데이터입니다. 컴퓨터 학습 [엔터티의](luis-concept-entity-types.md) 데이터 추출은 원하는 데이터를 받을 때까지 [제작 주기의](luis-concept-app-iteration.md) 일부로 포함 되어야 합니다.

## <a name="data-location-and-key-usage"></a>데이터 위치 및 키 사용
LUIS는 게시 된 [끝점](luis-glossary.md#endpoint)에서 사용자의 utterance 데이터를 추출 합니다. **HTTPS 요청**(POST 또는 GET)에는 스테이징 또는 프로덕션 환경과 같은 일부 선택적 구성과 발화가 포함됩니다.

**V2 예측 엔드포인트 요청**

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

**V3 예측 엔드포인트 요청**

`https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/<appID>/slots/<slot-type>/predict?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&query=book 2 tickets to paris`

`appID`는 LUIS 앱을 편집하는 동안 URL의 일부(`/apps/` 뒤) 및 LUIS 앱의 **설정** 페이지에서 사용할 수 있습니다. `subscription-key`는 앱을 쿼리하는 데 사용되는 엔드포인트 키입니다. LUIS를 학습시키는 동안 체험 작성/시작 키를 사용할 수 있지만, 엔드포인트 키를 [필요한 LUIS 사용](luis-limits.md#key-limits)을 지원하는 키로 변경해야 합니다. `timezoneOffset` 단위는 분입니다.

**HTTPS 응답**에는 스테이징 또는 프로덕션 엔드포인트의 현재 게시된 모델을 기반으로 LUIS가 확인할 수 있는 모든 의도 및 엔터티 정보가 포함됩니다. 엔드포인트 URL은 [LUIS](luis-reference-regions.md) 웹 사이트의 **관리** 섹션에 있는 **키 및 엔드포인트** 페이지에서 찾을 수 있습니다.

## <a name="data-from-intents"></a>의도의 데이터
기본 데이터는 상위 점수 **의도 이름**입니다. 엔드포인트 응답은 다음과 같습니다.

#### <a name="v2-prediction-endpoint-response"></a>[V2 예측 엔드포인트 응답](#tab/V2)

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 예측 엔드포인트 응답](#tab/V3)

```JSON
{
  "query": "when do you open next?",
  "prediction": {
    "normalizedQuery": "when do you open next?",
    "topIntent": "GetStoreInfo",
    "intents": {
        "GetStoreInfo": {
            "score": 0.984749258
        }
    }
  },
  "entities": []
}
```

[V3 예측 엔드포인트](luis-migration-api-v3.md)에 대해 자세히 알아봅니다.

* * *

|데이터 개체|데이터 형식|데이터 위치|값|
|--|--|--|--|
|Intent|String|topScoringIntent.intent|“GetStoreInfo”|

두 개 이상의 의도 점수에 따라 의사 결정 봇 또는 LUIS 호출 앱이 결정 되 면 모든 의도의 점수를 반환 합니다.


#### <a name="v2-prediction-endpoint-response"></a>[V2 예측 엔드포인트 응답](#tab/V2)

Querystring 매개 변수를 설정 `verbose=true` 합니다. 엔드포인트 응답은 다음과 같습니다.

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 예측 엔드포인트 응답](#tab/V3)

Querystring 매개 변수를 설정 `show-all-intents=true` 합니다. 엔드포인트 응답은 다음과 같습니다.

```JSON
{
    "query": "when do you open next?",
    "prediction": {
        "normalizedQuery": "when do you open next?",
        "topIntent": "GetStoreInfo",
        "intents": {
            "GetStoreInfo": {
                "score": 0.984749258
            },
            "None": {
                 "score": 0.2040639
            }
        },
        "entities": {
        }
    }
}
```

[V3 예측 엔드포인트](luis-migration-api-v3.md)에 대해 자세히 알아봅니다.

* * *

의도는 최고 점수에서 최하 점수 순으로 정렬됩니다.

|데이터 개체|데이터 형식|데이터 위치|값|점수|
|--|--|--|--|:--|
|Intent|String|intents[0].intent|“GetStoreInfo”|0.984749258|
|Intent|String|intents[1].intent|“None”|0.0168218873|

미리 빌드된 도메인을 추가하는 경우, 의도 이름은 의도뿐 아니라 `Utilties` 또는 `Communication`와 같은 도메인을 나타냅니다.

#### <a name="v2-prediction-endpoint-response"></a>[V2 예측 엔드포인트 응답](#tab/V2)

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 예측 엔드포인트 응답](#tab/V3)

```JSON
{
    "query": "Turn on the lights next monday at 9am",
    "prediction": {
        "normalizedQuery": "Turn on the lights next monday at 9am",
        "topIntent": "Utilities.ShowNext",
        "intents": {
            "Utilities.ShowNext": {
                "score": 0.07842206
            },
            "Communication.StartOver": {
                "score": 0.0239675418
            },
            "None": {
                "score": 0.00085447653
            }
        },
        "entities": []
    }
}
```

[V3 예측 엔드포인트](luis-migration-api-v3.md)에 대해 자세히 알아봅니다.

* * *

|도메인|데이터 개체|데이터 형식|데이터 위치|값|
|--|--|--|--|--|
|유틸리티|Intent|String|intents[0].intent|“<b>Utilities</b>.ShowNext”|
|통신|Intent|String|intents[1].intent|<b>Communication</b>.StartOver”|
||Intent|String|intents[2].intent|“None”|


## <a name="data-from-entities"></a>엔터티의 데이터
대부분의 채팅 bot 및 응용 프로그램에는 의도 이름 보다 많은 내용이 필요 합니다. 이 추가적이고 선택적인 데이터는 발화에서 검색되는 엔터티에서 가져옵니다. 각 엔터티 형식은 서로 다른 일치 정보를 반환합니다.

발화의 단일 단어 또는 구문이 둘 이상의 엔터티와 일치할 수 있습니다. 이 경우, 각 일치하는 엔터티는 점수와 함께 반환됩니다.

모든 엔터티는 끝점에서 응답의 **엔터티** 배열에 반환 됩니다.

## <a name="tokenized-entity-returned"></a>토큰화된 엔터티가 반환됨

LUIS에서 [토큰 지원을](luis-language-support.md#tokenization) 검토 합니다.


## <a name="prebuilt-entity-data"></a>미리 빌드된 엔터티 데이터
[미리 빌드된](luis-concept-entity-types.md) 엔터티는 오픈 소스 [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) 프로젝트를 사용하여 정규식 일치를 기반으로 검색됩니다. 미리 빌드된 엔터티는 엔터티 배열로 반환되며 `builtin::` 접두사가 추가된 형식 이름을 사용합니다.

## <a name="list-entity-data"></a>목록 엔터티 데이터

[목록 엔터티](reference-entity-list.md) 는 동의어와 함께 고정 된 닫힌 관련 단어 집합을 나타냅니다. LUIS는 목록 엔터티에 대한 추가 값을 검색하지 않습니다. **권장** 기능을 사용하여 현재 목록을 기준으로 권장되는 새 단어를 확인합니다. 동일한 값을 갖는 목록 엔터티가 둘 이상 있는 경우, 각 엔터티가 엔드포인트 쿼리에서 반환됩니다.

## <a name="regular-expression-entity-data"></a>정규식 엔터티 데이터

[정규식 엔터티](reference-entity-regular-expression.md) 는 사용자가 제공 하는 정규식에 따라 엔터티를 추출 합니다.

## <a name="extracting-names"></a>이름 추출
이름은 거의 문자와 단어의 조합일 수 있기 때문에 발화에서 이름을 가져오는 것은 어렵습니다. 추출할 이름의 유형에 따라 몇 가지 옵션이 있습니다. 다음 제안 사항을 규칙이 아니라 자세한 지침입니다.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>미리 빌드된 PersonName 및 GeographyV2 엔터티 추가

[PersonName](luis-reference-prebuilt-person.md) 및 [GeographyV2](luis-reference-prebuilt-geographyV2.md) 엔터티는 일부 [언어 문화권](luis-reference-prebuilt-entities.md)에서 사용할 수 있습니다.

### <a name="names-of-people"></a>사용자의 이름

사용자의 이름에는 언어 및 문화권에 따라 일부 약한 형식이 적용될 수 있습니다. 미리 작성 된 **[personName](luis-reference-prebuilt-person.md)** 엔터티 또는 [역할](luis-concept-roles.md) 을 성과 이름으로 사용 하는 **[간단한 엔터티](luis-concept-entity-types.md#simple-entity)** 를 사용 합니다.

단순 엔터티를 사용 하는 경우 utterance의 서로 다른 부분에 있는 성과 이름을 사용 하는 예제를 제공 하 고, 다른 길이의 길이 발언를 사용 하 여 모든 의도에 대 한 길이 발언를 제공 해야 합니다. 정기적으로 엔드포인트 발화를 [검토](luis-how-to-review-endoint-utt.md)하여 올바르게 예측되지 않은 이름에 레이블을 지정합니다.

### <a name="names-of-places"></a>위치 이름

위치 이름은 도시, 군, 주,도 및 국가/지역과 같은 설정 및 알려져 있습니다. 미리 작성 된 엔터티 **[geographyV2](luis-reference-prebuilt-geographyv2.md)** 를 사용 하 여 위치 정보를 추출 합니다.

### <a name="new-and-emerging-names"></a>새롭게 떠오르는 이름

일부 앱은 제품이나 회사와 같이 새롭게 떠오르는 이름을 찾을 수 있어야 합니다. 이러한 형식의 이름은 가장 어려운 데이터 추출 유형입니다. **[간단한 엔터티로](luis-concept-entity-types.md#simple-entity)** 시작 하 고 [구 목록을](luis-concept-feature.md)추가 합니다. 정기적으로 엔드포인트 발화를 [검토](luis-how-to-review-endoint-utt.md)하여 올바르게 예측되지 않은 이름에 레이블을 지정합니다.

## <a name="patternany-entity-data"></a>Pattern.any 엔터티 데이터

[패턴. any](reference-entity-pattern-any.md) 는 패턴이 시작 되 고 끝나는 위치를 표시 하는 패턴의 템플릿 utterance에만 사용 되는 가변 길이 자리 표시자입니다. 패턴에 사용 되는 엔터티는 패턴을 적용 하기 위해 찾을 수 있어야 합니다.

## <a name="sentiment-analysis"></a>정서 분석
[게시](luis-how-to-publish-app.md#sentiment-analysis)하는 동안 감정 분석을 구성 하는 경우 LUIS json 응답은 감정 분석을 포함 합니다. [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) 설명서에서 감정 분석에 대해 자세히 알아봅니다.

## <a name="key-phrase-extraction-entity-data"></a>키 구문 추출 엔터티 데이터
[키 구 추출 엔터티](luis-reference-prebuilt-keyphrase.md) 는 [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/)에서 제공 하는 utterance의 키 구를 반환 합니다.

## <a name="data-matching-multiple-entities"></a>여러 엔터티와 일치하는 데이터

LUIS는 발화에서 검색된 모든 엔터티를 반환합니다. 따라서 채팅 봇이 결과에 따라 결정을 내려야 할 수 있습니다.

## <a name="data-matching-multiple-list-entities"></a>여러 목록 엔터티와 일치하는 데이터

단어나 구가 둘 이상의 목록 엔터티와 일치하는 경우, 엔드포인트 쿼리는 각 목록 엔터티를 반환합니다.

쿼리의 경우 `when is the best time to go to red rock?` 앱은 둘 이상의 목록에 단어를 포함 하 고 `red` , LUIS은 모든 엔터티를 인식 하 고 엔터티 배열을 JSON 끝점 응답의 일부로 반환 합니다.

## <a name="next-steps"></a>다음 단계

LUIS 앱에 엔터티를 추가하는 방법에 대한 자세한 내용은 [엔터티 추가](luis-how-to-add-entities.md)를 참조하세요.
