---
title: Microsoft 상업용 Marketplace 의 기능
description: 이 문서에서는 상업용 Marketplace 거래 옵션에 대한 가격 책정, 청구, 송장 작성 및 지급 고려 사항에 대해 설명합니다.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: 653c55fa7476fa5fed077002db226297a33dfef6
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119432"
---
# <a name="commercial-marketplace-transact-capabilities"></a>상업용 Marketplace의 거래 기능

## <a name="transactions-by-publishing-option"></a>게시 옵션별 거래

게시자나 Microsoft는 상업용 Marketplace의 제품에 대한 소프트웨어 라이선스 트랜잭션을 관리할 책임이 있습니다. 제품에 대해 선택하는 게시 옵션에 따라 트랜잭션 관리자가 결정됩니다. 각 게시 옵션에 대한 가용성 및 설명은 [게시 옵션 확인](./determine-your-listing-type.md#choose-a-publishing-option)을 참조하세요.

### <a name="list-trial-and-byol-publishing-options"></a>목록, 평가판 및 BYOL 게시 옵션

기존 상거래 기능이 있는 게시자는 판촉 및 사용자 확보를 위해 목록, 평가판 및 BYOL(Bring-Your-Own-License) 게시 옵션 중에서 선택할 수 있습니다. 이러한 옵션을 사용할 때 Microsoft는 게시자의 소프트웨어 라이선스 트랜잭션에 직접 참여하지 않으며 연결된 트랜잭션 요금은 없습니다. 게시자는 주문, 이행, 계량, 청구, 송장, 지불 및 수금을 포함하되 이에 제한되지 않는 소프트웨어 라이선스 트랜잭션의 모든 측면을 지원할 책임이 있습니다. 목록 및 평가판 게시 옵션을 사용할 때 게시자는 고객으로부터 수금된 게시자 소프트웨어 라이선스 요금을 100% 보유합니다.

### <a name="transact-publishing-option"></a>거래 게시 옵션

거래 게시 옵션은 Microsoft 상거래 기능을 활용하고, 검색 및 평가에서 구매 및 구현에 이르는 종단 간 환경을 제공합니다. 거래 제품은 기존 Microsoft 구독 또는 신용 카드로 요금이 청구되므로 Microsoft에서는 게시자를 대신해 클라우드 Marketplace 트랜잭션을 호스트할 수 있습니다.

파트너 센터에서 새 제품을 만들 때 거래 옵션을 선택합니다. **제품 설정** 페이지의 **설정 세부 정보**에서 "예, Microsoft를 통해 판매하고 Microsoft가 대신 트랜잭션을 호스트하도록 하겠습니다."를 선택합니다. 이 옵션은 제품 형식에 대해 거래를 사용할 수 있는 경우에만 표시됩니다.

## <a name="transact-overview"></a>거래 개요

거래 게시 옵션을 사용하는 경우 Microsoft는 타사 소프트웨어의 판매 및 일부 제품 형식의 배포를 고객의 Azure 구독에서 사용할 수 있도록 설정합니다. 게시자는 청구 모델 및 제품 유형을 선택하는 경우 인프라 요금 및 사용자 고유의 소프트웨어 라이선스 요금의 청구에 대해 고려해야 합니다.

현재 다음 제품 형식에서 거래 게시 옵션이 지원됩니다.

- 가상 머신
- Azure 애플리케이션
- SaaS 애플리케이션

### <a name="billing-infrastructure-costs"></a>인프라 비용 청구

**Virtual machines** 및 **azure 응용 프로그램**의 경우 azure 인프라 사용 요금은 고객의 azure 구독에 청구 됩니다. 인프라 사용 요금의 가격이 책정되면 고객 청구서에 소프트웨어 공급 기업의 라이선스 요금과는 별도로 표시됩니다.

**SaaS 앱**의 경우 게시자는 Azure 인프라 사용 요금 및 소프트웨어 라이선스 요금을 단일 비용 항목으로 처리해야 합니다.  이는 고객에게 고정 요금으로 표시됩니다. Azure 인프라 사용량은 관리되며 파트너에게 직접 청구됩니다. 실제 인프라 사용 요금은 고객에게 표시되지 않습니다. 일반적으로 게시자는 Azure 인프라 사용 요금을 소프트웨어 라이선스 가격 책정에 추가하는 것을 선택합니다. 소프트웨어 라이선스 요금은 측정되거나 사용되지 않습니다.

## <a name="transact-billing-models"></a>거래 청구 모델

사용되는 트랜잭션 옵션에 따라 소프트웨어 라이선스 요금은 다음과 같습니다.

- **무료** – 소프트웨어 라이선스 요금이 부과되지 않습니다.
- **BYOL(Bring Your Own License)** - 소프트웨어 라이선스에 대한 적용 가능한 요금이 게시자와 고객 간에 직접 관리됩니다. Microsoft는 Azure 인프라 사용 요금을 통해서만 전달합니다. 이는 가상 머신 및 Azure 응용 프로그램에만 적용 됩니다.
- **종량제** – 소프트웨어 라이선스 요금이 사용된 Azure 인프라를 기준으로 시간당, 코어당(vCPU) 가격 책정 요금으로 표시됩니다. 이는 가상 머신 및 Azure 응용 프로그램에만 적용 됩니다.
- **구독 가격** – 소프트웨어 라이선스 요금은 정액 요금 또는 사용자 단위로 청구되는 월별 또는 연간 반복 요금으로 제공됩니다. 이는 SaaS 앱 (월간 또는 연간) 및 Azure 응용 프로그램 관리 앱 (월간)에만 적용 됩니다.
- **평가판 소프트웨어** – 30일 또는 90일 동안 소프트웨어 라이선스 요금이 부과되지 않습니다.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>무료 및 BYOL(사용자 라이선스 필요) 가격 책정

무료 또는 사용자 라이선스 필요 트랜잭션 제품을 게시하는 경우 Microsoft는 해당 소프트웨어 라이선스 요금에 대한 판매 트랜잭션을 촉진하는 역할을 담당하지 않습니다. 목록 및 평가판 게시 옵션과 같이 게시자는 소프트웨어 라이선스 요금을 100% 보유합니다.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>종량제 및 구독(사이트 기준) 가격 책정

종량제 또는 구독 트랜잭션 제품을 게시하는 경우 Microsoft는소프트웨어 라이선스 구입, 반품 및 지불 거절을 처리하는 기술 및 서비스를 제공합니다. 이 시나리오에서 게시자는 Microsoft가 이러한 목적을 위한 에이전트 역할을 하도록 권한을 부여합니다. 게시자는 Microsoft가 판매자, 공급 기업, 배포자 및 라이선스 허가자로서의 역할을 유지하는 동시에 소프트웨어 라이선스 트랜잭션을 촉진하도록 허용합니다.

Microsoft를 통해 고객은 Microsoft의 상업용 Marketplace 및 최종 사용자 사용권 계약의 사용 약관에 따라 소프트웨어를 주문, 라이선스 부여 및 사용할 수 있습니다. 사용자 고유의 최종 사용자 사용권 계약을 제공하거나 제품을 만들 때 [표준 계약](./standard-contract.md)을 선택해야 합니다.

### <a name="free-software-trials"></a>소프트웨어 평가판

거래 게시 시나리오의 경우 30일 또는 90일 동안 소프트웨어 라이선스를 무료로 사용할 수 있습니다. 이 할인 기능은 파트너 솔루션 사용으로 인한 Azure 인프라 사용 비용은 포함하지 않습니다.

### <a name="private-offers"></a>프라이빗 제품

제품의 수익화를 위해 제품 유형 및 청구 모델을 사용하는 것 외에도, 프라이빗 제품을 거래하거나, 협상된 거래별 가격 책정 또는 사용자 지정 구성을 완료할 수 있습니다. 프라이빗 제품은 3가지 거래 게시 옵션 모두에서 지원됩니다.

이 옵션을 사용 하면 공개적으로 제공되는 제품보다 더 높거나 더 낮은 가격 책정을 사용할 수 있습니다. 프라이빗 제품은 할인하거나 제품에 대한 프리미엄을 추가하는 데 사용될 수 있습니다. 프라이빗 제품은 제품 수준에서 Azure 구독을 허용 목록에 추가하면 둘 이상의 고객이 사용할 수 있습니다.

### <a name="examples"></a>예

**종량제** 

종량제의 비용 구조는 다음과 같습니다.

|라이선스 비용  | 시간당 $1.00   |
|---------|---------|
|Azure 사용량 비용(D1/1개 코어)    |   시간당 $0.14     |
|*Microsoft에서 고객에게 청구하는 요금*    |  *시간당 $1.14*       |
||

이 시나리오에서 Microsoft는 게시된 VM 이미지 사용에 대해 시간당 $1.14를 청구합니다.

|Microsoft 청구  | 시간당 $1.14  |
|---------|---------|
|Microsoft는 라이선스 비용의 80%를 지불합니다.|   시간당 $0.80     |
|Microsoft는 라이선스 비용의 20%를 유지합니다.  |  시간당 $0.20       |
|Microsoft는 Azure 사용 비용의 100%를 유지합니다. | 시간당 $0.14 |
||

**BYOL(사용자 라이선스 필요)**

BYOL의 비용 구조는 다음과 같습니다.

|라이선스 비용  | 사용자가 라이선스 요금을 협상하고 청구합니다.  |
|---------|---------|
|Azure 사용량 비용(D1/1개 코어)    |   시간당 $0.14     |
|*Microsoft에서 고객에게 청구하는 요금*    |  *시간당 $0.14*       |
||

이 시나리오에서 Microsoft는 게시된 VM 이미지 사용에 대해 시간당 $0.14를 청구합니다.

|Microsoft 청구  | 시간당 $0.14  |
|---------|---------|
|Microsoft는 Azure 사용량 비용을 유지합니다.    |   시간당 $0.14     |
|Microsoft은 라이선스 비용의 0%를 유지합니다.   |  시간당 $0.00       |
||

**SaaS 앱 구독**

이 옵션은 Microsoft를 통해 판매하도록 구성해야 하며, 월별 또는 연간 기준으로 정액제 또는 사용자 단위로 요금이 책정될 수 있습니다. SaaS 제품에 대해 **Microsoft를 통해 판매** 옵션을 사용하도록 설정하는 경우 다음과 같은 비용 구조가 적용됩니다.

| 라이선스 비용       | 매월 $100.00  |
|--------------|---------|
| Azure 사용량 비용(D1/1개 코어)    | 고객이 아닌 게시자에게 직접 청구됩니다. |
| *Microsoft에서 고객에게 청구하는 요금*    |  *월간 $100.00(참고: 게시자는 발생하거나 통과된 인프라 비용을 라이선스 요금으로 계산해야 함)*  |
||

이 시나리오에서 Microsoft는 소프트웨어 라이선스에 대해 $100.00를 청구하고, 게시자에게 $80.00를 지급합니다.

Marketplace 서비스 요금 절감에 적격한 파트너는 2019년 5월부터 2020년 6월까지 SaaS 제품에 대한 트랜잭션 요금이 감소합니다.

이 시나리오에서 Microsoft는 소프트웨어 라이선스에 대해 $100.00를 청구하고, 게시자에게 $90.00를 지급합니다.

|Microsoft 청구  | 매월 $100.00  |
|---------|---------|
|Microsoft는 라이선스 비용의 80%를 지불합니다. <br> \* Microsoft는 적격 SaaS 앱에 대해 90%의 라이선스 비용을 지급합니다.   |   매월 $80.00 <br> \* 매월 $90.00    |
|Microsoft는 라이선스 비용의 20%를 유지합니다. <br> \* Microsoft는 적격 SaaS 앱에 대해 10%의 라이선스 비용을 유지합니다.  |  매월 $20.00 <br> \* $10.00     |

상업적 marketplace에 게시 하는 특정 SaaS 제품의 경우 Microsoft는 Microsoft 게시자 계약에 설명 된 대로 20%에서 10%까지 **Marketplace 서비스 요금** 을 절감 합니다. 제품이 해당 자격을 획득하려면 Microsoft에서 하나 이상의 제품을 IP 공동 판매 준비 또는 IP 공동 판매 우선 순위로 지정해야 합니다. 이러한 Marketplace 서비스 요금 절감 혜택을 당월에 적용받으려면 전월 말로부터 영업일로 5일 이상 전에 자격을 충족해야 합니다. Marketplace 서비스 요금은 Vm, 관리 되는 앱 또는 상업적 Marketplace를 통해 제공 되는 기타 제품에는 적용 되지 않습니다. 이러한 요금 절감은 2019년 5월 1일부터 2020년 6월 30일까지 Microsoft에서 수금한 라이선스 요금에 대해 적격 제품에 제공됩니다. 이 기간 후에는 요금이 정상으로 돌아옵니다.

### <a name="customer-invoicing-payment-billing-and-collections"></a>고객 송장 처리, 지불, 청구 및 수금

**송장 처리 및 지불** – 게시자는 고객의 기본 송장 처리 방법을 사용하여 구독 또는 PAYGO 소프트웨어 라이선스 요금을 제공할 수 있습니다.

**기업계약** – 고객의 기본 송장 처리 방법이 Microsoft 기업 계약인 경우 소프트웨어 라이선스 요금은 이 송장 처리 방법을 사용하여 Azure별 사용 요금과는 별도로 항목별 요금으로 청구됩니다.

**신용 카드 및 월별 청구서** – 고객은 신용 카드 및 월별 청구서를 사용하여 지불할 수도 있습니다. 이 경우 소프트웨어 라이선스 요금은 기업 계약 시나리오와 마찬가지로 Azure별 사용 요금과는 별도로 항목별 요금으로 청구됩니다.

**무료 크레딧과 현금 약정 금액** - 기업 계약에서 현금 약정 금액으로 Azure에 선불하는 것을 선택하거나 Azure에서 무료 크레딧을 제공받는 고객도 있습니다. 이러한 크레딧은 Azure 사용에 대한 요금을 지불하는 데는 사용할 수 있으나, 게시자 소프트웨어 라이선스 요금 지불에는 사용할 수 없습니다.

**요금 청구 및 징수** – 게시자 소프트웨어 라이선스 요금 청구는 고객이 선택한 송장 처리 방법을 사용하여 표시되며 송장 처리 타임라인을 따릅니다. 기업 계약을 사용하지 않는 고객은 Marketplace 소프트웨어 라이선스에 대해 월별로 청구됩니다. 기업 계약을 사용하는 고객은 분기별로 표시된 청구서를 통해 월별로 청구됩니다.

구독 또는 종량제 가격 책정 모델을 선택한 경우 Microsoft는 게시자의 에이전트 역할을 하며 대금 청구, 지불 및 수금의 모든 측면을 담당합니다.

### <a name="publisher-payout-and-reporting"></a>게시자 지급 및 보고

Microsoft가 에이전트로서 수금한 모든 소프트웨어 라이선스 요금은 달리 지정하지 않은 경우 20% 거래 요금이 적용되고 게시자 지급 시점에 차감됩니다.

고객은 일반적으로 기업 계약 또는 종량제 계약이 활성화된 신용 카드를 사용하여 구매합니다. 계약 유형에 따라 청구, 송장, 수금 및 지급 시점이 결정됩니다.

>[!NOTE]
>거래 게시 옵션에 대한 모든 보고 및 인사이트는 파트너 센터의 분석 섹션을 통해 사용할 수 있습니다.

#### <a name="billing-questions-and-support"></a>청구 관련 질문 및 지원

자세한 내용과 법률 규정은 [게시자 계약](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)(파트너 센터에서 확인 가능)을 참조하세요.

청구 관련 질문에 대한 도움말은 [상업용 Marketplace 게시자 지원](https://aka.ms/marketplacepublishersupport)에 문의하세요.

## <a name="transact-requirements"></a>거래 요구 사항

이 섹션에서는 각 제품 유형에 대한 거래 요구 사항을 설명합니다.

### <a name="requirements-for-all-offer-types"></a>모든 제품 유형에 대한 요구 사항

- 제품의 가격 책정 모델과 관계 없이 Microsoft 계정 및 금융 정보는 거래 게시 옵션에 필요합니다.
- 필수 금융 정보에는 지급 계정 및 세금 프로필이 포함됩니다.

이러한 계정을 설정 하는 방법에 대 한 자세한 내용은 [파트너 센터에서 상용 마켓플레이스 계정 관리](partner-center-portal/manage-account.md)를 참조 하세요.

### <a name="requirements-for-specific-offer-types"></a>특정 제품 유형에 대한 요구 사항

거래 게시 옵션은 다음과 같은 Marketplace 제품 유형에서만 사용할 수 있습니다.

- **가상 머신** – 무료, 사용자 라이선스 보유 또는 종 량 제 모델을 선택 하 고 제품 수준에서 정의 된 sku로 제공 합니다. 고객의 Azure 청구서에서 Microsoft는 게시자 소프트웨어 라이선스 요금을 기본 Azure 인프라 요금과는 별도로 표시합니다. Azure 인프라 요금은 게시자 소프트웨어 사용으로 발생됩니다.

- **Azure 응용 프로그램: 솔루션 템플릿 또는 관리 되는 앱** – 하나 이상의 가상 머신을 프로 비전 하 고 가상 머신 가격의 합계를 가져옵니다. 단일 플랜의 관리형 앱은 가격 책정 모델로 가상 머신 가격 책정 대신 고정 월간 구독을 선택할 수 있습니다. 일부 경우에 Azure 인프라 사용 요금은 동일한 청구서에 소프트웨어 라이선스 요금과는 별도로 표시되어 고객에게 전달됩니다. 그러나 ISV 인프라 요금에 대 한 관리 되는 앱을 구성 하는 경우 Azure 리소스는 게시자로 청구 되 고 고객은 인프라, 소프트웨어 라이선스 및 관리 서비스 비용을 포함 하는 정액 요금을 받습니다.

- **Saas 응용 프로그램** -다중 테 넌 트 솔루션 이어야 하 고, 인증을 위해 [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 사용 하 고, [saas 처리 api](partner-center-portal/pc-saas-fulfillment-api-v2.md)와 통합 해야 합니다. Azure 인프라 사용량은 사용자 (파트너)와 직접 청구 되므로 Azure 인프라 사용 요금 및 소프트웨어 라이선스 요금은 단일 비용 항목으로 고려해 야 합니다. 자세한 지침은 [상업적 marketplace에서 새 SaaS 제품 만들기](partner-center-portal/create-new-saas-offer.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- 제품의 선택 및 구성을 완료하려면 제품 유형 섹션별 게시 옵션에서 자격 요구 사항을 검토합니다.
- 솔루션이 제품 유형 및 구성에 매핑되는 방법에 대한 예제는 상점별 게시 패턴을 검토합니다.
