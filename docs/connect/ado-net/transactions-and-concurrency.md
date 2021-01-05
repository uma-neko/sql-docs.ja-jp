---
title: トランザクションとコンカレンシー
description: トランザクションとコンカレンシーで Microsoft SqlClient Data Provider for SQL Server を使用する方法について説明します。
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a30a3d3184c411fc0b54c0e26330a8fb138ffdae
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051302"
---
# <a name="transactions-and-concurrency"></a>トランザクションとコンカレンシー

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

トランザクションは、単一のコマンド、またはパッケージとして実行されるコマンドのグループで構成されます。 トランザクションを使用することで、複数の操作を 1 つの作業単位にまとめることができます。 トランザクションのあるポイントで障害が発生した場合は、トランザクションが開始される前の状態にすべての更新をロールバックできます。

データの一貫性を保証するために、トランザクションは ACID プロパティ (原始性、一貫性、分離性、および持続性) に準拠する必要があります。 Microsoft SQL Server など、ほとんどのリレーショナル データベース システムでは、クライアント アプリケーションが更新、挿入、または削除の操作を行うたびに、ロック、ログ、およびトランザクション管理の機能を提供し、トランザクションをサポートします。

> [!NOTE]
> 複数のリソースがかかわるトランザクションで、ロックがあまりにも長く保持されると、コンカレンシー数が少なくなる場合があります。 そのため、トランザクションはできるだけ短くします。  

トランザクションに、同じデータベースまたはサーバーの複数のテーブルが含まれている場合、一般的にストアド プロシージャ内の明示的トランザクションの方がパフォーマンスが向上します。 SQL Server のストアド プロシージャにトランザクションを作成するには、Transact-SQL ステートメントの `BEGIN TRANSACTION`、`COMMIT TRANSACTION`、および `ROLLBACK TRANSACTION` を使用します。 詳細については、SQL Server オンライン ブックを参照してください。

SQL Server と Oracle 間のトランザクションなど、種類の異なるリソース マネージャーがトランザクションに含まれる場合、分散トランザクションが必要になります。

## <a name="in-this-section"></a>このセクションの内容

 [ローカル トランザクション](local-transactions.md)  
 データベースに対してトランザクションを実行する方法を示します。  
  
 [分散トランザクション](distributed-transactions.md)  
 ADO.NET で分散トランザクションを実行する方法について説明します。  
  
 [SQL Server と System.Transactions の統合](system-transactions-integration-with-sql-server.md)  
 分散トランザクションを使用するための、SQL Server での <xref:System.Transactions> の統合について説明します。  
  
 [オプティミスティック同時実行制御](optimistic-concurrency.md) オプティミスティックとペシミスティックの同時実行制御について説明し、コンカレンシー違反をテストする方法について説明します。  

## <a name="see-also"></a>関連項目

- [トランザクションの基礎](/dotnet/framework/data/transactions/transaction-fundamentals)
- [データ ソースへの接続](connecting-to-data-source.md)
- [コマンドおよびパラメーター](commands-parameters.md)
- [DataAdapter と DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
