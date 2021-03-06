---
title: Video Indexer에서 개인 모델 사용자 지정 - Azure
titleSuffix: Azure Media Services
description: 이 문서에서는 Video Indexer 개인 모델의 개념과 이를 사용자 지정하는 방법을 간략하게 설명합니다.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 3fabba98cb137975da749411ca9accb5a951742d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "73838307"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Video Indexer에서 개인 모델 사용자 지정

Video Indexer는 비디오에서 유명인 인식을 지원 합니다. 유명인 인식 기능은 IMDB, Wikipedia, 상위 LinkedIn 영향력 행사자 등 일반적으로 요청되는 데이터 원본을 기준으로 하는 약 백만 개의 얼굴을 처리합니다. Video Indexer에서 인식 되지 않는 얼굴은 여전히 검색 되지만 명명 되지 않습니다. 고객은 사용자 지정 개인 모델을 작성 하 고 Video Indexer를 사용 하 여 기본적으로 인식 되지 않는 얼굴을 인식할 수 있습니다. 고객은 사람의 이름을 사용자의 얼굴 이미지 파일과 연결 하 여 이러한 사용자 모델을 빌드할 수 있습니다.  

계정이 다른 사용 사례로 맞춘 계정 당 여러 사용자 모델을 만들 수 있는 이점을 누릴 수 있습니다. 예를 들어 계정의 콘텐츠를 다른 채널로 정렬 하려는 경우 각 채널에 대 한 별도의 사용자 모델을 만들 수 있습니다. 

> [!NOTE]
> 각 사용자 모델은 최대 100만 명의 사용자를 지원 하며, 각 계정에는 50 사람 모델의 제한이 있습니다. 

모델이 생성되면, 비디오를 업로드/인덱싱하거나 다시 인덱싱할 때 특정 개인 모델의 모델 ID를 제공하여 모델을 사용할 수 있습니다. 비디오에 대 한 새 얼굴을 학습 하면 비디오가 연결 된 특정 사용자 지정 모델을 업데이트 합니다. 

다중 개인 모델 지원이 필요하지 않은 경우, 업로드/인덱싱하거나 다시 인덱싱할 때 비디오에 개인 모델 ID를 할당하지 마세요. 이 경우 Video Indexer는 계정에 기본 사용자 모델을 사용 합니다. 

[웹 사이트를 사용 하 여 개인 모델 사용자 지정](customize-person-model-with-website.md) 항목에 설명 된 대로 Video Indexer 웹 사이트를 사용 하 여 비디오에서 검색 된 얼굴을 편집 하 고 계정에서 여러 사용자 지정 사용자 모델을 관리할 수 있습니다. Api를 [사용 하 여 개인 모델 사용자 지정](customize-person-model-with-api.md)에 설명 된 대로 api를 사용할 수도 있습니다.
