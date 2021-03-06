---
title: AutoML 실험의 기능화
titleSuffix: Azure Machine Learning
description: 기능화 설정 Azure Machine Learning 제품 및 기능 엔지니어링이 자동화 된 ML 실험에서 지원 되는 방법에 대해 알아봅니다.
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 05/28/2020
ms.custom: seodec18
ms.openlocfilehash: aa348728cd4e9ac0ce5d70cb293ac850cc549666
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84817124"
---
# <a name="featurization-in-automated-machine-learning"></a>자동화 된 기계 학습의 기능화

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

이 가이드에서는 다음에 대해 알아봅니다.

- 기능화 설정 Azure Machine Learning 제공 합니다.
- [자동화 된 machine learning 실험](concept-automated-ml.md)을 위해 이러한 기능을 사용자 지정 하는 방법입니다.

*기능 엔지니어링* 은 데이터에 대 한 도메인 정보를 사용 하 여 기계 학습 (ML) 알고리즘을 통해 더 나은 학습을 돕는 기능을 만드는 프로세스입니다. Azure Machine Learning에서 데이터 크기 조정 및 정규화 기술이 기능 엔지니어링을 용이 하 게 하기 위해 적용 됩니다. 이러한 기술과이 기능 엔지니어링은 자동화 된 기계 학습 또는 *Automl*, 실험에서 *기능화* 이라고 통칭 됩니다.

이 문서에서는 AutoML 실험을 구성 하는 방법을 이미 알고 있다고 가정 합니다. 구성에 대 한 자세한 내용은 다음 문서를 참조 하세요.

- 코드 우선 환경의 경우: [Python 용 AZURE MACHINE LEARNING SDK를 사용 하 여 자동화 된 ML 실험을 구성](how-to-configure-auto-train.md)합니다.
- 코드 또는 비 코드 환경의 경우: [Azure Machine Learning studio를 사용 하 여 자동화 된 기계 학습 모델을 만들고 검토 하 고 배포](how-to-use-automated-ml-for-ml-models.md)합니다.

## <a name="configure-featurization"></a>기능화 구성

자동화 된 모든 기계 학습 실험에서 [자동 크기 조정 및 정규화 기술이](#featurization) 기본적으로 데이터에 적용 됩니다. 이러한 기술은 다양 한 규모의 기능에 중요 한 *특정* 알고리즘을 지 원하는 기능화의 유형입니다. 그러나 *누락 값 대체*, *인코딩*및 *변환과*같은 추가 기능화를 사용 하도록 설정할 수도 있습니다.

> [!NOTE]
> 자동화 된 machine learning 기능화 (예: 기능 정규화, 누락 된 데이터 처리 또는 텍스트를 숫자로 변환)에 대 한 단계는 기본 모델의 일부가 됩니다. 예측에 모델을 사용 하는 경우 학습 중에 적용 되는 것과 동일한 기능화 단계가 입력 데이터에 자동으로 적용 됩니다.

Python SDK를 사용 하 여 구성 하는 실험의 경우 기능화 설정을 사용 하거나 사용 하지 않도록 설정 하 고 실험에 사용할 기능화 단계를 추가로 지정할 수 있습니다. Azure Machine Learning studio를 사용 하는 경우 [기능화를 사용 하도록 설정 하는 단계](how-to-use-automated-ml-for-ml-models.md#customize-featurization)를 참조 하세요.

다음 표에서는 `featurization` [AutoMLConfig 클래스](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)에서에 대해 허용 되는 설정을 보여 줍니다.

|기능화 구성 | 설명|
------------- | ------------- |
|`"featurization": 'auto'`| 전처리의 일부로 [데이터 guardrails 및 기능화 단계](#featurization) 를 자동으로 수행 하도록 지정 합니다. 이 설정은 기본값입니다.|
|`"featurization": 'off'`| 기능화 단계가 자동으로 수행 되지 않도록 지정 합니다.|
|`"featurization":`&nbsp;`'FeaturizationConfig'`| 사용자 지정 된 기능화 단계를 사용 하도록 지정 합니다. [기능화를 사용자 지정하는 방법을 알아보세요](#customize-featurization).|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>자동 기능화

다음 표에는 데이터에 자동으로 적용 되는 기술이 요약 되어 있습니다. 이러한 기술은 SDK 또는 studio를 사용 하 여 구성 된 실험에 적용 됩니다. 이 동작을 사용 하지 않도록 설정 하려면 `"featurization": 'off'` 개체에서을 설정 `AutoMLConfig` 합니다.

> [!NOTE]
> AutoML에서 만든 모델을 [onnx 모델로](concept-onnx.md)내보낼 계획인 경우 별표 ("*")로 표시 된 기능화 옵션만 onnx 형식으로 지원 됩니다. [모델을 ONNX로 변환](concept-automated-ml.md#use-with-onnx)하는 방법에 대해 자세히 알아보세요.

|기능화 &nbsp; 단계| 설명 |
| ------------- | ------------- |
|**상위 카디널리티를 삭제 하거나 가변성 기능을 제거 합니다.*** |이러한 기능을 학습 및 유효성 검사 집합에서 삭제 합니다. 모든 행에서 동일한 값을 가진 모든 값이 누락 된 기능 또는 높은 카디널리티 (예: 해시, Id 또는 Guid)를 가진 기능에 적용 됩니다.|
|**누락 값 돌립니다*** |숫자 기능의 경우 돌립니다는 열에 있는 값의 평균을 사용 합니다.<br/><br/>범주 기능의 경우 가장 자주 사용 되는 값을 돌립니다 합니다.|
|**추가 기능 생성*** |DateTime 기능: 연도, 월, 일, 요일, 연간 일자, 분기, 연간 주, 시간, 분, 초<br/><br/>텍스트 기능의 경우: 선 그램, bigrams 및 trigrams을 기반으로 하는 용어 빈도.|
|**변환 및 인코딩***|고유 값이 적은 숫자 기능을 범주 기능으로 변환 합니다.<br/><br/>단일 핫 인코딩은 낮은 카디널리티 범주 기능에 사용 됩니다. 단일 핫 해시 인코딩은 고급 카디널리티 범주 기능에 사용 됩니다.|
|**단어 포함**|텍스트 featurizer 사전 학습 된 모델을 사용 하 여 텍스트 토큰의 벡터를 문장 벡터로 변환 합니다. 문서에 있는 각 단어의 포함 벡터는 문서 기능 벡터를 생성 하기 위해 나머지와 함께 집계 됩니다.|
|**대상 인코딩**|범주 기능의 경우이 단계는 회귀 문제에 대 한 평균 목표 값과 분류 문제에 대 한 각 클래스의 확률에 각 범주를 매핑합니다. 빈도 기반 가중치 및 k 접기 교차 유효성 검사를 적용 하 여 스파스 데이터 범주로 인 한 매핑의 과잉 맞춤을 줄입니다.|
|**텍스트 대상 인코딩**|텍스트 입력의 경우 단어 모음이 포함된 누적 선형 모델을 사용하여 각 클래스의 확률을 생성합니다.|
|**WoE(증명 정보 가중치)**|WoE를 대상 열에 대한 범주별 열의 상관 관계 측정값으로 계산합니다. WoE는 클래스 내 및 클래스 외부 확률의 비율로 계산 됩니다. 이 단계에서는 클래스 당 하나의 숫자 기능 열을 생성 하 고 누락 된 값 및 이상 값 처리를 명시적으로 돌립니다 필요가 없습니다.|
|**클러스터 거리**|K를 트레인 하 여 모든 숫자 열에 대 한 클러스터링 모델을 의미 합니다. 각 클러스터의 중심에 대 한 각 샘플의 거리가 포함 된 *k* 개의 새로운 기능 (클러스터당 하나의 새 숫자 기능)을 생성 합니다.|

## <a name="data-guardrails"></a>데이터 가드 레일

*데이터 guardrails* 를 사용 하면 데이터의 잠재적 문제 (예: 누락 값 또는 [클래스 불균형](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data))를 식별할 수 있습니다. 또한 향상 된 결과에 대 한 정정 작업을 수행 하는 데 도움이 됩니다.

데이터 guardrails 적용 됩니다.

- **SDK 실험**: `"featurization": 'auto'` 개체에 또는 매개 변수가 `validation=auto` 지정 된 경우 `AutoMLConfig`
- **Studio 실험**: 자동 기능화을 사용 하는 경우.

실험에 대 한 데이터 guardrails을 검토할 수 있습니다.

- `show_output=True`SDK를 사용 하 여 실험을 제출할 때를 설정 합니다.

- Studio의 자동화 된 ML 실행의 **데이터 guardrails** 탭에서

### <a name="data-guardrail-states"></a>데이터 guardrail 상태

데이터 guardrails 세 가지 상태 중 하나를 표시 합니다.

|시스템 상태| 설명 |
|----|---- |
|**통과**| 데이터 문제가 검색 되지 않았으므로 사용자에 게 아무런 조치도 필요 하지 않습니다. |
|**완료**| 변경 내용이 데이터에 적용되었습니다. 변경 내용이 예상 된 결과와 일치 하는지 확인 하기 위해 AutoML에서 수행한 정정 작업을 검토 하는 것이 좋습니다. |
|**알림**| 데이터 문제가 검색 되었지만 해결할 수 없습니다. 문제를 수정 하 고 수정 하는 것이 좋습니다.|

### <a name="supported-data-guardrails"></a>지원 되는 데이터 guardrails

다음 표에서는 현재 지원 되는 데이터 guardrails 실험을 제출할 때 볼 수 있는 연결 된 상태에 대해 설명 합니다.

가드 레일|상태|트리거 조건
---|---|---
**누락된 기능 값 대체** |통과 <br><br><br> 완료| 학습 데이터에서 누락된 기능 값이 검색되지 않았습니다. [누락 값 대체](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) 에 대해 자세히 알아보세요. <br><br> 학습 데이터에서 누락 된 기능 값이 발견 되어 귀속 되었습니다.
**높은 카디널리티 기능 처리** |통과 <br><br><br> 완료| 입력이 분석 되었으며, 높은 카디널리티 기능이 검색 되지 않았습니다. [높은 카디널리티 기능 검색](#automatic-featurization)에 대해 자세히 알아보세요. <br><br> 사용자 입력에서 높은 카디널리티 기능이 검색 되었으며 처리 되었습니다.
**유효성 검사 분할 처리** |완료| 유효성 검사 구성이로 설정 되었고 `'auto'` 학습 데이터에 *2만 개 미만의 행*이 포함 되어 있습니다. <br> 교차 유효성 검사를 사용 하 여 학습 된 모델의 각 반복의 유효성을 검사 했습니다. [유효성 검사 데이터](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data)에 대해 자세히 알아보세요. <br><br> 유효성 검사 구성이로 설정 되었으며 `'auto'` 학습 데이터에 *2만 개 이상의 행*이 포함 되어 있습니다. <br> 모델의 유효성 검사를 위해 입력 데이터가 학습 데이터 세트와 유효성 검사 데이터 세트로 분할되었습니다.
**클래스 분산 검색** |통과 <br><br><br><br><br> 알림 | 입력이 분석되었으며, 학습 데이터에서 모든 클래스가 균형을 유지하고 있습니다. 데이터 집합에는 샘플의 수와 비율을 기준으로 하 여 데이터 집합에 적절 한 표현이 있는 경우 데이터 집합의 균형을 유지 하는 것으로 간주 됩니다. <br><br><br> 입력에서 불균형 클래스가 검색되었습니다. 모델 바이어스를 해결 하려면 분산 문제를 해결 하십시오. [불균형 있는 데이터](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)에 대해 자세히 알아보세요.
**메모리 문제 검색** |통과 <br><br><br><br> 완료 |<br> 선택한 값 (가로, 지연, 롤링 창)이 분석 되었으며 메모리 부족 문제가 감지 되었습니다. 시계열 [예측 구성](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment)에 대해 자세히 알아보세요. <br><br><br>선택한 값 (가로, 지연, 롤링 창)이 분석 되어 실험의 메모리가 부족 해질 수 있습니다. 지연 또는 창 롤링 구성이 해제 되었습니다.
**빈도 검색** |통과 <br><br><br><br> 완료 |<br> 시계열이 분석 되 고 모든 데이터 요소가 검색 된 빈도로 정렬 됩니다. <br> <br> 시계열이 분석 되었으며 검색 된 빈도로 일치 하지 않는 데이터 요소가 검색 되었습니다. 데이터 세트에서 이러한 데이터 요소가 제거되었습니다. 시계열 [예측에 대 한 데이터 준비](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data)에 대해 자세히 알아보세요.

## <a name="customize-featurization"></a>기능화 사용자 지정

기능화 설정을 사용자 지정 하 여 ML 모델을 학습 하는 데 사용 되는 데이터와 기능이 관련 된 예측을 얻을 수 있도록 할 수 있습니다.

Featurizations를 사용자 지정 하려면  `"featurization": FeaturizationConfig` 개체에를 지정 `AutoMLConfig` 합니다. 실험을 위해 Azure Machine Learning studio를 사용 하는 경우 [방법 문서](how-to-use-automated-ml-for-ml-models.md#customize-featurization)를 참조 하세요.

지원되는 사용자 지정은 다음과 같습니다.

|사용자 지정|정의|
|--|--|
|**열 용도 업데이트**|지정 된 열에 대 한 기능 유형을 재정의 합니다.|
|**변환기 매개 변수 업데이트** |지정 된 변환기에 대 한 매개 변수를 업데이트 합니다. 현재는 *(평균* , 가장 자주, 중앙값) 및 *HashOneHotEncoder*을 지원 합니다.|
|**삭제 열** |기능화에서 삭제할 열을 지정 합니다.|
|**블록 변환기**| 기능화 프로세스에 사용할 블록 변환기를 지정 합니다.|

`FeaturizationConfig`API 호출을 사용 하 여 개체를 만듭니다.

```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

## <a name="next-steps"></a>다음 단계

* 자동화 된 ML 실험을 설정 하는 방법에 대해 알아봅니다.

    * 코드 우선 환경의 경우: [AZURE MACHINE LEARNING SDK를 사용 하 여 자동화 된 ML 실험을 구성](how-to-configure-auto-train.md)합니다.
    * 코드 또는 비 코드 환경의 경우: [Azure Machine Learning studio에서 자동화 된 ML 실험을 만듭니다](how-to-use-automated-ml-for-ml-models.md).

* [모델 배포 방법 및 위치](how-to-deploy-and-where.md)에 대해 자세히 알아봅니다.

* [자동화 된 machine learning을 사용 하 여 회귀 모델을 학습 하는 방법](tutorial-auto-train-models.md) 또는 [원격 리소스에서 자동화 된 machine learning을 사용 하 여 학습](how-to-auto-train-remote.md)하는 방법에 대해 자세히 알아보세요.
