---
title: 게이트웨이 트래픽 마이그레이션 알림
description: 문서에서는 사용자에 게 Azure SQL Database 게이트웨이 IP 주소 마이그레이션에 대 한 알림을 제공 합니다.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1 
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: e9bf1f06b1ec1f99da1ce653b4bc72f4638ba451
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084956"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>최신 게이트웨이로 트래픽 마이그레이션 Azure SQL Database
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure 인프라가 개선 됨에 따라 Microsoft는 최상의 고객 환경을 제공 하기 위해 정기적으로 하드웨어를 새로 고칩니다. 향후 몇 개월 동안 새로운 하드웨어 세대를 기반으로 하는 게이트웨이를 추가 하 고, 트래픽을 마이그레이션하고, 궁극적으로는 일부 지역에서 이전 하드웨어를 기반으로 하는 게이트웨이를 서비스 해제할 계획입니다.  

고객은 각 지역에서 사용 가능한 게이트웨이를 변경 하는 것 보다 먼저 전자 메일 및 Azure Portal에 대 한 알림이 표시 됩니다. 최신 정보는 [Azure SQL Database GATEWAY IP addresses](connectivity-architecture.md#gateway-ip-addresses) 테이블에서 유지 관리 됩니다.

## <a name="status-updates"></a>상태 업데이트

# <a name="in-progress"></a>[진행 중](#tab/in-progress-ip)

### <a name="august-2020"></a>8 월 2020

새 SQL 게이트웨이가 다음 지역에 추가 됩니다.

- 오스트레일리아 동부: 13.70.112.9
- 캐나다 중부: 52.246.152.0, 20.38.144.1 
- 미국 서 부 2:40.78.240.8

이러한 SQL 게이트웨이는 2020 8 월 10 일에 고객 트래픽 수락을 시작 합니다. 

# <a name="completed"></a>[완료됨](#tab/completed-ip)

다음 게이트웨이 마이그레이션이 완료 되었습니다. 

### <a name="october-2019"></a>2019년 10월
- 브라질 남부
- 미국 서부
- 서유럽
- 미국 동부
- 미국 중부
- 동남아시아
- 미국 중남부
- 북유럽
- 미국 중북부
- 일본 서부
- 일본 동부
- 미국 동부 2
- 동아시아

---

## <a name="impact-of-this-change"></a>이 변경의 영향

트래픽 마이그레이션은 Azure SQL Database에서 데이터베이스에 대해 DNS가 확인 하는 공용 IP 주소를 변경할 수 있습니다.
다음 경우에 영향을 받을 수 있습니다.

- 온-프레미스 방화벽의 특정 게이트웨이에 대 한 IP 주소 하드 코딩
- Microsoft .SQL를 서비스 엔드포인트로 사용 하는 서브넷이 있지만 게이트웨이 IP 주소와 통신할 수 없습니다.
- 데이터베이스에 대 한 [영역 중복 구성](high-availability-sla.md#zone-redundant-configuration) 사용

다음이 있는 경우에는 영향을 받지 않습니다.

- 연결 정책으로 리디렉션
- Azure 내부에서 SQL Database 하 고 서비스 태그를 사용 하 여 연결
- SQL Server에 대해 지원 되는 버전의 JDBC 드라이버를 사용 하 여 만든 연결은 영향을 받지 않습니다. 지원 되는 JDBC 버전은 [SQL Server 용 MICROSOFT JDBC Driver 다운로드](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)를 참조 하세요.

## <a name="what-to-do-you-do-if-youre-affected"></a>영향을 받는 경우 수행할 작업

TCP 포트 1433 및 포트 범위 11000-11999에서 지역의 모든 [게이트웨이 ip 주소](connectivity-architecture.md#gateway-ip-addresses) 에 대 한 ip 주소에 대 한 아웃 바운드 트래픽을 허용 하는 것이 좋습니다. 이 권장 사항은 온-프레미스에서 연결 하는 클라이언트와 서비스 끝점을 통해 연결 하는 클라이언트에 적용 됩니다. 포트 범위에 대 한 자세한 내용은 [연결 정책](connectivity-architecture.md#connection-policy)을 참조 하세요.

Microsoft JDBC Driver 버전 4.0을 사용 하 여 응용 프로그램에서 만든 연결은 인증서 유효성 검사에 실패할 수 있습니다. 낮은 버전의 Microsoft JDBC는 인증서의 주체 필드에 CN (일반 이름)을 사용 합니다. 완화 방법은 hostNameInCertificate 속성이 *. database.windows.net로 설정 되어 있는지 확인 하는 것입니다. HostNameInCertificate 속성을 설정 하는 방법에 대 한 자세한 내용은 [암호화를 사용 하 여 연결](/sql/connect/jdbc/connecting-with-ssl-encryption)을 참조 하세요.

위의 완화 방법이 작동 하지 않는 경우 다음 URL을 사용 하 여 SQL Database 또는 SQL Managed Instance에 대 한 지원 요청을 파일에 만듭니다.https://aka.ms/getazuresupport

## <a name="next-steps"></a>다음 단계

- [AZURE SQL 연결 아키텍처](connectivity-architecture.md) 에 대해 자세히 알아보기
