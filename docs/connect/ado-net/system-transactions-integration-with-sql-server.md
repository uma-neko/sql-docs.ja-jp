---
title: SQL Server と System.Transactions の統合
description: 分散トランザクションを使用するための SQL Server と System.Transactions の統合について説明します。
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: af0ba2865719a5388314a4ca695e09191cb56173
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051317"
---
# <a name="systemtransactions-integration-with-sql-server"></a>SQL Server と System.Transactions の統合

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET には、<xref:System.Transactions> 名前空間を介してアクセスできるトランザクション フレームワークが含まれています。 このフレームワークにより、ADO.NET を含む .NET に完全に統合された方法でトランザクションが公開されます。  
  
プログラミング上の強化に加えて、<xref:System.Transactions> と ADO.NET の連係により、トランザクション処理が最適化されます。 昇格可能なトランザクションとは、必要に応じて完全な分散トランザクションに自動的に昇格する、軽量の (ローカル) トランザクションです。

SQL Server を使用すると、Microsoft SqlClient Data Provider for SQL Server で昇格可能なトランザクションがサポートされます。 昇格可能なトランザクションは、必要な場合以外、分散トランザクションのオーバーヘッドの増加を引き起こすことはありません。 昇格可能なトランザクションは自動的に処理され、開発者による介入は必要ありません。

## <a name="creating-promotable-transactions"></a>昇格可能なトランザクションの作成

Microsoft SqlClient Data Provider for SQL Server では昇格可能なトランザクションをサポートしており、<xref:System.Transactions> 名前空間内のクラスを介して処理されます。 昇格可能なトランザクションでは、必要が生じるまで分散トランザクションの作成を延期することで、分散トランザクションが最適化されます。 必要なリソース マネージャーが 1 つだけである場合は、分散トランザクションは発生しません。

> [!NOTE]
> 部分信頼のシナリオで分散トランザクションに昇格するには、 <xref:System.Transactions.DistributedTransactionPermission> が必要です。

## <a name="promotable-transaction-scenarios"></a>昇格可能なトランザクションのシナリオ

分散トランザクションは一般的にシステム リソースを大量に消費するため、トランザクションでアクセスされるすべてのリソース マネージャーを統合する、Microsoft Distributed Transaction Coordinator (MS DTC) で管理されます。 昇格可能なトランザクションは特殊な形式の <xref:System.Transactions> トランザクションで、単純な SQL Server トランザクションに処理を効果的に委任できます。 <xref:System.Transactions>、 <xref:Microsoft.Data.SqlClient>、および SQL Server では、トランザクションの処理に関連する作業が調整され、必要に応じて、トランザクションは完全な分散トランザクションに昇格されます。

昇格可能なトランザクションを使用する利点は、アクティブな <xref:System.Transactions.TransactionScope> トランザクションによって接続が開かれ、その他の接続が開いていない場合に、完全な分散トランザクションによるオーバーヘッドが生じることなく、トランザクションが軽量なトランザクションとしてコミットされることです。

### <a name="connection-string-keywords"></a>接続文字列キーワード

<xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> プロパティではキーワード `Enlist`がサポートされており、 <xref:Microsoft.Data.SqlClient> によってトランザクションのコンテキストが検出され、分散トランザクションに接続が自動的に参加するかどうかが示されます。 `Enlist=true`である場合は、開いているスレッドの現在のトランザクション コンテキストに接続が自動的に参加します。 `Enlist=false`である場合、 `SqlClient` 接続は分散トランザクションとは対話しません。 `Enlist` の既定値は true です。 `Enlist` が接続文字列で指定されていない場合、接続が開いたときに分散トランザクションがあればそれに自動的に参加します。

参加する `Transaction Binding` トランザクションと接続の関連付けは、 <xref:Microsoft.Data.SqlClient.SqlConnection> 接続文字列の `System.Transactions` キーワードによって制御されます。 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> の <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>プロパティを介して利用することもできます。

次の表で使用可能な値について説明します。
  
|キーワード|説明|  
|-------------|-----------------|  
|Implicit Unbind|これが既定値です。 完了時に接続がトランザクションからデタッチされ、自動コミット モードに切り替わります。|
|Explicit Unbind|トランザクションが閉じられるまで、接続がトランザクションにアタッチされたままになります。 関連付けられているトランザクションがアクティブでない場合や、 <xref:System.Transactions.Transaction.Current%2A>と一致しない場合、接続は失敗します。|

## <a name="using-transactionscope"></a>TransactionScope の使用

<xref:System.Transactions.TransactionScope> クラスでは、接続を分散トランザクションに暗黙的に参加させることで、コード ブロックがトランザクションのコード ブロックになります。 <xref:System.Transactions.TransactionScope.Complete%2A> ブロックの末尾では、終了する前に <xref:System.Transactions.TransactionScope> メソッドを呼び出す必要があります。 ブロックを終了すると、 <xref:System.Transactions.TransactionScope.Dispose%2A> メソッドが呼び出されます。 例外がスローされてコードがスコープから外れている場合は、トランザクションがアボートされたと見なされます。

使用中のブロックを終了したときに `using` オブジェクトで <xref:System.Transactions.TransactionScope.Dispose%2A> が呼び出されるように、 <xref:System.Transactions.TransactionScope> ブロックを使用することをお勧めします。 保留中のトランザクションのコミットまたはロールバックに失敗すると、 <xref:System.Transactions.TransactionScope> の既定のタイムアウトが 1 分であるため、パフォーマンスが大幅に低下する場合があります。 `using` ステートメントを使用しない場合は、すべての処理を `Try` ブロックで実行し、 <xref:System.Transactions.TransactionScope.Dispose%2A> メソッドを `Finally` ブロック内で明示的に呼び出す必要があります。

<xref:System.Transactions.TransactionScope>内で例外が発生した場合、そのトランザクションは矛盾しているとしてマークされ、破棄されます。 トランザクションは、 <xref:System.Transactions.TransactionScope> が破棄されるとロールバックされます。 例外が発生しない場合は、参加しているトランザクションがコミットされます。

> [!NOTE]
> 既定では、 `TransactionScope` クラスは、 <xref:System.Transactions.Transaction.IsolationLevel%2A> が `Serializable` であるトランザクションを作成します。 使用しているアプリケーションによっては、アプリケーション内での競合を回避するために、分離レベルを低下させる必要がある場合があります。

> [!NOTE]
> データベース リソースを大量に消費するため、分散トランザクション内で実行するのは更新、挿入、削除だけにすることをお勧めします。 Select ステートメントを使用すると、データベース リソースが不必要にロックされることがあり、シナリオによっては選択にトランザクションを使用する必要がある場合があります。 処理済みのその他のリソース マネージャーに関係する場合以外、データベース以外の処理はトランザクションのスコープ外で実行する必要があります。
トランザクションのスコープ内で例外が発生するとトランザクションのコミットが防止されますが、 <xref:System.Transactions.TransactionScope> クラスでは、作成したコードによってトランザクションのスコープ外で行われた変更はロールバックされません。 トランザクションがロールバックされたときに何らかの動作を行うようにする場合は、 <xref:System.Transactions.IEnlistmentNotification> インターフェイスを独自に実装し、トランザクションに明示的に参加する必要があります。

## <a name="example"></a>例

<xref:System.Transactions> を使用する場合は、System.Transactions.dll への参照が必要になります。

次の関数は、 <xref:Microsoft.Data.SqlClient.SqlConnection> ブロックでラップされている、異なる 2 つの <xref:System.Transactions.TransactionScope> オブジェクトで表された異なる 2 つの SQL Server インスタンスに対する、昇格可能なトランザクションを作成する方法を示しています。

次のコードは、<xref:System.Transactions.TransactionScope> ステートメントを使用して `using` ブロックを作成し、最初の接続を開きます。これにより、トランザクションが <xref:System.Transactions.TransactionScope> に自動参加します。

このトランザクションは、完全な分散トランザクションとしてではなく、最初に軽量のトランザクションとして参加します。 2 つ目の接続は、1 つ目の接続のコマンドで例外がスローされなかった場合にのみ、 <xref:System.Transactions.TransactionScope> に参加します。 2 つ目の接続が開かれると、トランザクションが完全な分散トランザクションに自動的に昇格されます。

その後、<xref:System.Transactions.TransactionScope.Complete%2A> メソッドが呼び出され、例外がスローされていない場合にのみ、トランザクションがコミットされます。 <xref:System.Transactions.TransactionScope> ブロックの任意の場所で例外がスローされると、 `Complete` が呼び出されず、 <xref:System.Transactions.TransactionScope> ブロックの最後で `using` が破棄されたときに分散トランザクションがロールバックされます。

[!code-csharp[SqlTransactionScope#1](~/../sqlclient/doc/samples/SqlTransactionScope.cs#1)]

## <a name="see-also"></a>関連項目

- [トランザクションとコンカレンシー](transactions-and-concurrency.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
