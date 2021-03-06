---
title: Azure Cosmos DB 쿼리 언어 반전
description: Azure Cosmos DB에서 SQL 시스템 함수 역방향에 대해 알아봅니다.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a22e1c8a5f4350bd2f966ee48f96368c648a4a1e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302171"
---
# <a name="reverse-azure-cosmos-db"></a>REVERSE (Azure Cosmos DB)
 문자열 값을 역순으로 반환합니다.  
  
## <a name="syntax"></a>구문
  
```sql
REVERSE(<str_expr>)  
```  
  
## <a name="arguments"></a>인수
  
*str_expr*  
   문자열 식입니다.  
  
## <a name="return-types"></a>반환 형식
  
  문자열 식을 반환합니다.  
  
## <a name="examples"></a>예
  
  다음 예제에서는 쿼리에서를 사용 하는 방법을 보여 줍니다 `REVERSE` .  
  
```sql
SELECT REVERSE("Abc") AS reverse  
```  
  
 결과 집합은 다음과 같습니다.  
  
```json
[{"reverse": "cbA"}]  
```  

## <a name="remarks"></a>설명

이 시스템 함수는 인덱스를 활용 하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [문자열 함수 Azure Cosmos DB](sql-query-string-functions.md)
- [시스템 함수 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 소개](introduction.md)
