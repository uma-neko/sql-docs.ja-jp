---
description: DELETE (Transact-SQL)
title: DELETE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DELETE
- DELETE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server]
- DELETE statement [SQL Server], about DELETE statement
- views [SQL Server], deleting rows
- removing rows
- tables [SQL Server], deleting rows
- DELETE statement [SQL Server]
- deleting rows
- row removal [SQL Server], DELETE statement
- deleting data
ms.assetid: ed6b2105-0f35-408f-ba51-e36ade7ad5b2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 59637197b72232df9f5054b88ea9a111f34b58a0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464093"
---
# <a name="delete-transact-sql"></a>DELETE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] のテーブルまたはビューから 1 つ以上の行を削除します。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ WITH <common_table_expression> [ ,...n ] ]  
DELETE   
    [ TOP ( expression ) [ PERCENT ] ]   
    [ FROM ]   
    { { table_alias  
      | <object>   
      | rowset_function_limited   
      [ WITH ( table_hint_limited [ ...n ] ) ] }   
      | @table_variable  
    }  
    [ <OUTPUT Clause> ]  
    [ FROM table_source [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                   { { [ GLOBAL ] cursor_name }   
                       | cursor_variable_name   
                   }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <Query Hint> [ ,...n ] ) ]   
[; ]  
  
<object> ::=  
{   
    [ server_name.database_name.schema_name.   
      | database_name. [ schema_name ] .   
      | schema_name.  
    ]  
    table_or_view_name   
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics

[ WITH <common_table_expression> [ ,...n ] ] 
DELETE [database_name . [ schema ] . | schema. ] table_name  
FROM [database_name . [ schema ] . | schema. ] table_name 
JOIN {<join_table_source>}[ ,...n ]  
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DELETE 
    [ FROM [database_name . [ schema ] . | schema. ] table_name ]   
    [ WHERE <search_condition> ]   
    [ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引数
 WITH \<common_table_expression>  
 DELETE ステートメントのスコープ内で定義された、一時的な名前付き結果セット (共通テーブル式とも呼ばれる) を指定します。 結果セットは SELECT ステートメントから派生します。  
  
 共通テーブル式は、SELECT、INSERT、UPDATE、CREATE VIEW の各ステートメントでも使用できます。 詳細については、「[WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)」を参照してください。  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 削除するランダムな行数または比率 (%) を指定します。 *expression* は行数または行の比率 (%) にすることができます。 INSERT、UPDATE、または DELETE を使用する TOP 式で参照される行は、順序付けされません。 詳細については、「[TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)」を参照してください。  
  
 FROM  
 DELETE キーワードと対象の *table_or_view_name* または *rowset_function_limited* の間で使用できる、省略可能なキーワードです。  
  
 *table_alias*  
 FROM *table_source* 句で指定される別名です。行を削除するテーブルまたはビューを表します。  
  
 *server_name*  
 **適用対象**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降。  
  
 テーブルまたはビューがあるサーバー名 (リンクされたサーバー名またはサーバー名として [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 関数を使用) です。 *server_name* が指定されている場合、*database_name* と *schema_name* が必要です。  
  
 *database_name*  
 データベースの名前。  
  
 *schema_name*  
 テーブルまたはビューが属するスキーマの名前です。  
  
 *table_or_view_name*  
 行を削除するテーブルまたはビューの名前です。  
  
 テーブル変数は、そのスコープ内では、DELETE ステートメントでテーブル ソースとしても使用できます。  
  
 *table_or_view_name* が参照するビューは更新可能であることが条件となります。また、そのビュー定義の FROM 句ではベース テーブルを 1 つだけ参照している必要があります。 更新可能なビューの詳細については、「[CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)」を参照してください。  
  
 *rowset_function_limited*  
 **適用対象**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降。  
  
 プロバイダーの機能によって、[OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 関数、または [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 関数のどちらかです。  
  
 WITH **(** \<table_hint_limited> [... *n*] **)**  
 対象のテーブルに設定可能なテーブル ヒントを 1 つ以上指定します。 キーワード WITH とかっこが必要です。 NOLOCK および READUNCOMMITTED は指定できません。 テーブル ヒントの詳細については、「[テーブル ヒント &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)」を参照してください。  
  
 \<OUTPUT_Clause>  
 DELETE 操作の一部として、削除された行または行に基づく式を返します。 OUTPUT 句は、ビューまたはリモート テーブルを対象とする DML ステートメントではサポートされません。 この句の引数と動作について詳しくは、「[OUTPUT 句 &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)」をご覧ください。  
  
 FROM *table_source*  
 追加の FROM 句を指定します。 DELETE に追加されたこの [!INCLUDE[tsql](../../includes/tsql-md.md)] 拡張機能では、\<table_source> のデータを指定して、最初の FROM 句のテーブルから対応する行を削除できます。  
  
 WHERE 句内のサブクエリを使用する代わりに、この拡張機能で結合を指定して、削除する行を特定できます。  
  
 詳細については、「[FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)」を参照してください。  
  
 WHERE  
 削除する行数を制限するときに使用する条件を指定します。 WHERE 句を指定しない場合は、DELETE によってテーブルからすべての行が削除されます。  
  
 WHERE 句に指定する内容によって、削除操作は次の 2 種類に分けられます。  
  
-   検索結果削除。削除する行を限定する検索条件を指定します。 たとえば、WHERE *column_name* = *value* のように使います。  
  
-   位置指定削除。CURRENT OF 句を使用してカーソルを指定します。 削除操作は、カーソルの現在の位置で発生します。 位置指定削除は、WHERE *search_condition* 句によって削除する行を限定する、検索結果削除の DELETE ステートメントよりも正確です。 検索結果削除の DELETE ステートメントでは、検索条件で 1 つの行が一意に識別されない場合、複数の行が削除されます。  
  
\<search_condition>  
 削除する行を制限する条件を指定します。 検索条件に含まれる述語の数に制限はありません。 詳しくは、「[検索条件 &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md)」をご覧ください。  
  
 CURRENT OF  
 指定したカーソルの現在位置で DELETE を実行します。  
  
 GLOBAL  
 *cursor_name* でグローバル カーソルを参照することを指定します。  
  
 *cursor_name*  
 フェッチが行われるオープン カーソルの名前を指定します。 *cursor_name* という名前のグローバル カーソルとローカル カーソルの両方がある場合、GLOBAL を指定すると、この引数はグローバル カーソルを参照します。GLOBAL を指定しないと、この引数はローカル カーソルを参照します。 カーソルは、更新可能である必要があります。  
  
 *cursor_variable_name*  
 カーソル変数の名前を指定します。 カーソル変数は、更新可能なカーソルを参照する必要があります。  
  
 OPTION **(** \<query_hint> [ **,** ... *n*] **)**  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)]のステートメント処理をカスタマイズするためのオプティマイザー ヒントを示すキーワードです。 詳細については、「[クエリ ヒント &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)」を参照してください。  
  
## <a name="best-practices"></a>ベスト プラクティス  
 テーブル内のすべての行を削除するには、TRUNCATE TABLE を使用します。 DELETE と比べると TRUNCATE TABLE の方が高速で、システムとトランザクション ログのリソース使用量も少なくて済みます。 TRUNCATE TABLE には制限があり、たとえば、テーブルがレプリケーションに参加することはできません。 詳細については、「[TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)」を参照してください。  
  
 @@ROWCOUNT 関数を使用して、削除される行数をクライアント アプリケーションに返します。 詳細については、「[@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)」を参照してください。  
  
## <a name="error-handling"></a>エラー処理  
 TRY...CATCH 構造でステートメントを指定することで、DELETE ステートメントのエラー処理を実装できます。  
  
 DELETE ステートメントは、トリガーに違反したり、FOREIGN KEY 制約で別のテーブル内のデータによって参照されている行を削除しようとすると、失敗する可能性があります。 DELETE で複数の行を削除するときに、削除される行のいずれかがトリガーや制約に違反すると、ステートメントは取り消され、エラーが返されます。行は削除されません。  
  
 DELETE ステートメントで式の評価中に算術エラー (オーバーフロー、0 による除算、またはドメイン エラー) が発生すると、[!INCLUDE[ssDE](../../includes/ssde-md.md)]では SET ARITHABORT が ON に設定されている場合と同様にこれらのエラーが処理されます。 残りのバッチは取り消され、エラー メッセージが返されます。  
  
## <a name="interoperability"></a>相互運用性  
 変更するオブジェクトがテーブル変数の場合は、ユーザー定義関数内で DELETE を使用できます。  
  
 FILESTREAM 列を含む行を削除すると、その基となるファイル システム ファイルも削除されます。 基になるファイルは、FILESTREAM ガベージ コレクターによって削除されます。 詳しくは、「[Transact-SQL による FILESTREAM データへのアクセス](../../relational-databases/blob/access-filestream-data-with-transact-sql.md)」をご覧ください。  
  
 INSTEAD OF トリガーが定義されているビューを直接または間接的に参照している DELETE ステートメントでは、FROM 句は指定できません。 INSTEAD OF トリガーの詳細については、「[CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)」を参照してください。  
  
## <a name="limitations-and-restrictions"></a>制限事項と制約事項  
 TOP を DELETE と共に使用する場合、参照される行は任意の順序に並べられません。また、このステートメントで、ORDER BY 句を直接指定することはできません。 TOP を使用して、意味のある順序で行を削除する必要がある場合は、サブセレクト ステートメントで ORDER BY 句を指定して TOP を使用する必要があります。 例については、後の「例」のセクションを参照してください。  
  
 パーティション ビューに対して DELETE ステートメントで TOP を使用することはできません。  
  
## <a name="locking-behavior"></a>ロック動作  
 既定では、DELETE ステートメントは、常に、それにより変更されるテーブル オブジェクトについてインテント排他 (IX) ロックを獲得し、トランザクションが完了するまでそのロックを保持します。 インテント排他 (IX) ロックをかけたトランザクション以外はデータを変更できませんが、NOLOCK ヒントまたは READUNCOMMITTED 分離レベルが使用されている場合に限り、読み取り操作は行うことができます。 別のロック手法を指定することで DELETE ステートメントの期間のこの既定の動作をオーバーライドするテーブル ヒントを指定できます。ただし、このヒントは、経験豊富な開発者およびデータベース管理者が最後の手段としてのみ使用することを推奨します。 詳細については、「[テーブル ヒント &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)」を参照してください。  
  
 ヒープから行を削除するときには、 [!INCLUDE[ssDE](../../includes/ssde-md.md)] によって、操作に行またはページ ロックが使用されることがあります。 その結果、削除操作で空になったページがヒープに割り当てられたままになります。 空のページの割り当てが解除されないと、データベース内の他のオブジェクトで該当の領域を再利用できなくなります。  
  
 ヒープ内の行を削除し、ページの割り当てを解除するには、次のいずれかの方法を使用します。  
  
-   DELETE ステートメントで TABLOCK ヒントを指定します。 TABLOCK ヒントを使用すると、削除操作では、行またはページ ロックではなく、テーブルの排他的ロックが取得されます。 これにより、ページの割り当てを解除できるようになります。 TABLOCK ヒントについて詳しくは、「[テーブル ヒント &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)」をご覧ください。  
  
-   テーブルからすべての行を削除する場合は、`TRUNCATE TABLE` を使用します。  
  
-   行を削除する前に、ヒープにクラスター化インデックスを作成します。 作成したクラスター化インデックスは、行を削除した後、削除できます。 この方法は前の 2 つの方法より時間がかかり、一時リソースがより多く使用されます。  
  
> [!NOTE]  
>  空のページは、いつでも `ALTER TABLE <table_name> REBUILD` ステートメントを使ってヒープから削除できます。  
  
## <a name="logging-behavior"></a>ログ記録の動作  
DELETE ステートメントは、常に完全にログに記録されます。  
  
## <a name="security"></a>セキュリティ  
  
### <a name="permissions"></a>アクセス許可  
 対象のテーブルに対する `DELETE` 権限が必要です。 ステートメントに WHERE 句が含まれる場合は、`SELECT` 権限も必要です。  
  
 DELETE 権限は、既定では `sysadmin` 固定サーバー ロール、`db_owner` および `db_datawriter` 固定データベース ロールのメンバー、テーブル所有者に与えられています。 `sysadmin`、`db_owner`、`db_securityadmin` ロールのメンバーと、テーブル所有者は、他のユーザーに権限を譲渡できます。  
  
## <a name="examples"></a>例  
  
|カテゴリ|主な構文要素|  
|--------------|------------------------------|  
|[基本構文](#BasicSyntax)|DELETE|  
|[削除する行数を制限する](#LimitRows)|WHERE、FROM、カーソル|  
|[リモート テーブルから行を削除する](#RemoteTables)|リンク サーバー、OPENQUERY 行セット関数、OPENDATASOURCE 行セット関数|  
|[DELETE ステートメントの結果をキャプチャする](#CaptureResults)|OUTPUT 句|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> 基本構文  
 このセクションの例では、最低限必要な構文を使用して DELETE ステートメントの基本機能を示します。  
  
#### <a name="a-using-delete-with-no-where-clause"></a>A. WHERE 句を指定せずに DELETE を使用する  
 次の例では、削除する行数を制限する WHERE 句が指定されていないため、 `SalesPersonQuotaHistory` データベース内の [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] テーブルからすべての行が削除されます。  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory;  
GO  
```  
  
###  <a name="limiting-the-rows-deleted"></a><a name="LimitRows"></a> 削除する行数を制限する  
 このセクションの例では、削除する行数を制限する方法を示します。  
  
#### <a name="b-using-the-where-clause-to-delete-a-set-of-rows"></a>B. WHERE 句を使用して行セットを削除する  
 次の例は、`StandardCost` 列の値が `1000.00` より大きいすべての行を [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内の `ProductCostHistory`テーブルから削除します。  
  
```sql
DELETE FROM Production.ProductCostHistory  
WHERE StandardCost > 1000.00;  
GO  
```  
  
 次の例では、より複雑な WHERE 句を示します。 WHERE 句では、削除する行を決定するために満たす必要がある 2 つの条件を定義しています。 `StandardCost` 列の値が `12.00` から `14.00` までの範囲に含まれ、 `SellEndDate` 列の値が NULL であることが必要です。 この例では、削除される行数を返す **\@\@ROWCOUNT** 関数から値も出力されます。  
  
```sql
DELETE Production.ProductCostHistory  
WHERE StandardCost BETWEEN 12.00 AND 14.00  
      AND EndDate IS NULL;  
PRINT 'Number of rows deleted is ' + CAST(@@ROWCOUNT as char(3));  
```  
  
#### <a name="c-using-a-cursor-to-determine-the-row-to-delete"></a>C. カーソルを使用して削除する行を決定する  
 次の例は、`complex_cursor` というカーソルを使用している 1 行を [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内の `EmployeePayHistory` テーブルから削除します。 この操作では、カーソルから現在フェッチされている 1 行だけが削除されます。  
  
```sql
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
DELETE FROM HumanResources.EmployeePayHistory  
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
#### <a name="d-using-joins-and-subqueries-to-data-in-one-table-to-delete-rows-in-another-table"></a>D. 1 つのテーブルへの結合およびサブクエリを使用して、別のテーブルの行を削除する  
 次の例では、1 つのテーブル内の行を、別のテーブルのデータに基づいて削除する 2 つの方法を示します。 どちらの例も、`SalesPerson` テーブルに格納されている今年に入ってからの売り上げに基づいて、[!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内の `SalesPersonQuotaHistory` テーブルから行を削除します。 最初の `DELETE` ステートメントは ISO 互換のサブクエリ ソリューションを示しています。また、2 つ目の `DELETE` ステートメントは、2 つのテーブルを結合する [!INCLUDE[tsql](../../includes/tsql-md.md)] FROM 拡張機能を示しています。  
  
```sql
-- SQL-2003 Standard subquery  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
WHERE BusinessEntityID IN   
    (SELECT BusinessEntityID   
     FROM Sales.SalesPerson   
     WHERE SalesYTD > 2500000.00);  
GO  
```  
  
```sql
-- Transact-SQL extension  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
INNER JOIN Sales.SalesPerson AS sp  
ON spqh.BusinessEntityID = sp.BusinessEntityID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
```sql
-- No need to mention target table more than once.  
  
DELETE spqh  
  FROM  
        Sales.SalesPersonQuotaHistory AS spqh  
    INNER JOIN Sales.SalesPerson AS sp  
        ON spqh.BusinessEntityID = sp.BusinessEntityID  
  WHERE  sp.SalesYTD > 2500000.00;  
```  
  
#### <a name="e-using-top-to-limit-the-number-of-rows-deleted"></a>E. TOP を使用して削除する行数を制限する  
 DELETE ステートメントで TOP (*n*) 句を使用した場合、ランダムに選択される '*n*' 行に対して削除操作が実行されます。 次の例では、納期が 2006 年 7 月 1 日より早い `20` 行を [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内の `PurchaseOrderDetail` テーブルからランダムに選択して削除します。  
  
```sql
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 TOP を使用して、意味のある順序で行を削除する必要がある場合は、サブセレクト ステートメントで ORDER BY を指定して TOP を使用する必要があります。 次のクエリでは、納期が早いものから 10 行を `PurchaseOrderDetail` テーブルから削除します。 10 行だけを確実に削除するために、サブセレクト ステートメントではテーブルの主キーの列 (`PurchaseOrderID`) を指定しています。 サブセレクト ステートメントで非キー列を指定すると、指定した列に重複する値が含まれる場合、10 行以上の行が削除される可能性があります。  
  
```sql
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
###  <a name="deleting-rows-from-a-remote-table"></a><a name="RemoteTables"></a> リモート テーブルから行を削除する  
 このセクションの例では、[リンク サーバー](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)または[行セット関数](../functions/opendatasource-transact-sql.md)を使用してリモート テーブルを参照し、リモート テーブルから行を削除する方法を示します。 リモート テーブルとは、別のサーバーまたは別の SQL Server インスタンスにあるテーブルのことです。  
  
**適用対象**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降。  
  
#### <a name="f-deleting-data-from-a-remote-table-by-using-a-linked-server"></a>F. リンク サーバーを使用してリモート テーブルからデータを削除する  
 次の例では、リモート テーブルの行を削除します。 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) を使用してリモート データ ソースへのリンクを作成した後、 *server.catalog.schema.object* という形式の、4 つの要素で構成されたオブジェクト名の一部として、リンク サーバー名 `MyLinkServer` を指定します。  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_name\instance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
DELETE MyLinkServer.AdventureWorks2012.HumanResources.Department 
WHERE DepartmentID > 16;  
GO  
```  
  
#### <a name="g-deleting-data-from-a-remote-table-by-using-the-openquery-function"></a>G. OPENQUERY 関数を使用してリモート テーブルからデータを削除する  
 次の例では、[OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 行セット関数を指定してリモート テーブルから行を削除します。 この例では、前の例で作成したリンク サーバー名を使用します。  
  
```sql
DELETE OPENQUERY (MyLinkServer, 'SELECT Name, GroupName 
FROM AdventureWorks2012.HumanResources.Department  
WHERE DepartmentID = 18');  
GO  
```  
  
#### <a name="h-deleting-data-from-a-remote-table-by-using-the-opendatasource-function"></a>H. OPENDATASOURCE 関数を使用してリモート テーブルからデータを削除する  
 次の例では、[OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 行セット関数を指定してリモート テーブルから行を削除します。 *server_name* または *server_name\instance_name* という形式を使用して、データ ソースの有効なサーバー名を指定します。  
  
```sql
DELETE FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department   
WHERE DepartmentID = 17;
```  
  
###  <a name="capturing-the-results-of-the-delete-statement"></a><a name="CaptureResults"></a> DELETE ステートメントの結果をキャプチャする  
  
#### <a name="i-using-delete-with-the-output-clause"></a>I. DELETE を OUTPUT 句と共に使用する  
 次の例では、`DELETE` ステートメントの結果を [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内のテーブル変数に保存する方法を示します。  
  
```sql
DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] 
FROM Sales.ShoppingCartItem 
WHERE ShoppingCartID = 20621;  
GO  
```  
  
#### <a name="j-using-output-with-from_table_name-in-a-delete-statement"></a>J. OUTPUT を DELETE ステートメント内で <from_table_name> と共に使用する  
 次の例は、`DELETE` ステートメントの `FROM` 句で定義された検索条件に基づいて、[!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] データベース内の `ProductProductPhoto` テーブルの行を削除します。 `OUTPUT` 句では、削除されるテーブルの列 ( `DELETED.ProductID`、 `DELETED.ProductPhotoID`)、および `Product` テーブルの列を返します。 これは `FROM` 句で削除する行を指定するときに使用されます。  
  
```sql
DECLARE @MyTableVar table (  
    ProductID int NOT NULL,   
    ProductName nvarchar(50)NOT NULL,  
    ProductModelID int NOT NULL,   
    PhotoID int NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>例: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="k-delete-all-rows-from-a-table"></a>K. テーブルからすべての行を削除する  
 次の例では、削除する行数を制限する WHERE 句が指定されていないため、`Table1` テーブルからすべての行が削除されます。  
  
```sql
DELETE FROM Table1;  
```  
  
### <a name="l-delete-a-set-of-rows-from-a-table"></a>L. テーブルから行のセットを削除する

 次の例では、`StandardCost` 列の値が 1000.00 より大きいすべての行を `Table1` テーブルから削除します。  
  
```sql
DELETE FROM Table1  
WHERE StandardCost > 1000.00;  
```  
  
### <a name="m-using-label-with-a-delete-statement"></a>M. DELETE ステートメントで LABEL を使用する

 次の例では、DELETE ステートメントでラベルを使用します。  
  
```sql
DELETE FROM Table1  
OPTION ( LABEL = N'label1' );  
  
```  
  
### <a name="n-using-a-label-and-a-query-hint-with-the-delete-statement"></a>N. DELETE ステートメントでラベルとクエリ ヒントを使用する

 このクエリでは、DELETE ステートメントでクエリ結合ヒントを使用する場合の基本構文を示します。 結合ヒントと OPTION 句の使用方法の詳細については、「[OPTION 句 (Transact-SQL)](../queries/option-clause-transact-sql.md)」を参照してください。
  
```sql
-- Uses AdventureWorks  
  
DELETE FROM dbo.FactInternetSales  
WHERE ProductKey IN (   
    SELECT T1.ProductKey FROM dbo.DimProduct T1   
    JOIN dbo.DimProductSubcategory T2  
    ON T1.ProductSubcategoryKey = T2.ProductSubcategoryKey  
    WHERE T2.EnglishProductSubcategoryName = 'Road Bikes' )  
OPTION ( LABEL = N'CustomJoin', HASH JOIN ) ;  
```  

### <a name="o-delete-using-a-where-clause"></a>O. WHERE 句を使用した削除

このクエリでは、FROM 句ではなく、WHERE 句を使用して削除する方法が示されています。

```sql
DELETE tableA WHERE EXISTS (
SELECT TOP 1 1 FROM tableB tb WHERE tb.col1 = tableA.col1
)
```

### <a name="p-delete-based-on-the-result-of-joining-with-another-table"></a>P. 別のテーブルとの結合結果に基づいて削除する

この例は、別のテーブルとの結合結果に基づいてテーブルから削除する方法を示しています。

```sql
CREATE TABLE dbo.Table1   
    (ColA int NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  

CREATE TABLE dbo.Table2   
    (ColA int PRIMARY KEY NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  

DELETE dbo.Table2   
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA)
    WHERE dboTable2.ColA = 1;  
```

## <a name="see-also"></a>参照

 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)  
  
