---
title: Azure Cosmos DB의 다양한 API에 대해 자주 묻는 질문
description: 전역적으로 분산된 다중 모델 데이터베이스 서비스인 Azure Cosmos DB에 대해 자주 묻는 질문에 대한 대답을 살펴봅니다. 용량, 성능 수준 및 크기 조정에 대해 알아봅니다.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: f6b2d081b0dc475e0950b12de31bab83cbe7da8b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213094"
---
# <a name="frequently-asked-questions-about-different-apis-in-azure-cosmos-db"></a>Azure Cosmos DB의 다양한 API에 대한 질문과 대답

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB의 대표적인 사용 사례는 무엇인가요?

Azure Cosmos DB는 자동 크기 조정, 예측 가능한 성능, 밀리초 응답 시간의 빠른 주문, 스키마 제약 없는 데이터에 대한 쿼리 기능 등이 중요한 경우 새로운 웹, 모바일, 게임 및 IoT 애플리케이션에 적합한 솔루션입니다. Azure Cosmos DB는 애플리케이션 데이터 모델의 신속한 개발 및 연속적인 반복 지원에 적합합니다. 사용자 생성 콘텐츠 및 데이터를 관리하는 애플리케이션은 [Azure Cosmos DB의 일반적인 사용 사례](use-cases.md)입니다.

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB는 어떻게 예측 가능한 성능을 제공하나요?

RU([요청 단위](request-units.md))는 Azure Cosmos DB의 처리량 측정 단위입니다. 1RU 처리량은 1KB 문서의 GET 처리량에 해당합니다. 읽기, 쓰기, SQL 쿼리, 저장 프로시저 실행 등 Azure Cosmos DB에서의 모든 작업에는 작업을 완료하는 데 필요한 처리량을 기반으로 하는 결정적 RU 값이 있습니다. CPU, IO 및 메모리와 이 각각이 애플리케이션 처리량에 미치는 영향을 고려하는 대신 단일 RU 측정값 측면에서 고려할 수 있습니다.

초당 RU 처리량 측면에서 프로비저닝된 처리량으로 각 Azure Cosmos 컨테이너를 구성할 수 있습니다. 모든 규모의 애플리케이션에 대해 개별 요청을 벤치마킹하여 해당 RU 값을 측정하고 모든 요청에서 총 요청 단위 합계를 처리하도록 컨테이너를 프로비전할 수 있습니다. 또한 애플리케이션 요구 사항이 진화함에 따라 컨테이너의 처리량을 확장하거나 축소할 수 있습니다. 요청 단위에 대해 자세히 알아보고 컨테이너 요구 사항을 확인하는 데 도움을 받으려면 [처리량 계산기](https://www.documentdb.com/capacityplanner)를 사용하세요.

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Azure Cosmos DB에서는 키/값, 열 형식, 문서 및 그래프와 같은 다양한 데이터 모델을 어떻게 지원하나요?

Azure Cosmos DB는 ARS(아톰, 레코드 및 시퀀스) 디자인을 기반으로 하므로 키/값(테이블), 열 형식, 문서 및 그래프 데이터 모델을 기본적으로 모두 지원합니다. 아톰, 레코드 및 시퀀스는 다양한 데이터 모델에 쉽게 매핑 및 프로젝션할 수 있습니다. 모델의 하위 집합에 대한 API는 현재 사용 가능하며(SQL, MongoDB, 테이블 및 Gremlin) 추가 데이터 모델과 관련된 기타 사항은 향후 제공될 예정입니다.

Azure Cosmos DB에는 개발자로부터 스키마 또는 보조 인덱스를 요구하지 않고도 수집되는 모든 데이터를 자동으로 인덱싱할 수 있는 스키마 독립적인 인덱싱 엔진을 포함합니다. 이 엔진은 인덱스 및 쿼리 처리 하위 시스템에서 스토리지 레이아웃을 분리하는 논리적 인덱스 레이아웃 집합(반전됨, 열 형식, 트리)을 기반으로 합니다. Cosmos DB는 유선 프로토콜과 API 세트를 확장 가능한 방식으로 지원하고 핵심 데이터 모델(1)과 논리적 인덱스 레이아웃(2)으로 효율적으로 변환하여 기본적으로 둘 이상의 데이터 모델을 고유하게 지원할 수 있도록 합니다.

### <a name="can-i-use-multiple-apis-to-access-my-data"></a>여러 API를 사용하여 내 데이터에 액세스할 수 있나요?

Azure Cosmos DB는 전 세계에 배포되는 Microsoft의 다중 모델 데이터베이스 서비스입니다. 다중 모델이 Azure Cosmos DB가 여러 API와 여러 데이터 모델을 지원하는 것을 의미하는 경우 여러 API가 스토리지 및 유선 프로토콜에 대해 서로 다른 데이터 형식을 사용합니다. 예를 들어 SQL은 JSON, MongoDB는 BSON, Table은 EDM, Cassandra는 CQL, Gremlin은 JSON 형식을 사용합니다. 따라서 지정된 계정에 있는 데이터에 대한 모든 액세스에 동일한 API를 사용하는 것이 좋습니다.

각 API는 상호 운용할 수 있는 Gremlin 및 SQL API를 제외하고 독립적으로 작동합니다.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB는 HIPAA 규격인가요?

예, Azure Cosmos DB는 HIPAA 규격입니다. HIPAA는 개별적으로 식별할 수 있는 상태 정보의 사용, 공개 및 보호에 대한 요구 사항을 설정합니다. 자세한 내용은 [Microsoft 보안 센터](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)를 참조하세요.

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB의 스토리지 한도는 어느 정도인가요?

하나의 컨테이너가 Azure Cosmos DB에 저장할 수 있는 총 데이터 양에는 제한이 없습니다.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB의 처리량 한도는 어느 정도인가요?

하나의 컨테이너가 Azure Cosmos DB에서 지원할 수 있는 총 처리량에는 제한이 없습니다. 이는 대다수 파티션 키 간에 워크로드를 거의 균등하게 분산시키기 위한 것입니다.

### <a name="are-direct-and-gateway-connectivity-modes-encrypted"></a>직접 연결 및 게이트웨이 연결 모드를 암호화하나요?

예, 두 모드는 항상 완벽하게 암호화됩니다.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Azure Cosmos DB 비용은 얼마인가요?

자세한 내용은 [Azure Cosmos DB 가격 책정 세부 정보](https://azure.microsoft.com/pricing/details/cosmos-db/) 페이지를 참조하세요. Azure Cosmos DB 사용 요금은 프로비전된 컨테이너 수, 컨테이너가 온라인이었던 시간 및 각 컨테이너의 프로비전된 처리량에 따라 결정됩니다.

### <a name="is-a-free-account-available"></a>무료 계정을 사용할 수 있나요?

예, 약정이나 비용 없이 시간이 제한된 계정에 등록할 수 있습니다. 등록하려면 [무료로 Azure Cosmos DB 사용](https://azure.microsoft.com/try/cosmosdb/)을 방문하거나 [Azure Cosmos DB FAQ 사용](#try-cosmos-db)을 자세히 참조하세요.

Azure를 처음 사용하는 경우 [Azure 체험 계정](https://azure.microsoft.com/free/)에 가입하면 30일간 모든 Azure 서비스를 사용해 볼 수 있습니다. Visual Studio 구독이 있다면 모든 Azure 서비스에서 사용할 수 있는 [평가판 Azure 크레딧](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)에 대한 자격이 주어집니다.

[Azure Cosmos DB 에뮬레이터](local-emulator.md)를 사용하여 Azure 구독을 구입하지 않고도 무료로 로컬에서 애플리케이션을 개발하고 테스트할 수 있습니다. Azure Cosmos DB 에뮬레이터에서 애플리케이션이 작동하는 방식에 만족하는 경우 Azure Cosmos DB 계정을 클라우드에서 사용하도록 전환할 수 있습니다.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Azure Cosmos DB 추가 도움말은 어떻게 구할 수 있나요?

기술 관련 질문을 하려면 다음 두 가지의 질문 및 답변 포럼 중 하나에 게시합니다.

* [Microsoft Q&A 질문 페이지](https://docs.microsoft.com/answers/topics/azure-cosmos-db.html)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack Overflow는 프로그래밍 질문에 가장 적합합니다. 질문이 [주제와 관련된](https://stackoverflow.com/help/on-topic) 것인지 확인하고 [질문에 대해 분명하고 답변할 수 있도록 가능한 한 자세하게 제공하세요](https://stackoverflow.com/help/how-to-ask).

새로운 기능을 요청하려면 [사용자 의견](https://feedback.azure.com/forums/263030-azure-cosmos-db)에서 새 요청을 만듭니다.

계정 관련 문제를 해결하려면 Azure Portal에서 [지원 요청](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)을 제출합니다.

## <a name="try-azure-cosmos-db-subscriptions"></a><a id="try-cosmos-db"></a>Azure Cosmos DB 구독 사용해 보기

이제 구독이나 요금, 약정 없이 시간이 제한된 Cosmos DB를 경험할 수 있습니다. Azure Cosmos DB 구독에 등록하려면 [Try Azure Cosmos DB for free](https://azure.microsoft.com/try/cosmosdb/)(Azure Cosmos DB 체험하기)로 이동하여 개인 Microsoft 계정(MSA)을 사용합니다. 이 구독은 [Azure 평가판](https://azure.microsoft.com/free/)과 별도이며 Azure 평가판 또는 Azure 유료 구독과 함께 사용할 수 있습니다.

Azure Cosmos DB 구독 사용이 사용자 ID와 연결된 다른 구독의 옆에 있는 Azure Portal에 표시됩니다.

다음 조건은 Azure Cosmos DB 구독 사용에 적용됩니다.

* 계정 액세스는 개인 Microsoft 계정(MSA)에 부여할 수 있습니다. AAD(Active Directory) 계정 또는 회사 AAD 테넌트에 속하는 계정을 사용하지 않는 것이 좋습니다. 이와 같은 계정에는 액세스 권한 부여를 차단하는 제한 사항이 있을 수 있습니다.
* SQL, Gremlin API 및 테이블 계정에 대해 구독당 하나의 [처리량이 프로비저닝된 컨테이너](./set-throughput.md#set-throughput-on-a-container)
* MongoDB 계정에 대해 구독당 최대 3개의 [처리량이 프로비저닝된 컬렉션](./set-throughput.md#set-throughput-on-a-container)
* 구독당 하나의 [처리량이 프로비저닝된 데이터베이스](./set-throughput.md#set-throughput-on-a-database). 처리량이 프로비저닝된 데이터베이스에는 내부 컨테이너 수에 제한이 없습니다.
* 10GB 스토리지 용량
* 전역 복제는 [Azure 지역](https://azure.microsoft.com/regions/) 미국 중부, 북유럽 및 동남 아시아에서 사용할 수 있습니다.
* 컨테이너 수준에서 프로비저닝되는 경우 최대 처리량은 5,000RU/초입니다.
* 데이터베이스 수준에서 프로비저닝되는 경우 최대 처리량은 20,000RU/초입니다.
* 구독은 30일 후에 만료되고 최대 총 31일까지 확장될 수 있습니다. 만료 후 포함된 정보는 삭제됩니다.
* Azure Cosmos DB 체험 계정용으로 Azure 지원 티켓을 만들 수 없습니다. 단, 기존 지원 플랜을 사용하는 구독자에게는 지원이 제공됩니다.

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB 설정

### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Azure Cosmos DB에는 어떻게 등록하나요?

Azure Cosmos DB는 Azure Portal에서 사용할 수 있습니다. 먼저 Azure 구독에 등록합니다. 등록한 후 Azure 구독에 Azure Cosmos DB 계정을 추가할 수 있습니다.

### <a name="what-is-a-master-key"></a>마스터 키란 무엇인가요?

마스터 키는 계정의 모든 리소스에 액세스할 수 있는 보안 토큰입니다. 이 키가 있는 개인은 데이터베이스 계정의 모든 리소스에 대해 읽기 및 쓰기 액세스 권한을 가집니다. 마스터 키를 배포할 때 주의하세요. 주 마스터 키 및 보조 마스터 키는 [Azure Portal][azure-portal]의 **키** 블레이드에서 사용할 수 있습니다. 키에 대한 자세한 내용은 [액세스 키 보기, 복사 및 다시 생성](manage-with-cli.md#list-account-keys)을 참조하세요.

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>PreferredLocations를 설정할 수 있는 지역은 어디인가요?

Cosmos DB를 사용할 수 있는 모든 Azure 지역에 PreferredLocations 값을 설정할 수 있습니다. 사용 가능한 영역 목록은 [Azure 지역](https://azure.microsoft.com/regions/)을 참조하세요.

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Azure의 데이터 센터를 통해 전 세계에서 데이터를 배포하는 경우 알아야 할 점이 있나요?

Azure Cosmos DB는 [Azure 지역](https://azure.microsoft.com/regions/) 페이지에 지정된 대로 모든 Azure 지역에 존재합니다. 핵심 서비스이기 때문에 새로운 데이터 센터에는 모두 Azure Cosmos DB가 갖춰집니다.

지역을 설정할 때 Azure Cosmos DB는 국가 및 정부 클라우드를 따른다는 점을 염두에 두어야 합니다. 즉, [국가 지역](https://azure.microsoft.com/global-infrastructure/)에 계정을 만든 경우 해당 [국가 지역](https://azure.microsoft.com/global-infrastructure/) 외부로 복제할 수 없습니다. 마찬가지로 외부 계정에서 다른 국가 위치로 복제할 수 없습니다.

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>컨테이너 수준 처리량 프로비전에서 데이터베이스 수준 처리량 프로비전으로 전환할 수 있나요? 또는 그 반대로 가능한가요?

컨테이너 및 데이터베이스 수준 처리량 프로비전은 별개의 제안이며 이를 전환하려면 원본에서 대상으로 데이터를 마이그레이션해야 합니다. 즉, 새 데이터베이스 또는 새 컨테이너를 만든 다음, [대량 실행기 라이브러리](bulk-executor-overview.md) 또는 [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)를 사용하여 데이터를 마이그레이션해야 합니다.

### <a name="does-azure-cosmosdb-support-time-series-analysis"></a>Azure CosmosDB는 시계열 분석을 지원하나요?

예, Azure CosmosDB는 시계열 분석을 지원합니다. 여기 [시계열 패턴](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/samples/Patterns) 샘플이 있습니다. 이 샘플에서는 시계열 데이터에 따라 집계된 보기를 빌드하기 위해 변경 피드를 사용하는 방법을 보여 줍니다. Spark 스트리밍 또는 다른 스트림 데이터 프로세서를 사용 하여 이 접근 방식을 확장할 수 있습니다.

## <a name="what-are-the-azure-cosmos-db-service-quotas-and-throughput-limits"></a>Azure Cosmos DB 서비스 할당량 및 처리량 제한이란 무엇인가요?

자세한 내용은 Azure Cosmos DB [서비스 할당량](concepts-limits.md) 및 [컨테이너 및 데이터베이스당 처리량 제한](set-throughput.md#comparison-of-models) 문서를 참조하세요.

## <a name="frequently-asked-questions-about-sql-api"></a><a id="sql-api-faq"></a>SQL API에 대해 자주 묻는 질문

### <a name="how-do-i-start-developing-against-the-sql-api"></a>SQL API에 대해 어떻게 개발을 시작하나요?

먼저 Azure 구독에 등록해야 합니다. Azure 구독에 등록하면 사용자의 Azure 구독에 SQL API 컨테이너를 추가할 수 있습니다. Azure Cosmos DB 계정 추가에 대한 지침은 [Azure Cosmos 데이터베이스 계정 만들기](create-sql-api-dotnet.md#create-account)를 참조하세요.

[SDK](sql-api-sdk-dotnet.md) 는 .NET, Python, Node.js, JavaScript 및 Java용으로 제공됩니다. 또한 개발자는 [RESTful HTTP API](/rest/api/cosmos-db/)를 사용하여 다양한 플랫폼 및 언어의 Azure Cosmos DB 리소스를 조작할 수 있습니다.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>유리한 출발을 위해 사전에 준비된 몇 가지 샘플에 액세스할 수 있나요?

GitHub에서 SQL API [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](sql-api-nodejs-samples.md)및 [Python](sql-api-python-samples.md) SDK의 샘플을 사용할 수 있습니다.

### <a name="does-the-sql-api-database-support-schema-free-data"></a>SQL API 데이터베이스에서 스키마 제약이 없는 데이터를 지원하나요?

예, SQL API를 통해 애플리케이션은 스키마 정의 또는 힌트 없이 임의의 JSON 문서를 저장할 수 있습니다. Azure Cosmos DB SQL 쿼리 인터페이스를 통해 쿼리에 즉시 데이터를 사용할 수 있습니다.

### <a name="does-the-sql-api-support-acid-transactions"></a>SQL API에서 ACID 트랜잭션을 지원하나요?

예, SQL API는 JavaScript 저장 프로시저 및 트리거로 표현되는 문서 간 트랜잭션을 지원합니다. 동시에 실행되는 다른 코드 및 사용자 요청과 "모두 분리되거나 전혀 분리되지 않으므로" 트랜잭션은 범위가 각 컨테이너 내의 단일 파티션으로 지정되고 ACID 시맨틱으로 실행됩니다. JavaScript 애플리케이션 코드의 서버 쪽 실행을 통해 예외가 발생하는 경우 전체 트랜잭션이 롤백됩니다. 

### <a name="what-is-a-container"></a>컨테이너란?

컨테이너는 문서 및 관련 JavaScript 애플리케이션 논리의 그룹입니다. 컨테이너는 처리량 및 사용한 스토리지에 따라 [비용](performance-levels.md)이 결정되는 청구 가능 엔터티입니다. 컨테이너는 하나 이상의 파티션 또는 서버에 걸쳐 있을 수 있으며 크기가 거의 무제한인 스토리지 또는 처리량을 처리하도록 확장할 수 있습니다.

* SQL API의 경우 리소스를 컨테이너 라고 합니다.
* Cosmos DB의 MongoDB용 API 계정인 경우 컨테이너가 컬렉션에 매핑됩니다.
* Cassandra 및 Table API 계정의 경우 컨테이너는 테이블에 매핑됩니다.
* Gremlin API 계정의 경우 컨테이너는 Graph에 매핑됩니다.

컨테이너는 Azure Cosmos DB의 청구 엔터티이기도 합니다. 각 컨테이너는 프로비전된 처리량 및 사용된 스토리지 공간에 따라 시간 단위로 요금이 청구됩니다. 자세한 내용은 [Azure Cosmos DB 가격 책정](https://azure.microsoft.com/pricing/details/cosmos-db/)을 참조하세요.

### <a name="how-do-i-create-a-database"></a>데이터베이스를 어떻게 만드나요?

[컨테이너 추가](create-sql-api-java.md#add-a-container)의 설명대로 [Azure Portal](https://portal.azure.com)을 사용하거나 [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) 중 하나를 사용하거나 [REST API](/rest/api/cosmos-db/)를 사용하여 데이터베이스를 만들 수 있습니다.

### <a name="how-do-i-set-up-users-and-permissions"></a>사용자 및 사용 권한을 어떻게 설정하나요?

[Cosmos DB API SDK](sql-api-sdk-dotnet.md) 또는 [REST API](/rest/api/cosmos-db/) 중 하나를 사용하여 사용자 및 사용 권한을 만들 수 있습니다.

### <a name="does-the-sql-api-support-sql"></a>SQL API는 SQL을 지원하나요?

SQL API 계정에서 지원하는 SQL 쿼리 언어는 SQL Server에서 지원되는 쿼리 기능의 향상된 하위 집합입니다. Azure Cosmos DB SQL 쿼리 언어는 JavaScript 기반 UDF(사용자 정의 함수)를 통해 다양한 계층 구조 및 관계형 연산자를 제공합니다. JSON 문법은 레이블이 지정된 노드가 있는 트리로 JSON 문서 모델링을 허용합니다. 이 내용은 Azure Cosmos DB 자동 인덱싱 기술 및 Azure Cosmos DB의 SQL 쿼리 언어 모두에 사용됩니다. SQL 문법을 사용하는 방법에 대한 자세한 내용은 [SQL 쿼리][query] 문서를 참조하세요.

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>SQL API는 SQL 집계 함수를 지원하나요?

SQL API는 `COUNT`, `MIN`, `MAX`, `AVG` 집계 함수를 통해 모든 규모의 짧은 대기 시간 집계를 지원하고 SQL 문법을 통해 `SUM`을 지원합니다. 자세한 내용은 [집계 함수](sql-query-aggregates.md)를 참조하세요.

### <a name="how-does-the-sql-api-provide-concurrency"></a>SQL API는 어떻게 동시성을 제공하나요?

SQL API는 HTTP 엔터티 태그 또는 ETag를 통해 OCC(낙관적 동시성 제어)를 지원합니다. 모든 SQL API 리소스에는 ETag가 있고 ETag는 문서가 업데이트될 때마다 서버에서 설정됩니다. ETag 헤더 및 현재 값은 모든 응답 메시지에 포함됩니다. ETag는 If-Match 헤더와 함께 사용하여 서버가 리소스를 업데이트해야 하는지를 결정하도록 할 수 있습니다. If-Match 값은 검사해야 할 ETag 값입니다. ETag 값이 서버 ETag 값과 일치하는 경우 리소스가 업데이트됩니다. ETag가 최신 상태가 아닌 경우 서버에서는 "HTTP 412 사전 조건 실패" 응답 코드와 함께 작업을 거부합니다. 그러면 클라이언트는 리소스를 다시 페치하여 리소스에 대한 현재 ETag 값을 가져옵니다. 또한 If-None-Match 헤더와 함께 ETag를 사용하여 리소스를 다시 페치해야 하는지를 확인합니다.

.NET에서 낙관적 동시성을 사용하려면 [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) 클래스를 사용합니다. .NET 샘플은 GitHub에 있는 DocumentManagement 샘플에서 [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) 를 참조하세요.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>SQL API에서 어떻게 트랜잭션을 수행하나요?

SQL API는 JavaScript 저장 프로시저 및 트리거를 통해 언어 통합 트랜잭션을 지원합니다. 스크립트 내의 모든 데이터베이스 작업은 스냅샷이 격리된 상태에서 실행됩니다. 단일 파티션 컨테이너인 경우 실행 범위는 해당 컨테이너로 지정됩니다. 컨테이너가 분할된 경우 실행 범위는 컨테이너 내에서 파티션 키 값이 동일한 문서로 지정됩니다. 문서 버전의 스냅샷(ETags)은 트랜잭션의 시작 부분에서 찍고 스크립트가 성공한 경우에만 커밋됩니다. JavaScript에서 오류가 발생한 경우 트랜잭션이 롤백됩니다. 자세한 내용은 [Azure Cosmos DB에 대한 서버 쪽 JavaScript 프로그래밍](stored-procedures-triggers-udfs.md)을 참조하세요.

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Cosmos DB에 문서를 어떻게 일괄 삽입할 수 있나요?

다음 방법 중 하나로 Azure Cosmos DB에 문서를 대량 삽입할 수 있습니다.

* [대량 실행기 .NET 라이브러리 사용](bulk-executor-dot-net.md) 및 [대량 실행자 Java 라이브러리 사용](bulk-executor-java.md)에 설명된 대량 실행기 도구
* [Azure Cosmos DB용 데이터베이스 마이그레이션 도구](import-data.md)에 설명된 대로 데이터 마그레이션 도구를 사용합니다.
* [Azure Cosmos DB에 대한 서버 쪽 JavaScript 프로그래밍](stored-procedures-triggers-udfs.md)에 설명된 대로 저장 프로시저를 사용합니다.

### <a name="does-the-sql-api-support-resource-link-caching"></a>SQL API에서 리소스 링크 캐싱을 지원하나요?

예, Azure Cosmos DB는 RESTful 서비스이므로 리소스 링크가 제한되며 캐시될 수 있습니다. SQL API 클라이언트는 문서 또는 컨테이너와 같은 모든 리소스 읽기에 대해 “If-None-Match” 헤더를 지정한 다음, 서버 버전이 변경된 후 로컬 복사본을 업데이트할 수 있습니다.

### <a name="is-a-local-instance-of-sql-api-available"></a>SQL API의 로컬 인스턴스를 사용할 수 있나요?

예. [Azure Cosmos DB 에뮬레이터](local-emulator.md)는 신뢰도 있는 Cosmos DB 서비스의 에뮬레이션을 제공합니다. JSON 문서 만들기 및 쿼리, 컬렉션 프로비전 및 확장, 저장 프로시저 및 트리거 실행을 비롯하여 Azure Cosmos DB와 동일한 기능을 지원합니다. Azure Cosmos DB 에뮬레이터를 사용하여 애플리케이션을 개발 및 테스트하고 Azure Cosmos DB에 대한 연결 엔드포인트에 대한 단일 구성을 변경하여 글로벌 규모로 Azure에 배포할 수 있습니다.

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>이유는 포털의 데이터 탐색기에서 볼 때 반올림되는 문서에서 긴 부동 소수점 값이기 때문입니다.

이는 JavaScript의 제한 사항입니다. JavaScript는 IEEE 754에 지정된 배정밀도 부동 소수점 형식 숫자를 사용하고 -(2<sup>53</sup> - 1)과 2<sup>53</sup>-1 사이의 숫자(즉, 9007199254740991)를 안전하게 유지할 수 있습니다.

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>개체 계층 구조에서 사용 권한이 허용되나요?

ResourceTokens를 사용한 사용 권한 만들기는 컨테이너 수준 및 해당 하위 항목에서 허용됩니다(예: 문서, 첨부 파일). 이는 데이터베이스에서 권한을 생성하려고 하거나 계정 수준이 현재 허용되지 않음을 의미합니다.

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md

## <a name="next-steps"></a>다음 단계

다른 API에서 자주 묻는 질문에 대한 자세한 내용은 다음을 참조하세요.

* [Azure Cosmos DB의 MongoDB용 API](mongodb-api-faq.md)에 대해 자주 묻는 질문
* [Azure Cosmos DB의 Gremlin API](gremlin-api-faq.md)에 대해 자주 묻는 질문
* [Azure Cosmos DB의 Cassandra API](cassandra-faq.md)에 대해 자주 묻는 질문
* [Azure Cosmos DB의 Table API](table-api-faq.md)에 대해 자주 묻는 질문
