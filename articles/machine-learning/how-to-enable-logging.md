---
title: Azure Machine Learning에서 로깅 사용
description: 기본 Python 로깅 패키지를 사용 하 고 SDK 관련 기능을 사용 하 여 Azure Machine Learning에서 로깅을 사용 하도록 설정 하는 방법을 알아봅니다.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: trbye
ms.date: 03/05/2020
ms.custom: tracking-python
ms.openlocfilehash: 25c0f906cdf8a351d868dcae0b794d4ea833466e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84560231"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Azure Machine Learning에서 로깅 사용
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning Python SDK를 사용하면 로컬 로깅 및 포털의 작업 영역 로깅을 위해 기본 Python 로깅 패키지와 SDK 특정 기능 둘 다로 로깅을 사용할 수 있습니다. 로그는 애플리케이션 상태에 대한 실시간 정보를 개발자에게 제공하며 오류 또는 경고 진단에 도움이 될 수 있습니다. 이 문서의 다음 영역에서는 로깅을 사용하는 다양한 방법을 설명합니다.

> [!div class="checklist"]
> * 모델 학습 및 컴퓨팅 대상
> * 이미지 만들기
> * 배포된 모델
> * Python `logging` 설정

[Azure Machine Learning 작업 영역을 만듭니다](how-to-manage-workspace.md). SDK에 대 한 자세한 내용은 [가이드를 참조](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 하십시오.

## <a name="training-models-and-compute-target-logging"></a>모델 학습 및 컴퓨팅 대상 로깅

모델 학습 프로세스 중 로깅을 사용하는 여러 가지 방법이 있으며, 표시된 예제는 일반적인 디자인 패턴을 보여 줍니다. `Experiment` 클래스의 `start_logging` 함수를 사용하여 클라우드의 작업 영역에 실행 관련 데이터를 쉽게 로그할 수 있습니다.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

추가 로깅 함수는 [실행](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) 클래스에 대 한 참조 설명서를 참조 하세요.

학습 진행 중 애플리케이션 상태의 로컬 로깅을 사용하려면 `show_output` 매개 변수를 사용합니다. 자세한 정보 로깅을 사용하면 학습 프로세스의 세부 정보와 원격 리소스 또는 컴퓨팅 대상에 대한 정보를 확인할 수 있습니다. 실험 제출 시 로깅을 사용하려면 다음 코드를 사용합니다.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

결과 실행의 `wait_for_completion` 함수에서도 동일한 매개 변수를 사용할 수 있습니다.

```python
run.wait_for_completion(show_output=True)
```

또한 SDK는 특정 학습 시나리오에서 기본 Python 로깅 패키지 사용을 지원합니다. 다음 예제에서는 `AutoMLConfig` 개체의 `INFO` 로깅 수준을 사용하도록 설정합니다.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

영구적 컴퓨팅 대상을 만드는 경우에도 `show_output` 매개 변수를 사용할 수 있습니다. 컴퓨팅 대상 생성 중 로깅을 사용하려면 `wait_for_completion` 함수에 이 매개 변수를 지정합니다.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>배포된 모델에 대한 로깅

이전에 배포된 웹 서비스에서 로그를 검색하려면 서비스를 로드하고 `get_logs()` 함수를 사용합니다. 로그에는 배포 중에 발생한 오류에 대한 자세한 정보가 포함되어 있을 수 있습니다.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

요청/응답 시간, 실패율, 예외를 모니터링할 수 있는 Application Insights를 사용하여 웹 서비스에 대한 사용자 지정 스택 추적을 로그할 수도 있습니다. Application Insights를 사용하려면 기존 웹 서비스에서 `update()` 함수를 호출합니다.

```python
service.update(enable_app_insights=True)
```

자세한 내용은 [ML 웹 서비스 끝점에서 데이터 모니터링 및 수집](how-to-enable-app-insights.md)을 참조 하세요.

## <a name="python-native-logging-settings"></a>Python 기본 로깅 설정

SDK의 특정 로그에 로깅 수준을 디버그로 설정하라는 오류가 있는 경우도 있습니다. 로깅 수준을 설정하려면 스크립트에 다음 코드를 추가합니다.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>다음 단계

* [ML 웹 서비스 엔드포인트에서 데이터 모니터링 및 수집](how-to-enable-app-insights.md)
