---
title: SQL Server Management Studio (SSMS) を使用して Azure Synapse Analytics の専用 SQL プールに接続し、クエリを実行する
description: SSMS を使用して、Azure Synapse Analytics の専用 SQL プールに接続します。 SSMS で基本的な T-SQL クエリを実行することで、Azure Synapse Analytics の専用 SQL プールを作成し、クエリを実行します。
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 102eaff45b6cdfb89a159e3de77039c8735395e2
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619294"
---
# <a name="quickstart-connect-and-query-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>クイックスタート: SQL Server Management Studio (SSMS) を使用して Azure Synapse Analytics の専用 SQL プールに接続し、クエリを実行する

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

SQL Server Management Studio (SSMS) の使用を開始して Azure Synapse Analytics の専用 SQL プールに接続し、いくつかの Transact-SQL (T-SQL) コマンドを実行します。

> [!div class="checklist"]
> - Azure Synapse Analytics の専用 SQL プールに接続する
> - データベースを作成する
> - 新しいデータベースにテーブルを作成する
> - 新しいテーブルに行を挿入する
> - 新しいテーブルにクエリを行って結果を表示する
> - クエリ ウィンドウのテーブルを使用して接続のプロパティを確認する

## <a name="prerequisites"></a>前提条件

この記事を完了するには、SQL Server Management Studio と、データ ソースへのアクセスが必要です。

- [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md) をインストールします。
- [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE)

## <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Azure Synapse Analytics の専用 SQL プールに接続する

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. SQL Server Management Studio を起動します。 SSMS を初めて実行すると、**[サーバーへの接続]** ウィンドウが開きます。 開かない場合は、 **[オブジェクト エクスプローラー]**  >  **[接続]**  >  **[データベース エンジン]** の順に選択して、手動で開くことができます。

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-object-explorer.png" alt-text="オブジェクト エクスプローラーの接続リンク":::

2. **[サーバーへの接続]** ウィンドウで、下の一覧どおりに進めます。

    |   設定   |   推奨値   |   説明   |
    |-------------|------------------------|-----------------|
    | **サーバーの種類** | データベース エンジン | **[サーバーの種類]** に、**[データベース エンジン]** (通常は既定のオプションです) を選択します。 |
    | **サーバー名** | 完全修飾サーバー名 | **[サーバー名]** には、Azure Synapse Analytics サーバーの名前を入力します。 |
    | **認証** | SQL Server 認証 | Azure Synapse Analytics と接続するには、**SQL Server 認証** を使用します。 </br> </br> Azure SQL では、**Windows 認証** 方法はサポートされていません。 詳細については、[Azure SQL 認証](/azure/sql-database/sql-database-security-overview#access-management)に関するページを参照してください。 |
    | **Login** | サーバー アカウントのユーザー ID | サーバーを作成するために使用するサーバー アカウントのユーザー ID。 |
    | **パスワード** | サーバー アカウントのパスワード | サーバーを作成するために使用するサーバー アカウントのパスワード。 |

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-to-azure-synapse-analytics-object-explorer.png" alt-text="Azure Synapse Analytics の [サーバー名] フィールド":::

3. すべてのフィールドを入力したら、**[接続]** を選択します。

    **[オプション]** を選択して追加の接続オプションを変更することもできます。 接続オプションの例には、接続しているデータベース、接続のタイムアウト値、ネットワーク プロトコルなどがあります。 この記事では、すべてのオプションについて既定値を使用します。

    ファイアウォールをまだ設定していない場合は、ファイアウォールを構成するように求めるプロンプトが表示されます。 サインインしたら、自分の Azure アカウントのログイン情報を入力し、引き続きファイアウォール規則を設定します。 **[OK]** をクリックします。 このプロンプトは、1 回限りの操作です。 ファイアウォールを構成したら、ファイアウォールのプロンプトは表示されなくなります。

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Azure SQL の新しいファイアウォール規則":::

4. Azure Synapse Analytics が正常に接続されたことを確認するには、**オブジェクト エクスプローラー** 内のオブジェクトを展開して探索します。ここには、サーバー名、SQL Server バージョン、およびユーザー名が表示されます。 これらのオブジェクトは、サーバーの種類によって異なります。

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-azure-synapse-analytics.png" alt-text="Azure Synapse Analytics データベースへの接続":::

## <a name="troubleshoot-connectivity-issues"></a>接続の問題のトラブルシューティング

Azure Synapse Analytics で接続の問題が発生する可能性があります。 接続の問題のトラブルシューティングの詳細については、[接続の問題のトラブルシューティング](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues)に関するページを参照してください。

Azure SQL Database または Azure SQL Managed Instance とやりとりする際に発生する接続および一時的なエラーを回避、トラブルシューティング、診断、および軽減することができます。 詳細については、[一時的な接続エラーのトラブルシューティング](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-connectivity-issues)に関するページを参照してください。

## <a name="create-a-database"></a>データベースを作成する

ここでは、次の手順に従って、TutorialDB という名前のデータベースを作成します。

1. オブジェクト エクスプローラーでサーバー インスタンスを右クリックして、**[新しいクエリ]** を選択します。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-query.png" alt-text="[新しいクエリ] のリンク":::

2. クエリ ウィンドウに次の T-SQL コード スニペットを貼り付けます。

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. **[実行]** を選択するか、お使いのキーボードの F5 キーを選択し、クエリを実行します。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/execute.png" alt-text="[実行] コマンド":::
  
    クエリが完了すると、オブジェクト エクスプローラーのデータベースの一覧に新しい TutorialDB データベースが表示されます。 表示されない場合は、**[データベース]** ノードを右クリックして **[更新]** を選択します。

## <a name="create-a-table-in-the-new-database"></a>新しいデータベースにテーブルを作成する

このセクションでは、新しく作成した TutorialDB データベースにテーブルを作成します。 クエリ エディターがまだ *マスター* データベースのコンテキストにあるため、次の手順で接続コンテキストを *TutorialDB* データベースに切り替えます。

1. 次に示すように、[データベース] ドロップダウン リストで、目的のデータベースを選択します。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/change-db.png" alt-text="データベースの変更":::

2. クエリ ウィンドウに次の T-SQL コード スニペットを貼り付けます。

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. **[実行]** を選択するか、お使いのキーボードの F5 キーを選択し、クエリを実行します。

クエリが完了すると、オブジェクト エクスプローラーのテーブルの一覧に新しい Customers テーブルが表示されます。 テーブルが表示されない場合は、オブジェクト エクスプローラーで **[TutorialDB]**  >  **[テーブル]** ノードを右クリックした後、 **[更新]** を選択します。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-table.png" alt-text="新しいテーブル":::

## <a name="insert-rows-into-the-new-table"></a>新しいテーブルに行を挿入する

ここでは、作成した Customers テーブルにいくつかの行を挿入します。 クエリ ウィンドウに次の T-SQL コード スニペットを貼り付けて、**[実行]** を選択します。

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>テーブルのクエリを行って結果を表示する

クエリの結果は、クエリ テキスト ウィンドウの下に表示されます。 Customers テーブルに対しクエリを実行し、挿入した行を表示するには、次の手順に従います。

1. クエリ ウィンドウに次の T-SQL コード スニペットを貼り付けて、**[実行]** を選択します。

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    クエリの結果は、テキストを入力した領域の下に表示されます。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/query-results.png" alt-text="[結果] 一覧":::

    以下のオプションのいずれかを選択して、結果の表示方法を変更することもできます。

   ![クエリの結果を表示するための 3 つのオプション](media/ssms-connect-query-azure-synapse-analytics/results.png)

   - 1 番目のボタンを選ぶと、結果は **テキスト ビュー** で表示されます。次のセクションの図はこの表示方法です。
   - 中央のボタンを選ぶと、結果は **グリッド ビュー** で表示されます。これが、既定のオプションです。
       - これが既定値として設定されています。
   - 3 番目のボタンでは、結果がファイルに保存されます。ファイルの既定の拡張子は .rpt です。

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>クエリ ウィンドウのテーブルを使用して接続のプロパティを確認する

接続プロパティに関する情報は、クエリの結果の下で確認できます。 前の手順で示したクエリを実行した後、クエリ ウィンドウの下端にある接続のプロパティを確認します。

- 接続先のサーバーとデータベースと使用しているユーザー名がわかります。
- クエリの実行時間と、前に実行したクエリによって返された行数も確認できます。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connection-properties.png" alt-text="接続のプロパティ":::

## <a name="additional-tools"></a>その他のツール

[Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) を使用して、[SQL Server](../../azure-data-studio/quickstart-sql-server.md)、[Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md)、および [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md) に接続し、クエリを実行することもできます。

## <a name="next-steps"></a>次のステップ

SSMS に慣れ親しむには、実践的な経験を積むのが最も効果的です。 以下の記事は、SSMS 内で使用できるさまざまな機能を使用するのに役立ちます。

- [SQL Server Management Studio (SSMS) クエリ エディター](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [スクリプトの作成](../tutorials/scripting-ssms.md)
- [SSMS でテンプレートを使用する](../template/templates-ssms.md)
- [SSMS を構成する](../tutorials/ssms-configuration.md)
- [SSMS を使用するための追加のヒントとテクニック](../tutorials/ssms-tricks.md)