---
title: 分散トランザクション
description: ADO.NET で分散トランザクションを実行する方法について説明します
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559234"
---
# <a name="distributed-transactions"></a>分散トランザクション

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

トランザクションとは、たとえば、1 つの単位として成功 (コミット) または失敗 (アボート) する関連タスク セットです。 "*分散トランザクション*" は、複数のリソースに影響するトランザクションです。 分散トランザクションがコミットされるためには、すべての参加要素が、すべてのデータ変更が永久的な変更となることを保証する必要があります。 システム クラッシュその他の予期しない出来事が発生した場合でも、変更は保持されます。 1 つの参加要素がこの保証に失敗しただけでも、トランザクション全体が失敗し、トランザクションのスコープ内のデータに対する変更がロールバックされます。 

> [!NOTE]
> トランザクションがアクティブであるときに `DataReader` が開始された場合、トランザクションをコミットまたはロールバックしようとすると例外がスローされます。

## <a name="working-with-systemtransactions"></a>System.Transactions の操作

.NET では、分散トランザクションは <xref:System.Transactions> 名前空間の API によって管理されます。 複数の永続的なリソース マネージャーが関係する場合、<xref:System.Transactions> API は分散トランザクション処理を Microsoft Distributed Transaction Coordinator (MS DTC) などのトランザクション モニターに委任します。 詳しくは、「[トランザクションの基礎](/dotnet/framework/data/transactions/transaction-fundamentals)」をご覧ください。

ADO.NET 2.0 では、`EnlistTransaction` メソッドを使用した分散トランザクションへの参加のサポートが導入されました。これにより、接続を <xref:System.Transactions.Transaction> インスタンスに参加させることができます。 ADO.NET の以前のバージョンでは、分散トランザクションへの明示的な参加は、接続の `EnlistDistributedTransaction` メソッドを使用して実行されていました。これによって <xref:System.EnterpriseServices.ITransaction> インスタンス内の接続を参加させることで、下位互換性を得ることができます。 Enterprise Services トランザクションについて詳しくは、「[Enterprise Services および COM+ トランザクションとの相互運用性](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions)」をご覧ください。

Microsoft SqlClient Data Provider for SQL Server で SQL Server データベースに対して <xref:System.Transactions> トランザクションを使用する場合は、軽量な <xref:System.Transactions.Transaction> が自動的に使用されます。 トランザクションは、必要に応じて完全な分散トランザクションに昇格させることができます。 詳細については、「[SQL Server と System.Transactions の統合](system-transactions-integration-with-sql-server.md)」を参照してください。

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>分散トランザクションへの自動参加

自動参加は、ADO.NET 接続を `System.Transactions` に統合する場合の既定の (推奨される) 方法です。 接続オブジェクトは、トランザクションがアクティブであると判定されると、既存の分散トランザクションに自動的に参加します。トランザクションがアクティブであることは、`System.Transaction` から見ると、`Transaction.Current` が null でないことを意味します。 自動トランザクション参加は、接続が開かれた場合に行われます。 その後は、コマンドがトランザクションのスコープ内で実行された場合でも、自動トランザクション参加は行われません。 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> の接続文字列パラメーターとして `Enlist=false` を指定することで既存のトランザクションで自動登録を無効にできます。

## <a name="manually-enlisting-in-a-distributed-transaction"></a>分散トランザクションへの手動登録

自動登録が無効になっているか、接続の確立後に開始されたトランザクションを登録する必要がある場合、Microsoft SqlClient Data Provider for SQL Server の <xref:Microsoft.Data.SqlClient.SqlConnection> オブジェクトの `EnlistTransaction` メソッドを利用し、既存の分散トランザクションに登録できます。 既存の分散トランザクションに参加すると、トランザクションがコミットまたはロールバックされた場合、データ ソースに対してコードで行った変更もコミットまたはロールバックされます。

分散トランザクションへの参加は、特にビジネス オブジェクトをプールする場合に適切です。 ビジネス オブジェクトが、開かれている接続と共にプールされている場合は、その接続を開くときだけ、自動トランザクション参加が行われます。 プールされているビジネス オブジェクトを使用して複数のトランザクションが実行される場合、そのオブジェクトの開かれている接続は、新しく開始されるトランザクションには自動参加しません。 この場合には、接続の自動トランザクション参加を無効にし、`EnlistTransaction` を使用して、接続をトランザクションに参加させることができます。

`EnlistTransaction` は、既存のトランザクションを参照する <xref:System.Transactions.Transaction> 型の引数を 1 つ受け取ります。 接続の `EnlistTransaction` メソッドを呼び出した後、接続を使用してデータ ソースに対して行ったすべての変更は、トランザクションに組み込まれます。 null 値を渡すことで、現在の分散トランザクションから接続の参加が解除されます。 この接続は、`EnlistTransaction` を呼び出す前に開く必要があることに注意してください。

> [!NOTE]
> 一度接続を明示的にトランザクションに参加させると、最初のトランザクションが終了するまでは、参加を解除したり別のトランザクションに参加させたりすることはできません。

> [!CAUTION]
> 接続の `EnlistTransaction` メソッドを使用してトランザクションが既に開始していた場合、<xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> は例外をスローします。 ただし、トランザクションがデータ ソースで開始されたローカル トランザクションである場合 (たとえば <xref:Microsoft.Data.SqlClient.SqlCommand> を使用して BEGIN TRANSACTION ステートメントを明示的に実行した場合)、`EnlistTransaction` はローカル トランザクションをロールバックし、要求されたように既存の分散トランザクションに参加します。 ローカル トランザクションがロールバックされたことは通知されないため、<xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> を使用して開始したのではないローカル トランザクションについては、自分で管理する必要があります。 Microsoft SqlClient Data Provider for SQL Server を SQL Server で使用する場合、登録しようとすると例外がスローされます。 その他の場合については検出されません。  

## <a name="promotable-transactions-in-sql-server"></a>SQL Server で昇格可能なトランザクション

SQL Server は、軽量のローカル トランザクションを必要に応じて分散トランザクションに自動的に昇格できる、昇格可能なトランザクションをサポートしています。 昇格可能なトランザクションは、必要な場合以外、分散トランザクションのオーバーヘッドの増加を引き起こすことはありません。 詳細とコード サンプルについては、「[SQL Server と System.Transactions の統合](system-transactions-integration-with-sql-server.md)」を参照してください。

## <a name="configuring-distributed-transactions"></a>分散トランザクションの設定

 分散トランザクションを使用するには、ネットワーク上の MS DTC を有効にする必要があります。 Windows ファイアウォールを有効にしている場合は、MS DTC サービスでネットワークを使用するか MS DTC ポートを開くことができるようにする必要があります。  
  
## <a name="see-also"></a>関連項目

- [トランザクションとコンカレンシー](transactions-and-concurrency.md)
- [SQL Server と System.Transactions の統合](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
