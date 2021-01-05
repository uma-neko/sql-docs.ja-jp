---
title: SQL Server Management Studio (SSMS) を使用して Azure VM 上で SQL Server インスタンスに接続し、クエリを行う
description: SSMS を使用して Azure VM 上で SQL Server インスタンスに接続します。 SSMS で基本的な T-SQL クエリを実行して、Azure VM 上で SQL Server を作成し、クエリを実行します。
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 29c39caf6885ee974c62ed153df982b435c72c95
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619324"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>クイックスタート: SQL Server Management Studio (SSMS) を使用して Azure 仮想マシン上で SQL Server インスタンスに接続し、クエリを行う

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

SQL Server Management Studio (SSMS) の使用を開始して Azure 仮想マシン上で SQL Server インスタンスに接続し、いくつかの Transact-SQL (T-SQL) コマンドを実行します。

> [!div class="checklist"]
> - SQL Server インスタンスに接続する
> - データベースを作成する
> - 新しいデータベースにテーブルを作成する
> - 新しいテーブルに行を挿入する
> - 新しいテーブルにクエリを行って結果を表示する
> - クエリ ウィンドウのテーブルを使用して接続のプロパティを確認する
## <a name="prerequisites"></a>前提条件

この記事を完了するには、SQL Server Management Studio と、データ ソースへのアクセスが必要です。

- [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md) をインストールします。
- [Azure VM 上の SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE)。

## <a name="connect-to-sql-virtual-machines"></a>SQL 仮想マシンに接続する

次の手順では、ご使用の Azure VM の省略可能な DNS ラベルを作成し、SQL Server Management Studio (SSMS) と接続する方法を示します。

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>パブリック IP アドレスの DNS ラベルの構成

インターネットから SQL Server データベース エンジンに接続する場合は、パブリック IP アドレスの DNS ラベルを作成することを検討してください。 IP アドレスで参加することはできますが、DNS ラベルによって、識別が容易で、基になるパブリック IP アドレスを抽象化する A レコードが作成されます。

> [!NOTE]
> 同じ Virtual Network 内の SQL Server インスタンスのみに接続する場合、またはローカルでのみ接続する予定の場合は、DNS ラベルは必要ありません。

1. DNS ラベルを作成するには、ポータルで **[Virtual Machines]** を選びます。 SQL Server VM を選択し、そのプロパティを表示します。

2. 仮想マシンの概要で、目的の **パブリック IP アドレス** を選択します。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="パブリック IP アドレス":::

3. パブリック IP アドレスのプロパティで、 **[構成]** を展開します。

4. DNS ラベル名を入力します。 この名前は、IP アドレスではなく名前で SQL Server VM に直接接続するために使用できる A レコードです。

5. **[保存]** ボタンを選択します。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="DNS ラベル":::

### <a name="connect"></a>接続する

1. SQL Server Management Studio を起動します。 SSMS を初めて実行すると、**[サーバーへの接続]** ウィンドウが開きます。 開かない場合は、 **[オブジェクト エクスプローラー]**  >  **[接続]**  >  **[データベース エンジン]** の順に選択して、手動で開くことができます。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="オブジェクト エクスプローラーの接続リンク":::

2. **[サーバーに接続]** ダイアログ ボックスが表示されます。 次の情報を入力します。

    |   設定   |   推奨値   |   説明   |
    |--------------|-----------------------|-----------------|
    | **サーバーの種類** | データベース エンジン | **[サーバーの種類]** に、**[データベース エンジン]** (通常は既定のオプションです) を選択します。 |
    | **サーバー名** | 完全修飾サーバー名 | **[サーバー名]** に、Azure SQL VM の名前を入力します。 Azure SQL VM の IP アドレスを使用して接続することもできます。 | 
    | **認証** | SQL Server 認証 | Azure SQL VM と接続するには、**SQL Server 認証** を使用します。 また、Azure Active Directory 環境のセットアップがある場合は、任意の Azure Active Directory オプションを使用できます。 </br> </br> **Windows 認証** の方法は、Azure SQL VM ではサポートされていません。 詳細については、[Azure SQL 認証](/azure/sql-database/sql-database-security-overview#access-management)に関するページを参照してください。|
    | **Login** | サーバー アカウントのユーザー ID | サーバーを作成するために使用するサーバー アカウントのユーザー ID。 **SQL Server 認証** を使用する場合は、ログインが必要です。 |
    | **パスワード** | サーバー アカウントのパスワード | サーバーを作成するために使用するサーバー アカウントのパスワード。 **SQL Server 認証** を使用する場合は、パスワードが必要です。 |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="SQL 仮想マシンのサーバー名フィールド":::

3. すべてのフィールドを入力したら、**[接続]** を選択します。

    **[オプション]** を選択して追加の接続オプションを変更することもできます。 接続オプションの例には、接続しているデータベース、接続のタイムアウト値、ネットワーク プロトコルなどがあります。 この記事では、すべてのオプションについて既定値を使用します。

4. Azure VM 上の SQL Server が正常に完了したことを確認するには、**オブジェクト エクスプローラー** 内のオブジェクトを展開して探索します。ここには、サーバー名、SQL Server バージョン、およびユーザー名が表示されます。 これらのオブジェクトは、サーバーの種類によって異なります。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Azure SQL VM の接続":::

## <a name="troubleshoot-connectivity-issues"></a>接続の問題のトラブルシューティング

ポータルには接続性を自動的に構成するオプションが用意されていますが、接続性を手動で構成する方法も知っておくと便利です。 要件を把握しておくと、トラブルシューティングにも役立ちます。

Azure VM 上の SQL Server に接続するための要件を次の表に示します。

| 要件 | 説明 |
|---|---|
| [SQL Server 認証モードを有効にする](/sql/database-engine/configure-windows/change-server-authentication-mode#use-ssms) | 仮想ネットワーク上で Active Directory を構成済みである場合を除き、VM にリモート接続するには SQL Server 認証が必要になります。 |
| [SQL ログインを作成する](/sql/relational-databases/security/authentication-access/create-a-login) | SQL 認証を使用する場合、実際のターゲット データベースへのアクセス許可も持つ、ユーザー名とパスワードによる SQL ログインが必要です。 |
| TCP/IP プロトコルを有効にする | SQL Server では、TCP 経由の接続を許可する必要があります。 |
| [SQL Server ポートのファイアウォール規則を有効にする](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | VM 上のファイアウォールでは、SQL Server ポート (既定では 1433) の受信トラフィックを許可する必要があります。 |
| [TCP 1433 のネットワーク セキュリティ グループの規則を作成する](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#create-a-security-rule) | インターネット経由で接続する場合は、VM が SQL Server ポート (既定では 1433) でトラフィックを受信できるようにします。 これは、ローカルと仮想ネットワーク専用の接続では不要です。 この手順は Azure portal でのみ必要です。 |

> [!TIP]
> ポータル内で接続を構成すると、上記の表の手順が自動的に実行されます。 これらの手順を使用するのは、ご自分の構成を確認する場合または SQL Server の接続を手動で設定する場合だけです。

## <a name="create-a-database"></a>データベースを作成する

次の手順で、TutorialDB という名前のデータベースを作成します。

1. オブジェクト エクスプローラーでサーバー インスタンスを右クリックして、**[新しいクエリ]** を選択します。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="[新しいクエリ] のリンク":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="[実行] コマンド":::
  
    クエリが完了すると、オブジェクト エクスプローラーのデータベースの一覧に新しい TutorialDB データベースが表示されます。 表示されない場合は、**[データベース]** ノードを右クリックして **[更新]** を選択します。

## <a name="create-a-table-in-the-new-database"></a>新しいデータベースにテーブルを作成する

このセクションでは、新しく作成した TutorialDB データベースにテーブルを作成します。 クエリ エディターがまだ *マスター* データベースのコンテキストにあるため、次の手順で接続コンテキストを *TutorialDB* データベースに切り替えます。

1. 次に示すように、[データベース] ドロップダウン リストで、目的のデータベースを選択します。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="データベースの変更":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="新しいテーブル":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="[結果] 一覧":::

    以下のオプションのいずれかを選択して、結果の表示方法を変更することもできます。

   ![クエリの結果を表示するための 3 つのオプション](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - 1 番目のボタンを選ぶと、結果は **テキスト ビュー** で表示されます。次のセクションの図はこの表示方法です。
   - 中央のボタンを選ぶと、結果は **グリッド ビュー** で表示されます。これが、既定のオプションです。
       - これが既定値として設定されています。
   - 3 番目のボタンでは、結果がファイルに保存されます。ファイルの既定の拡張子は .rpt です。

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>クエリ ウィンドウのテーブルを使用して接続のプロパティを確認する

接続プロパティに関する情報は、クエリの結果の下で確認できます。 前の手順で示したクエリを実行した後、クエリ ウィンドウの下端にある接続のプロパティを確認します。

- 接続先のサーバーとデータベースと使用しているユーザー名がわかります。
- クエリの実行時間と、前に実行したクエリによって返された行数も確認できます。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="接続のプロパティ":::

## <a name="additional-tools"></a>その他のツール

[Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) を使用して、[SQL Server](../../azure-data-studio/quickstart-sql-server.md)、[Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md)、および [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md) に接続し、クエリを実行することもできます。

## <a name="next-steps"></a>次のステップ

SSMS に慣れ親しむには、実践的な経験を積むのが最も効果的です。 以下の記事は、SSMS 内で使用できるさまざまな機能を使用するのに役立ちます。

- [SQL Server Management Studio (SSMS) クエリ エディター](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [スクリプトの作成](../tutorials/scripting-ssms.md)
- [SSMS でテンプレートを使用する](../template/templates-ssms.md)
- [SSMS を構成する](../tutorials/ssms-configuration.md)
- [SSMS を使用するための追加のヒントとテクニック](../tutorials/ssms-tricks.md)