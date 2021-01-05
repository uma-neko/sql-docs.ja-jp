---
title: SQL Server 用の Apache Spark コネクタ
description: SQL Server 用 Apache Spark コネクタの使用方法について説明します。
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 7450ebddf94a4378313bb1793bcefe34a88407a5
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442948"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Apache Spark コネクタ:SQL Server および Azure SQL

SQL Server と Azure SQL 用の Apache Spark コネクタは、パフォーマンスの高いコネクタであり、ビッグ データ分析でトランザクションデータを使用し、アドホック クエリやレポートの結果を保持できるようにします。 コネクタを使用すると、オンプレミスまたはクラウド内のあらゆる SQL データベースを、Spark ジョブの入力データ ソースまたは出力データ シンクとして使用できます。

このライブラリには、SQL Server と Azure SQL 用の Apache Spark コネクタのソース コードが含まれています。

[Apache Spark](https://spark.apache.org/) は、"大規模なデータ処理のための統合された分析エンジン" です。

Maven 座標 (`com.microsoft.azure:spark-mssql-connector:1.0.0`) を使用して、コネクタをプロジェクトにインポートできます。 また、ソースからコネクタを作成することも、GitHub のリリース セクションから jar をダウンロードすることもできます。 コネクタの最新情報については、[SQL Spark コネクタの GitHub リポジトリ](https://github.com/microsoft/sql-spark-connector)に関する記事をご覧ください。

## <a name="supported-features"></a>サポートされている機能

* すべての Spark バインド (Scala、Python、R) のサポート
* 基本認証と Active Directory (AD) のキー タブのサポート
* 並べ替えられた `dataframe` 書き込みのサポート
* SQL Server ビッグ データ クラスターでの SQL Server 単一インスタンスおよびデータ プールへの書き込みのサポート
* SQL Server 単一インスタンスに対する信頼性の高いコネクタのサポート

| コンポーネント                            | サポートされているバージョン              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5 (Spark 3.0 はサポートされていません) |
| Scala                                | 2.11                            |
| SQL Server 用 Microsoft JDBC ドライバー | 8.2                             |
| Microsoft SQL Server                 | SQL Server 2008 以降        |
| Azure SQL Databases                  | サポートされています                       |

> [!NOTE]
> このコネクタでの Azure Synapse Analytics の使用はテストされていません。 動作する場合もありますが、意図しない結果になる可能性があります。

### <a name="supported-options"></a>サポートされるオプション
SQL Server と Azure SQL 用の Apache Spark コネクタは、こちらで定義されているオプションをサポートしています:[SQL DataSource JDBC](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

さらに、次のオプションがサポートされています

| オプション | Default | 説明 |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` または `NO_DUPLICATES`。 `NO_DUPLICATES` を使用すると、実行プログラム再起動シナリオで、信頼性の高い挿入を実装できます |
| `dataPoolDataSource` | `none` | `none` は、値が設定されておらず、SQL Server の単一インスタンスへの書き込みにコネクタを使用する必要があることを意味します。 ビッグ データ クラスターのデータ プール テーブルに書き込むデータ ソース名に、この値を設定します|
| `isolationLevel` | `READ_COMMITTED` | 分離レベルを指定します |
| `tableLock` | `false` | 書き込みパフォーマンスを向上させるために、`TABLOCK` オプションと共に挿入を実装します |
| `schemaCheckEnabled` | `true` | false に設定されている場合は、データ フレームと sql テーブル スキーマの厳密なチェックを無効にします |

その他の[一括コピー オプション](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions)を、`dataframe` のオプションとして設定でき、書き込み時に `bulkcopy` API に渡されます

## <a name="performance-comparison"></a>パフォーマンスの比較

SQL Server および Azure SQL 用の Apache Spark コネクタは、SQL Server に書き込むための汎用 JDBC コネクタよりも最大 15 倍高速です。 パフォーマンス特性は、種類、データ量、使用するオプションによって異なり、実行ごとの変動が示される場合もあります。 次のパフォーマンス結果は、spark `dataframe` で 143.9M 行の SQL テーブルを上書きするために要した時間です。 spark `dataframe` は [spark TPCDS ベンチマーク](https://github.com/databricks/spark-sql-perf)を使用して生成された `store_sales` HDFS テーブルを読み取ることによって構築されます。 `dataframe` への `store_sales` の読み取り時間は除外されます。 結果は、3 回の実行を平均したものです。

| コネクタの種類 | オプション | 説明 |  書き込み時間 |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | Default | 既定のオプションを使用した汎用 JDBC コネクタ |  1385 秒 |
| `sql-spark-connector` | `BEST_EFFORT` | 既定のオプションを使用したベスト エフォート `sql-spark-connector` |580 秒 |
| `sql-spark-connector` | `NO_DUPLICATES ` | 信頼性の高い `sql-spark-connector` | 709 秒 |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | テーブル ロックを有効にしたベスト エフォート `sql-spark-connector` | 72 秒 |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| テーブル ロックを有効にした信頼性の高い `sql-spark-connector`| 198 秒 |

構成

- Spark 構成: num_executors = 20、executor_memory = '1664 m'、executor_cores = 2
- データ生成構成: scale_factor=50、partitioned_tables=true
- 143,997,590 行の nr を含むデータ ファイル `store_sales`

環境

- [SQL Server ビッグ データ クラスター](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 ノード
- 各ノード Gen 5 サーバー、512 GB RAM、4 TB NVM/ノード、NIC 10 GB

## <a name="commonly-faced-issues"></a>一般的に発生する問題

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

この問題は、Hadoop 環境で以前のバージョンの mssql ドライバー (現在このコネクタに含まれています) を使用した場合に発生します。 以前の Azure SQL コネクタを使用していて、Azure Active Directory の互換性を保つためにそのクラスターにドライバーを手動でインストールしている場合は、それらのドライバーを削除する必要があります。

この問題を解決する手順:

1. 汎用の Hadoop 環境を使用している場合は、mssql jar: `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar` を確認し、削除します。 Databricks を使用している場合は、グローバルまたはクラスターの init スクリプトを追加して、以前のバージョンの mssql ドライバーを `/databricks/jars` フォルダーから削除するか、または既存のスクリプトに `rm /databricks/jars/*mssql*` の行を追加します。
2. `adal4j` および `mssql` パッケージを追加します。 たとえば、Maven を使用することはできますが、どのような方法でも機能するはずです。 

   > [!CAUTION]
   > SQL spark コネクタをこのようにインストールしないでください。

1. 接続構成にドライバー クラスを追加します。 たとえば、次のように入力します。

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

詳細と説明については、[https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339) の解決策を参照してください。

## <a name="get-started"></a>開始するには

SQL Server および Azure SQL 用の Apache Spark コネクタは、Spark DataSourceV1 API と SQL Server Bulk API に基づいており、組み込みの JDBC Spark SQL コネクタと同じインターフェイスを使用します。 この統合により、書式設定パラメーターを `com.microsoft.sqlserver.jdbc.spark` で更新するだけで、コネクタを簡単に統合し、既存の Spark ジョブを移行させることができます。

このコネクタをプロジェクトに含めるには、このリポジトリをダウンロードし、SBT を使用して jar を構築します。

## <a name="write-to-a-new-sql-table"></a>新しい SQL テーブルに書き込む

> [!WARNING]
> `overwrite` モードでは、テーブルが既定でデータベースに既に存在する場合は、最初にテーブルが削除されます。 予期しないデータ損失を避けるため、このオプションは注意して使用してください。

モード `overwrite` を使用する場合、テーブルの再作成時にオプション `truncate` を使用しないと、インデックスは失われます。 これで列ストア テーブルがヒープになります。 既存のインデックス作成を維持する場合は、オプション `truncate` も値 true で指定してください。 たとえば、「 `.option("truncate","true")` 」のように入力します。

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

### <a name="append-to-sql-table"></a>SQL テーブルに追加する

```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>分離レベルを指定する

このコネクタは、データベースへの一括挿入を実行するときに、既定で `READ_COMMITTED` 分離レベルを使用します。 分離レベルをオーバーライドする場合は、次に示すように `mssqlIsolationLevel` オプションを使用します。

```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>SQL テーブルから読み取る

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="azure-active-directory-authentication"></a>Azure Active Directory 認証

### <a name="python-example-with-service-principal"></a>サービス プリンシパルを使用した Python の例

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Active Directory パスワードを使用した Python の例

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Active Directory を使用して認証するには、必要な依存関係がインストールされている必要があります。

**Scala** の場合、`_com.microsoft.aad.adal4j_` アーティファクトをインストールする必要があります。

**Python** の場合、`_adal_` ライブラリをインストールする必要があります。  これは、pip を介して利用できます。

例については、[サンプル ノートブック](https://github.com/microsoft/sql-spark-connector/tree/master/samples)を確認してください。

## <a name="support"></a>サポート

Azure SQL および SQL Server 用の Apache Spark コネクタは、オープンソース プロジェクトです。 このコネクタには、Microsoft サポートは付属していません。 コネクタに関する問題や質問については、このプロジェクト リポジトリに問題を作成してください。 コネクタ コミュニティはアクティブであり、送信を監視しています。

## <a name="next-steps"></a>次のステップ

[SQL Spark コネクタの GitHub リポジトリ](https://github.com/microsoft/sql-spark-connector)にアクセスします。

分離レベルについての詳細については、「[SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)」を参照してください。