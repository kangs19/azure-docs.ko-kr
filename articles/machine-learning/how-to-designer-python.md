---
title: 디자이너에서 Python 스크립트 실행
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 디자이너에서 Python을 사용하여 데이터를 변환하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 02/28/2020
ms.custom: designer, tracking-python
ms.openlocfilehash: 31028ba0417f312aa0caaf49cb4b2d432f89979f
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86041016"
---
# <a name="run-python-code-in-azure-machine-learning-designer"></a>Azure Machine Learning 디자이너에서 Python 코드 실행

이 문서에서는 [Python 스크립트 실행](algorithm-module-reference/execute-python-script.md) 모듈을 사용하여 Azure Machine Learning 디자이너에 사용자 지정 논리를 추가하는 방법에 대해 알아봅니다. 다음 방법에서는 Pandas 라이브러리를 사용하여 간단한 기능 엔지니어링을 수행합니다.

기본 제공 코드 편집기를 사용하여 간단한 Python 논리를 빠르게 추가할 수 있습니다. 더 복잡한 코드를 추가하거나 추가 Python 라이브러리를 업로드하려면 Zip 파일 메서드를 사용해야 합니다.

기본 실행 환경에서는 Python의 Anacondas 배포를 사용합니다. 사전 설치된 패키지의 전체 목록은 [Python 스크립트 실행 모듈 참조](algorithm-module-reference/execute-python-script.md) 페이지를 참조하세요.

![Python 입력 맵 실행](media/how-to-designer-python/execute-python-map.png)

## <a name="execute-python-written-in-the-designer"></a>디자이너에서 작성한 Python 실행

### <a name="add-the-execute-python-script-module"></a>Python 스크립트 실행 모듈 추가

1. 디자이너 팔레트에서 **Python 스크립트 실행** 모듈을 찾습니다. **Python 언어** 섹션에서 찾을 수 있습니다.

1. 모듈을 파이프라인 캔버스로 끌어다 놓습니다.

### <a name="connect-input-datasets"></a>입력 데이터 세트 연결

이 문서에서는 **자동차 가격 데이터(원시)** 샘플 데이터 세트를 사용합니다. 

1. 데이터 세트를 파이프라인 캔버스로 끌어다 놓습니다.

1. 데이터 세트 출력 포트를 **Python 스크립트 실행** 모듈 왼쪽 상단에 있는 입력 포트에 연결합니다. 디자이너는 입력을 진입점 스크립트에 매개 변수로 표시합니다.
    
    오른쪽 입력 포트는 압축된 Python 라이브러리용으로 예약되어 있습니다.

    ![데이터 세트 연결](media/how-to-designer-python/connect-dataset.png)
        

1. 사용할 입력 포트를 적어 놓습니다. 디자이너는 `dataset1` 변수에 왼쪽 입력 포트를 할당하고 `dataset2`에 가운데 입력 포트를 할당합니다. 

**Python 스크립트 실행** 모듈에서 직접 데이터를 생성하거나 가져올 수 있으므로 입력 모듈은 선택 사항입니다.

### <a name="write-your-python-code"></a>Python 코드 작성

디자이너는 고유 Python 코드를 편집하고 입력할 수 있는 초기 진입점 스크립트를 제공합니다. 

이 예에서는 Pandas를 사용하여 자동차 데이터 세트에 있는 두 열 **가격** 및 **마력**을 결합하고 새 열 **마력당 달러**를 만듭니다. 이 열은 각 마력에 대한 비용을 나타내며 이는 자동차가 금액에 적합한지 결정하는 데 유용한 기능입니다. 

1. **Python 스크립트 실행** 모듈을 선택합니다.

1. 캔버스 오른쪽에 표시되는 창에서 **Python 스크립트** 텍스트 상자를 선택합니다.

1. 다음 코드를 복사하여 텍스트 상자에 붙여 넣습니다.

    ```python
    import pandas as pd
    
    def azureml_main(dataframe1 = None, dataframe2 = None):
        dataframe1['Dollar/HP'] = dataframe1.price / dataframe1.horsepower
        return dataframe1
    ```
    파이프라인은 다음 이미지와 같습니다.
    
    ![Python 파이프라인 실행](media/how-to-designer-python/execute-python-pipeline.png)

    진입점 스크립트는 `azureml_main` 함수를 포함해야 합니다. **Python 스크립트 실행** 모듈의 두 입력 포트에 매핑되는 두 개의 함수 매개 변수가 있습니다.

    반환 값은 Pandas 데이터 프레임이어야 합니다. 최대 2개의 데이터 프레임을 모듈 출력으로 반환할 수 있습니다.
    
1. 파이프라인을 제출합니다.

이제 자동차 추천자를 교육하는 데 유용하게 사용할 수 있는 새 기능 **달러/HP**가 포함된 데이터 세트를 사용할 수 있습니다. 이는 기능 추출 및 차원 감소 예입니다. 

## <a name="next-steps"></a>다음 단계

Azure Machine Learning 디자이너에서 [고유 데이터를 가져오는 방법](how-to-designer-import-data.md)에 대해 알아봅니다.
