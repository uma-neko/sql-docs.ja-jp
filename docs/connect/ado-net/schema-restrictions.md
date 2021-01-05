---
title: スキーマの制限
description: Microsoft SqlClient Data Provider for SQL Server を使用するときに、GetSchema で使用できるスキーマ制限について説明します。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051386"
---
# <a name="schema-restrictions"></a>スキーマの制限

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

**GetSchema** メソッドの 2 番目のオプション パラメーターは、返されるスキーマ情報の量を制限するために使用される制限で、文字列の配列として **GetSchema** メソッドに渡されます。 配列での位置により、渡すことができる値が決定します。これは、制限の番号に相当します。  
  
たとえば、次の表は、Microsoft SqlClient Data Provider for SQL Server を使用する "テーブル" スキーマ コレクションでサポートされている制限を示しています。 SQL Server スキーマ コレクションの追加の制限をこのトピックの終わりに示します。  

|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|TABLE_CATALOG|1|  
|Owner|@Owner|TABLE_SCHEMA|2|  
|テーブル|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>制限値の指定  

"Tables" スキーマ コレクションの制限の 1 つを使用するには、4 つの要素を使って文字列の配列を作成してから、制限の番号と一致する要素内に値を配置します。 たとえば、**GetSchema** メソッドにより返されるテーブルを、"Sales" スキーマ内のテーブルのみに制限するには、**GetSchema** メソッドに渡す前に、配列の 2 番目の要素を "Sales" に設定します。  
  
> [!NOTE]
> - `SqlClient` の制限のコレクションには、追加の `ParameterName` 列があります。 制限の既定の列は、下位互換性のために存在してはいますが、現在は無視されています。 制限の値を指定する場合、文字列置換ではなく、パラメーター付きのクエリを使って、SQL への注入攻撃のリスクを最小限にする必要があります。  
> - 配列内の要素数は、指定したスキーマ コレクションでサポートされる制限数以下にする必要があります。そうでない場合、<xref:System.ArgumentException> がスローされます。 制限は最大数よりも小さい場合があります。 指定されていない制限は、null (無制限) と見なされます。  
  
Microsoft SqlClient Data Provider for SQL Server に対してクエリを実行して、サポートされている制限の一覧を確認するには、"Restrictions" という制限のスキーマ コレクションの名前を指定して **GetSchema** メソッドを呼び出します。 これにより、コレクション名の一覧、制限の名前、既定の制限値、および制限の番号と共に、<xref:System.Data.DataTable> が返されます。  
  
### <a name="example"></a>例  

Microsoft SqlClient Data Provider for SQL Server <xref:Microsoft.Data.SqlClient.SqlConnection> クラスの <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> メソッドを使って、**AdventureWorks** サンプル データベースに含まれるすべてのテーブルに関するスキーマ情報を取得し、返される情報を "Sales" スキーマ内のテーブルのみに制限する方法について、いくつかの例を次に示します。  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>SQL Server スキーマの制限  

 次の表に、SQL Server スキーマ コレクションの制限を示します。  
  
### <a name="users"></a>Users  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>データベース  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|名前|@Name|名前|1|  
  
### <a name="tables"></a>[テーブル]  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|TABLE_CATALOG|1|  
|Owner|@Owner|TABLE_SCHEMA|2|  
|テーブル|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>列  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|TABLE_CATALOG|1|  
|Owner|@Owner|TABLE_SCHEMA|2|  
|テーブル|@Table|TABLE_NAME|3|  
|Column|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|TABLE_CATALOG|1|  
|Owner|@Owner|TABLE_SCHEMA|2|  
|テーブル|@Table|TABLE_NAME|3|  
|Column|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>Views  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|TABLE_CATALOG|1|  
|Owner|@Owner|TABLE_SCHEMA|2|  
|テーブル|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|VIEW_CATALOG|1|  
|Owner|@Owner|VIEW_SCHEMA|2|  
|テーブル|@Table|VIEW_NAME|3|  
|Column|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|SPECIFIC_CATALOG|1|  
|Owner|@Owner|SPECIFIC_SCHEMA|2|  
|名前|@Name|SPECIFIC_NAME|3|  
|パラメーター|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>プロシージャ  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|SPECIFIC_CATALOG|1|  
|Owner|@Owner|SPECIFIC_SCHEMA|2|  
|名前|@Name|SPECIFIC_NAME|3|  
|種類|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|db_name()|1|  
|Owner|@Owner|user_name()|2|  
|テーブル|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|Column|@Column|c.name|5|  
  
### <a name="indexes"></a>Indexes  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|db_name()|1|  
|Owner|@Owner|user_name()|2|  
|テーブル|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|制限の名前|パラメーター名|制限の既定値|制限の番号|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalog|@Catalog|CONSTRAINT_CATALOG|1|  
|Owner|@Owner|CONSTRAINT_SCHEMA|2|  
|テーブル|@Table|TABLE_NAME|3|  
|名前|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>関連項目

- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
