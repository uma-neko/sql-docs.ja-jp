---
title: ローカル トランザクション
description: Microsoft SqlClient Data Provider for SQL Server を使用してデータベースに対してトランザクションを実行する方法を示します。
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051303"
---
# <a name="local-transactions"></a>ローカル トランザクション

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET でのトランザクションは、複数のタスクをバインドして単一の作業単位として実行する場合に使用します。 たとえば、あるアプリケーションが 2 つのタスクを実行するものとします。 まず、注文情報に従ってテーブルが更新されます。 次に、在庫情報を含むテーブルが更新され、注文品の金額が借方記入されます。 いずれかのタスクが失敗すると、両方の更新がロールバックされます。  

## <a name="determining-the-transaction-type"></a>トランザクションの種類の決定

トランザクションは、単一フェーズであり、データベースによって直接処理される場合、ローカル トランザクションと見なされます。 トランザクション モニターによって調整され、トランザクションの解決にフェール セーフ機構 (2 フェーズのコミットなど) が使用されているトランザクションは、分散トランザクションと見なされます。

Microsoft SqlClient Data Provider for SQL Server には、SQL Server データベースでローカル トランザクションを実行するための独自の <xref:Microsoft.Data.SqlClient.SqlTransaction> オブジェクトがあります。 他の .NET データ プロバイダーでも、独自の `Transaction` オブジェクトが提供されています。 さらに、トランザクションを必要とする、プロバイダーに依存しないコードを記述するための <xref:System.Data.Common.DbTransaction> クラスもあります。

> [!NOTE]
> トランザクションは、サーバー上で実行するのが最も効率的です。 明示的なトランザクションを広範に使用する SQL Server データベースを操作する場合は、Transact-SQL の BEGIN TRANSACTION ステートメントを使用して、ストアド プロシージャとしてトランザクション処理を記述するとよいでしょう。

## <a name="performing-a-transaction-using-a-single-connection"></a>単一の接続を使用したトランザクションの実行 

ADO.NET では、`Connection` オブジェクトを使用してトランザクションを制御します。 ローカル トランザクションは、`BeginTransaction` メソッドを使用して開始できます。 トランザクションを開始すると、`Transaction` オブジェクトの `Command` プロパティを使用して、そのトランザクションにコマンドを参加させることができます。 次に、トランザクションの内容が成功したか失敗したかに基づいて、データ ソースに対する変更をコミットまたはロールバックします。

> [!NOTE]
> `EnlistDistributedTransaction` メソッドをローカル トランザクションで使用することはできません。

トランザクションのスコープは、接続に限定されています。 次の例では、`try` ブロック内の 2 つの個別のコマンドで構成される明示的なトランザクションを実行しています。 これらのコマンドは、AdventureWorks SQL Server サンプル データベース内の `Production.ScrapReason` テーブルに対して INSERT ステートメントを実行し、例外がスローされない場合にコミットします。 `catch` ブロック内のコードは、例外がスローされた場合にトランザクションをロールバックします。 トランザクションが完了する前に中止されるか接続が終了すると、トランザクションは自動的にロールバックされます。

## <a name="example"></a>例  

 トランザクションを実行するには、次の手順に従います。

1. <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> オブジェクトの <xref:Microsoft.Data.SqlClient.SqlConnection> メソッドを呼び出して、トランザクションの開始位置をマークします。 <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> メソッドは、トランザクションへの参照を返します。 この参照は、トランザクションに参加する <xref:Microsoft.Data.SqlClient.SqlCommand> オブジェクトに割り当てられます。

2. 実行する `Transaction` の <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A> プロパティに、<xref:Microsoft.Data.SqlClient.SqlCommand> オブジェクトを割り当てます。 アクティブなトランザクションを持つ接続上でコマンドが実行され、`Transaction` オブジェクトの `Transaction` プロパティに `Command` オブジェクトが割り当てられていない場合は、例外がスローされます。

3. 必要なコマンドを実行します。

4. <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> オブジェクトの <xref:Microsoft.Data.SqlClient.SqlTransaction> メソッドを呼び出してトランザクションを完了するか、<xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> メソッドを呼び出してトランザクションを終了します。 <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> または <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> メソッドが実行される前に接続が終了または破棄されると、トランザクションはロールバックされます。

次のコード例は、Microsoft SqlClient Data Provider for SQL Server を使用するトランザクション ロジックを示しています。  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>関連項目

- [トランザクションとコンカレンシー](transactions-and-concurrency.md)
- [分散トランザクション](distributed-transactions.md)
- [SQL Server と System.Transactions の統合](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
