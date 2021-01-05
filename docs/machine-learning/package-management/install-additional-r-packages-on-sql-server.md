---
title: sqlmlutils を使用して R パッケージをインストールする
description: qlmlutils を使用して、新しい Python パッケージを、SQL Server Machine Learning Services のインスタンスにインストールする方法について説明します。
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9db282708c8f2e9bbd4ee44d45bac0b0d25dc5b9
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617561"
---
# <a name="install-r-packages-with-sqlmlutils"></a>sqlmlutils を使用して R パッケージをインストールする

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
この記事では、[**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) パッケージの関数を使用して、[SQL Server 上の Machine Learning Services](../sql-server-machine-learning-services.md) および[ビッグ データ クラスター](../../big-data-cluster/machine-learning-services.md)のインスタンスに R パッケージをインストールする方法について説明します。 インストールするパッケージは、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL ステートメントを使用してデータベース内で実行されている R スクリプトで使用できます。

> [!NOTE]
> この記事で説明されている **sqlmlutils** パッケージは SQL Server 2019 以降で R パッケージを追加するために使用されます。 SQL Server 2017 以前の場合は、「[R ツールを使用してパッケージをインストールする](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true)」を参照してください。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
この記事では、[**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) パッケージの関数を使用して、[Azure SQL Managed Instance の Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) のインスタンスに R パッケージをインストールする方法について説明します。 インストールするパッケージは、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL ステートメントを使用してデータベース内で実行されている R スクリプトで使用できます。
::: moniker-end

## <a name="prerequisites"></a>前提条件

- [R](https://www.r-project.org) と [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) を、SQL Server への接続に使用するクライアント コンピューターに インストールします。 スクリプトの実行には任意の R IDE を使用できますが、この記事では RStudio を想定しています。

  クライアント コンピューター上の R のバージョンは、サーバー上の R のバージョンと一致している必要があります。また、インストールするパッケージは、お持ちの R のバージョンに準拠している必要があります。
  SQL Server バージョンごとに含まれている R のバージョンの詳細については、「[Python および R のバージョン](../sql-server-machine-learning-services.md#versions)」を参照してください。
  
  特定の SQL Server の R のバージョンを確認するには、次の T-SQL コマンドを使用します。

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- SQL Server への接続に使用するクライアント コンピューターに [Azure Data Studio](../../azure-data-studio/what-is.md) をインストールします。 他のデータベース管理ツールまたはクエリ ツールも使用できますが、この記事では Azure Data Studio を想定しています。

### <a name="other-considerations"></a>その他の考慮事項

- パッケージのインストールは、**sqlmlutils** に渡す接続情報で指定する SQL インスタンス、データベース、ユーザーに固有のものです。 パッケージを複数の SQL インスタンスまたはデータベースで使用する場合や、別のユーザーに対してパッケージを使用する場合は、それぞれにパッケージをインストールする必要があります。 例外として、`dbo` のメンバーによってパッケージがインストールされた場合、そのパッケージは *パブリック* であり、すべてのユーザーと共有することができます。 ユーザーがパブリック パッケージの新しいバージョンをインストールした場合、パブリック パッケージは影響を受けませんが、そのユーザーは新しいバージョンにアクセスできます。

- SQL Server で実行されている R スクリプトでは、既定のインスタンス ライブラリにインストールされているパッケージのみを使用できます。 SQL Server では、外部ライブラリからパッケージを読み込むことはできません。これには同じコンピューター上にある外部ライブラリや、 他の Microsoft 製品と共にインストールされた R ライブラリも含まれます。

- 強化された SQL Server 環境では、次のパッケージを避けることをお勧めします。
  - ネットワーク アクセスを必要とするパッケージ
  - 管理者特権でのファイル システム アクセスが必要なパッケージ
  - Web 開発、または SQL Server 内で実行しても効果のないタスクに使用されるパッケージ

## <a name="install-sqlmlutils-on-the-client-computer"></a>sqlmlutils をクライアント コンピューターにインストールする

**sqlmlutils** を使用するには、最初に sqlmlutils を、SQL Server への接続に使用するクライアント コンピューターにインストールする必要があります。

**sqlmlutils** パッケージは **odbc** パッケージに依存し、**odbc** は他の複数のパッケージに依存しています。 次の手順では、これらのパッケージすべてが正しい順序でインストールされます。

### <a name="install-sqlmlutils-online"></a>sqlmlutils をオンラインでインストールする

クライアント コンピューターがインターネットにアクセスされている場合は、**sqlmlutils** とその依存パッケージをオンラインでダウンロードしてインストールできます。

1. 最新の **sqlmlutils** ファイル (Windows の場合は `.zip`、Linux の場合は `.tar.gz`) を https://github.com/Microsoft/sqlmlutils/tree/master/R/dist からクライアント コンピューターにダウンロードします。 このファイルは展開しないでください。

1. **コマンド プロンプト** を開き、次のコマンドを実行して、パッケージ **odbc** および **sqlmlutils** をインストールします。 ダウンロードした **sqlmlutils** ファイルのパスに置き換えます。 **odbc** パッケージはオンラインで検索され、インストールされます。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>sqlmlutils をオフラインでインストールする

クライアント コンピューターがインターネットに接続されていない場合は、インターネットにアクセスできるコンピューターを使用して、パッケージ **odbc** と **sqlmlutils** を事前にダウンロードしておく必要があります。 その後、ファイルをクライアント コンピューターのフォルダーにコピーし、パッケージをオフラインでインストールできます。

**odbc** パッケージは多数のパッケージに依存しており、パッケージのすべての依存関係を特定するのは困難です。 [**miniCRAN**](https://andrie.github.io/miniCRAN/) を使用して、すべての依存パッケージが含まれるパッケージ用のローカル リポジトリ フォルダーを作成することをお勧めします。
詳細については、「[miniCRAN を使用してローカル R パッケージ リポジトリを作成する](create-a-local-package-repository-using-minicran.md)」を参照してください。

**sqlmlutils** パッケージは、クライアント コンピューターにコピーしてインストールできる 1 つのファイルです。

インターネットに接続されているコンピューターでの操作:

1. **miniCRAN** をインストールします。 詳細については、「[miniCRAN をインストールする](create-a-local-package-repository-using-minicran.md#install-minicran)」を参照してください。

1. RStudio で、次の R スクリプトを実行して、**odbc** パッケージのローカル リポジトリを作成します。 この例では、リポジトリがフォルダー `odbc` に作成されることを前提としています。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   `Rversion` 値については、SQL Server にインストールされている R のバージョンを使用します。 インストールされているバージョンを確認するには、次の T-SQL コマンドを使用します。

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. 最新の **sqlmlutils** ファイル (Windows の場合は `.zip`、Linux の場合は `.tar.gz`) を [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) からダウンロードします。 このファイルは展開しないでください。

1. **odbc** リポジトリ フォルダーと **sqlmlutils** ファイル全体をクライアント コンピューターにコピーします。

SQL Server への接続に使用するクライアント コンピューターで、次の操作を実行します。

1. コマンド プロンプトを開きます。

1. 次のコマンドを実行して、**odbc**、**sqlmlutils** を順にインストールします。 このコンピューターにコピーした **odbc** リポジトリ フォルダーと **sqlmlutils** ファイルの完全なパスに置き換えます。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>SQL Server で R パッケージを追加する

次の例では、[**glue**](https://cran.r-project.org/web/packages/glue/) パッケージを SQL Server に追加します。

### <a name="add-the-package-online"></a>パッケージをオンラインで追加する

SQL Server への接続に使用するクライアント コンピューターがインターネットにアクセスできる場合は、インターネット経由で **sqlmlutils** を使って **glue** パッケージおよびすべての依存関係を検索し、そのパッケージを SQL Server インスタンスにリモートでインストールできます。

1. クライアント コンピューターで RStudio を開き、新しい **R スクリプト** ファイルを作成します。

1. 次の R スクリプトを使用して、**sqlmlutils** を使って **glue** パッケージをインストールします。 実際の SQL Server データベースの接続情報に置き換えてください。

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > **scope** には、**PUBLIC** または **PRIVATE** を指定できます。 パブリック スコープは、全ユーザーが使用できるパッケージをデータベース管理者がインストールする場合に適しています。 プライベート スコープを指定すると、パッケージを使用できるのは、そのパッケージをインストールしたユーザーだけになります。 スコープを指定しなかった場合の既定のスコープは **PRIVATE** です。

### <a name="add-the-package-offline"></a>パッケージをオフラインで追加する

クライアント コンピューターがインターネットに接続されていない場合は、インターネットにアクセスできるコンピューターを使用して、**miniCRAN** を使って **glue** パッケージをダウンロードできます。 次に、パッケージをオフラインでインストールできるクライアント コンピューターにパッケージをコピーします。
**miniCRAN** のインストールについては、「[miniCRAN をインストールする](create-a-local-package-repository-using-minicran.md#install-minicran)」を参照してください。

インターネットに接続されているコンピューターでの操作:

1. 次の R スクリプトを実行して、**glue** のローカル リポジトリ を作成します。 この例では、`c:\downloads\glue` にリポジトリ フォルダーが作成されます。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   `Rversion` 値については、SQL Server にインストールされている R のバージョンを使用します。 インストールされているバージョンを確認するには、次の T-SQL コマンドを使用します。

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. **glue** リポジトリ フォルダー全体 (`c:\downloads\glue`) をクライアント コンピューターにコピーします。 たとえば、`c:\temp\packages\glue` フォルダーにコピーします。

クライアント コンピューターでの操作:

1. RStudio を開いて、新しい **R Script** ファイルを作成します。

1. 次の R スクリプトを使用して、**sqlmlutils** を使って **glue** パッケージをインストールします。 ご自身の SQL Server データベース接続情報に置き換えます (Windows 認証を使用しない場合は、`uid` パラメーターと `pwd` パラメーターを追加します)。

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > **scope** には、**PUBLIC** または **PRIVATE** を指定できます。 パブリック スコープは、全ユーザーが使用できるパッケージをデータベース管理者がインストールする場合に適しています。 プライベート スコープを指定すると、パッケージを使用できるのは、そのパッケージをインストールしたユーザーだけになります。 スコープを指定しなかった場合の既定のスコープは **PRIVATE** です。

## <a name="use-the-package"></a>パッケージを使用する

**glue** パッケージがインストールされたら、T-SQL の **sp_execute_external_script** コマンドを使用して、SQL Server の R スクリプトでそのパッケージを使用できます。

1. Azure Data Studio を開き、ご自身の SQL Server データベースに接続します。

1. 次のコマンドを実行します。

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **結果**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>パッケージを削除する

**glue** パッケージを削除する場合は、次の R スクリプトを実行します。 先ほど定義したものと同じ **接続** 変数を使用します。

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>その他の sqlmlutils 関数

**sqlmlutils** パッケージには、R パッケージを管理するためと、SQL Server でストアド プロシージャやクエリを作成、管理、実行するための関数が多数含まれています。 詳細については、[sqlmlutils R の README ファイル](https://github.com/microsoft/sqlmlutils/tree/master/R)を参照してください。

**sqlmlutils** 関数の詳細については、R の **help** 関数または **?** を使用してださい。 演算子にすることはできません。 次に例を示します。

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>次のステップ

- インストール済み R パッケージの詳細については、「[R パッケージ情報の取得](r-package-information.md)」を参照してください
- R パッケージの操作情報については、「[R パッケージを使用するためのヒント](tips-for-using-r-packages.md)」を参照してください
- Python パッケージのインストールの詳細については、[pip を使用した Python パッケージのインストール](install-additional-python-packages-on-sql-server.md)に関するページをご覧ください
- SQL Server Machine Learning Services の詳細については、「[SQL Server Machine Learning Services とは (Python と R)](../sql-server-machine-learning-services.md)」を参照してください
