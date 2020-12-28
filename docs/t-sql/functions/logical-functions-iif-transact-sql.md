---
description: 論理関数 - IIF (Transact-SQL)
title: IIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b58823a6b9e6b43b3458392d1b9016c0716a2e32
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116363"
---
# <a name="logical-functions---iif-transact-sql"></a>論理関数 - IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ではブール式が true または false のいずれに評価されるかによって、2 つの値のいずれかを返します。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```syntaxsql
IIF ( boolean_expression, true_value, false_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引数
 *boolean_expression*  
 有効なブール式。  
  
 この引数がブール式でない場合、構文エラーが発生します。  
  
 *true_value*  
 *boolean_expression* が true に評価された場合に返す値。  
  
 *false_value*  
 *boolean_expression* が false に評価された場合に返す値。  
  
## <a name="return-types"></a>戻り値の型  
 *true_value* および *false_value* の型から最も優先順位の高い型を返します。 詳細については、「[データ型の優先順位 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)」を参照してください。
  
## <a name="remarks"></a>解説  
 IIF は CASE 式の簡略版です。 最初の引数として渡されたブール式を評価し、評価の結果に基づいて他の 2 つの引数のいずれかを返します。 つまり、 *true_value* ブール式が true の場合、返されると、 *false_value* ブール式が false または不明のかどうかに返されます。 任意の型の *true_value* と *false_value* を指定できます。 ブール式、NULL 処理、および戻り値の型に対する CASE 式に適用されるのと同じ規則が IIF にも適用されます。 詳細については、を参照してください。 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md).  
  
 IIF が CASE に変換されるという事実は、この関数の動作の他の側面にも影響を与えます。 CASE 式では最大 10 のレベルまで入れ子が許容されるため、IIF ステートメントでも最大 10 のレベルまで入れ子が許容されます。 また、IIF では、意味が同等の CASE 式として他のサーバーにリモート処理を行い、リモート処理された CASE 式のすべての動作を実行します。  
  
## <a name="examples"></a>例  
  
### <a name="a-simple-iif-example"></a>A. 簡単な IIF の例  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;  
SELECT IIF ( @a > @b, 'TRUE', 'FALSE' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
  
(1 row(s) affected)  
```  
  
### <a name="b-iif-with-null-constants"></a>B. NULL 定数を使用する IIF  
  
```sql 
SELECT IIF ( 45 > 30, NULL, NULL ) AS Result;  
```  
  
 このステートメントの結果はエラーになります。  
  
### <a name="c-iif-with-null-parameters"></a>C. NULL のパラメーターを使用する IIF  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT IIF ( 45 > 30, @p, @s ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>参照  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
