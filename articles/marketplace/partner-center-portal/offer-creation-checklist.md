---
title: Microsoft 상업적 marketplace의 SaaS 제품 생성 검사 목록
description: 파트너 센터에서 SaaS 제품 만들기 프로세스에 제공할 수 있는 세부 정보입니다.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: c56295f1e56e4ba3b6af9caf8ba38ce1f0552eeb
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86101711"
---
# <a name="saas-offer-creation-checklist-in-partner-center"></a>파트너 센터에서 SaaS 제품 생성 검사 목록

SaaS 제품 만들기 프로세스는 여러 페이지로 구성되어 있습니다.  각 페이지에서 제공할 수 있는 세부 정보와 각 항목에 대해 자세히 알아볼 수 있는 링크는 다음과 같습니다.

제공하거나 지정해야 하는 항목은 아래에 나와 있습니다.  일부 영역은 선택적이거나 기본값을 제공합니다(원하는 대로 변경할 수 있음).  다음 섹션에서 여기에 나열된 순서대로 작업을 수행할 필요는 없습니다.

>[!Note]
>불가능 SaaS 제품을 만드는 경우 [saas 처리 api](./pc-saas-fulfillment-apis.md)와의 통합을 구현 해야 합니다.  Transactability가 Marketplace에서 제대로 작동 하는 유일한 방법은 Api와의 통합입니다.

| **항목**    | **용도**  |
| :---------- | :-------------------|
| [**새 제품 모달**](#new-offer-modal) | 제품 ID 정보를 수집합니다.  |
| [제품 설치 페이지](#offer-setup-page) | 주요 기능을 사용하도록 옵트인하고 Microsoft를 통해 제품을 판매하는 방법을 선택할 수 있습니다.  |
| [속성 페이지](#properties-page) | 마켓플레이스에서 제품을 그룹화하는 데 사용되는 범주와 제품 및 앱 버전을 지원하는 법적 계약을 정의합니다. |
| [제품 목록 페이지](#offer-listing-page) | 제품 및 마케팅 자산에 대한 설명을 포함하여 마켓플레이스에 표시되는 제품 세부 정보를 정의합니다.|
| [미리 보기 페이지](#preview-page) | 제품을 더 광범위한 마켓플레이스 대상에 게시하기 전에 제품 출시를 위해 제한된 미리 보기 대상 그룹을 정의합니다.|
| [제품 기술 구성 페이지](#technical-configuration-page)  |  Microsoft를 통해 제품을 판매하도록 선택한 경우에만 사용할 수 있습니다.  Marketplace에서 제품에 연결 하는 데 사용 하는 기술 세부 정보 (방문 페이지 URL, 연결 webhook URL, Azure AD 테 넌 트 ID 및 Azure AD 앱 ID)를 정의 합니다.  이러한 매개 변수는 SaaS 배송 및 Marketplace 요금제 청구 Api와 올바르게 통합 하는 데 필요 합니다.|
| [**새 플랜 모달**](#plan-identity-modal) | 플랜 ID 정보를 수집합니다.  |
| [플랜 목록 페이지](#plan-listing-page)  | Microsoft를 통해 제품을 판매하도록 선택한 경우에만 사용할 수 있습니다. 마켓플레이스에서 플랜을 나열하는 데 사용되는 세부 정보를 정의합니다.  |
| [플랜 가격 책정 및 가용성 페이지](#plan-pricing--availability-page)  | Microsoft를 통해 제품을 판매하도록 선택한 경우에만 사용할 수 있습니다.  제품의 각 플랜(버전)에 대한 비즈니스 특성(가격 책정 모델), 대상 그룹 및 시장 가용성을 수집합니다.  |
| [시험 사용 목록 페이지](#test-drive-listing-page)  | 제품 시험 사용을 제공하도록 선택한 경우에만 사용할 수 있습니다. 마켓플레이스에서 시험 사용을 나열하는 데 사용되는 세부 정보를 정의합니다.  |
| 시험 사용 기술 구성 페이지  | 제품 시험 사용을 제공하도록 선택한 경우에만 사용할 수 있습니다. 고객이 제품을 구입하기 전에 사용해 볼 수 있는 시연(또는 "시험 사용")에 대한 기술 세부 정보를 정의합니다.  |
| [검토 및 게시 페이지](#review-and-publish-page)  | 게시하려는 변경 내용을 선택하고, 각 페이지의 상태를 확인하고, 인증 팀에 설명을 제공합니다.  |


## <a name="new-offer-modal"></a>새 제품 모달 

제공해야 하는 첫 번째 정보는 제품의 ID 및 별칭입니다. 

| **필드 이름**    | **참고 사항**   |  
| :---------------- | :-----------| 
| 제품 ID  | 필수 사항이며 만든 후에는 변경할 수 없습니다. 최대 50자이고 소문자 영숫자 문자, 대시 또는 밑줄만 사용해야 합니다. |
| 제품 별칭  | 필수 요소. |

## <a name="offer-setup-page"></a>제품 설치 페이지

제품 설치 페이지에서는 다양한 채널 및 판매 동작을 선택하고 시험 사용 및 잠재 고객과 같은 주요 기능의 사용을 선언할 수 있습니다. 

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------|  
| Microsoft를 통해 판매하시겠습니까?  | 필수 사항입니다. Default: 예 |
| 잠재 고객이 제품 목록과 상호 작용하도록 하려면 어떻게 해야 하나요? (활용 방안)  | Microsoft를 통해 판매하지 않는 경우 필수입니다. Default: 무료 평가판, 옵션: "지금 받기", "무료 평가판", "게시자 문의". |
| 체험 URL  | 고객이 제품 목록과 상호 작용하는 방식으로 "무료 평가판"을 선택하는 경우 필수입니다. |
| 제품 URL  | 고객이 제품 목록과 상호 작용하는 방식으로 "지금 받기"를 선택하는 경우 필수입니다. |
| 채널  | 선택 사항입니다. Default: CSP(재판매인) 채널에는 옵트인되지 않습니다.  |
| 시험 사용 | 선택 사항입니다. Default: 시험 사용을 설정할 수 없습니다.  |
| 시험 사용 유형 | 시험 사용을 설정하는 경우 필수입니다. Default: 선택한 항목 없음. 옵션: Azure Resource Manager, Dynamics 365 for Business Central, Dynamics 365 for Customer Engagement, Dynamics 365 for Operations, 논리 앱, Power BI.  |
| 잠재 고객 - CRM 시스템에 연결 | Microsoft를 통해 판매하거나 제품을 "게시자 문의"로 나열하는 경우 필수입니다. 기본값: 연결된 CRM 시스템 없음. CRM 옵션: Azure 테이블, Azure Blob, Dynamics CRM online, HTTP 엔드포인트, Marketo, Salesforce  |

## <a name="properties-page"></a>속성 페이지

속성 페이지에서는 마켓플레이스에서 제품을 그룹화하는 데 사용되는 범주와 제품 및 앱 버전을 지원하는 법적 계약을 정의합니다. 제품이 적절히 표시되고 올바른 고객 그룹에게 제안되도록 이 페이지에서 제품에 대한 완전하고 정확한 세부 정보를 제공해야 합니다. 

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------|  
| 범주 및 하위 범주 | 필수 사항, 1 및 최대 3. Default: 선택한 항목 없음. |
| 산업 및 하위 산업 | 선택 사항입니다. L1 산업 최대 2개, 각 L1 산업에서 하위 산업 최대 2개, 기본값: 선택한 항목 없음 |
| 앱 버전  | 선택 사항입니다. Default: 없음 |
| 표준 계약 사용  | (선택 사항) 기본값: 선택하지 않음.  | |
| 사용 약관  | 표준 계약을 선택하지 않는 경우 필수 사항.  |

## <a name="offer-listing-page"></a>제품 목록 페이지

목록 페이지에서는 고객이 마켓플레이스에서 제품 목록을 볼 때 표시되는 텍스트와 이미지를 제공합니다. 

| **필드 이름**    | **참고 사항**   |
| :---------------- | :-----------| 
| 속성  | 필수 사항, 최대 50자. |
| 요약  | 필수 사항, 최대 100자. | 
| Description  | 필수 사항, 최대 3,000자. |
| 시작하기 지침  | 필수 사항, 최대 3,000자. |
| 시작하기 지침  | 필수 사항, 최대 3,000자. |
| 검색 키워드  | 선택 사항, 권장, 최대 3개 키워드. |
| 개인정보처리방침 URL  | 필수 요소. |
| CSP 프로그램 마케팅 자료 URL  | (선택 사항) |
| 유용한 링크 제목 + URL  | 선택 사항입니다. |
| 지원 문서 제목 + 파일  | 필수 사항, 최소 1 및 최대 3. PDF 파일 형식이어야 합니다. |
| 스크린샷  | 필수 사항, 최소 1개 스크린샷 및 최대 5개, 4개 이상 권장. PNG 형식 1280 X 720이어야 합니다. |
| 매장 로고 (소형, 중형, 크게, 전체)  | 작음 (48 X 48) 및 큼 (216 X 216) 필요; 기타 크기는 선택 사항 이지만 권장 됩니다. Medium (90 x 90), 와이드 (255 x 115). 는에 있어야 합니다. PNG 형식입니다. |
| 동영상 이름 + URL + 썸네일  | 선택 사항, 권장, 최대 4개 동영상. 썸네일은 PNG 형식의 1280 x 720이어야 합니다. 동영상은 YouTube 또는 Vimeo에서 호스트되어야 합니다. |
| 연락처(CSP 프로그램, 엔지니어링, 지원)  | 엔지니어링 및 지원 연락처(이름, 이메일 및 전화 번호)는 필수 사항입니다. CSP 프로그램 연락처는 선택 사항이지만 권장 사항입니다. |
| 지원 URL  | 필수 요소. |

## <a name="preview-page"></a>미리 보기 페이지

미리 보기 페이지에서는 제품 미리 보기에 대한 액세스 권한이 있는 대상 그룹을 지정하여 제품이 라이브 상태가 되기 전에 모든 요구 사항을 충족하는지 확인할 수 있습니다. 

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------| 
| AAD/MSA 이메일 + 설명 | 필수 사항, 수동으로 입력하는 경우 최소 1개 및 최대 10개, CSV 파일을 업로드하는 경우 최대 20개. |

## <a name="technical-configuration-page"></a>기술 구성 페이지 

기술 구성 페이지에서는 Microsoft가 제품에 연결하는 데 사용하는 기술 세부 정보를 지정합니다. Microsoft를 통해 판매하지 않기로 결정한 경우 이 페이지는 표시되지 않습니다.

| **필드 이름**    | **참고 사항**   |  
| :---------------- | :-----------| 
| 방문 페이지 URL | Microsoft를 통해 판매하는 경우 필수입니다. |
| 연결 웹후크 | Microsoft를 통해 판매하는 경우 필수입니다. |
| Azure AD 테넌트 ID | Microsoft를 통해 판매하는 경우 필수입니다. |
| Azure AD 앱 ID | Microsoft를 통해 판매하는 경우 필수입니다. |

## <a name="plan-identity-modal"></a>플랜 ID 모달

제공해야 하는 첫 번째 정보는 플랜의 이름 및 ID입니다. Microsoft를 통해 판매하지 않기로 결정한 경우 이 페이지는 표시되지 않습니다.

| **필드 이름**    | **참고 사항**   |  
| :---------------- | :-----------| 
| 플랜 ID  | Microsoft를 통해 판매하는 경우 필수입니다. 만든 후에는 변경할 수 없습니다. 최대 50자이고 소문자 영숫자 문자, 대시 또는 밑줄만 사용해야 합니다. |
| 플랜 이름  | Microsoft를 통해 판매하는 경우 필수입니다. 제품의 모든 플랜에서 고유해야 합니다. 최대 50자. |

## <a name="plan-listing-page"></a>플랜 목록 페이지

플랜 목록 페이지에서는 고객이 마켓플레이스에서 플랜을 볼 때 표시되는 텍스트를 제공합니다. Microsoft를 통해 판매하지 않기로 결정한 경우 이 페이지는 표시되지 않습니다.

| **필드 이름**    | **참고 사항**   |  
| :---------------- | :-----------| 
| 플랜 설명   | Microsoft를 통해 판매하는 경우 필수입니다. 최대 500자. | |

## <a name="plan-pricing--availability-page"></a>플랜 가격 책정 및 가용성 페이지

플랜 가격 책정 및 가용성 페이지에서는 제품의 각 플랜(버전)에 대한 비즈니스 특성, 대상 그룹 및 시장 가용성을 정의합니다. Microsoft를 통해 판매하지 않기로 결정한 경우 이 페이지는 표시되지 않습니다.

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------| 
| 시장 가용성  | 필수 사항, 최소 1 및 최대 141. |
| 가격 책정 모델  | 필수 요소. Default: 정액제. 옵션: 정액제, 사용자당. |
| 최소 및 최대 사용자  | 선택 사항이며 사용자 기반 가격 책정 모델을 선택한 경우에만 사용할 수 있습니다. |
| 청구 기간  | 필수 사항입니다. Default: 매월 옵션: 월간, 연간. |
| Price  | 필수 사항, 월간 청구 기간을 선택한 경우 월 USD, 연간 청구 기간을 선택한 경우 연 USD입니다. |
| 플랜 대상 그룹  | (선택 사항) Default: 공개 플랜. 옵션: 공개, 비공개(테넌트 ID 기준) |
| 제한된 플랜 대상 그룹(테넌트 ID + 설명)  | 비공개 플랜을 선택하는 경우 필수입니다. 수동으로 입력하는 경우 최소 1개 및 최대 10개의 테넌트 ID를 입력할 수 있고, CSV 파일을 가져오는 경우 최대 2만 개까지 가능합니다. |

## <a name="test-drive-listing-page"></a>시험 사용 목록 페이지

제품 시험 사용을 제공하도록 선택한 경우에만 사용할 수 있습니다. 마켓플레이스에서 시험 사용을 나열하는 데 사용되는 세부 정보를 정의합니다.

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------| 
| Description  | 필수 사항입니다. |
| 사용자 설명서 이름 + 파일  | 필수 사항, 최대 1개 문서. PDF 형식이어야 합니다. |
| 동영상 이름, URL + 썸네일  | 선택 사항이며 권장 사항입니다. 썸네일은 JPGP 또는 PNG 형식의 533 x 324이어야 합니다. 동영상은 YouTube 또는 Vimeo에서 호스트되어야 합니다. |

## <a name="review-and-publish-page"></a>검토 및 게시 페이지

| **필드 이름**    | **참고 사항**   | 
| :---------------- | :-----------| 
| 인증 참고 사항  | (선택 사항) |

## <a name="next-steps"></a>다음 단계

- [새로운 SaaS 제품 만들기](./create-new-saas-offer.md)
