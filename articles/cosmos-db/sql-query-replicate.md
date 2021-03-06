---
title: Azure Cosmos DB 쿼리 언어로 복제
description: Azure Cosmos DB에서 SQL 시스템 함수 복제에 대해 알아봅니다.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 19fcde522c5cb0355e53a5616145f27fada7dad9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302188"
---
# <a name="replicate-azure-cosmos-db"></a>복제 (Azure Cosmos DB)
 지정한 횟수만큼 문자열 값을 반복합니다.
  
## <a name="syntax"></a>구문
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>인수
  
*str_expr*  
   문자열 식입니다.
  
*num_expr*  
   숫자 식입니다. *Num_expr* 음수 이거나 한정 되지 않은 경우 결과가 정의 되지 않습니다.
  
## <a name="return-types"></a>반환 형식
  
  문자열 식을 반환합니다.
  
## <a name="remarks"></a>설명
  결과의 최대 길이는 1만 자입니다. 예를 들어 (length (*str_expr*) * *num_expr*) <= 1만입니다.

## <a name="examples"></a>예
  
  다음 예제에서는 쿼리에서를 사용 하는 방법을 보여 줍니다 `REPLICATE` .
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 결과 집합은 다음과 같습니다.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="remarks"></a>설명

이 시스템 함수는 인덱스를 활용 하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [문자열 함수 Azure Cosmos DB](sql-query-string-functions.md)
- [시스템 함수 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 소개](introduction.md)
