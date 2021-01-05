---
title: SqlPackage のエクスポート
description: SqlPackage.exe Export を使用してデータベース開発タスクを自動化する方法について説明します。 例と使用可能なパラメーター、プロパティ、および SQLCMD 変数を表示します。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: c0c3b1462fe165678e3826585f5ce82d5945de56
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577922"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>SqlPackage Export のパラメーターとプロパティ
SqlPackage.exe の Export 操作を実行すると、SQL Server または Azure SQL のライブ データベースが BACPAC パッケージ (.bacpac ファイル) にエクスポートされます。 既定では、すべてのテーブルのデータが .bacpac ファイルに含まれます。 必要に応じて、データをエクスポートする対象のテーブルの一部のみを指定できます。 Export 操作の検証によって、エクスポート対象にテーブルの一部を指定した場合でも、完全な対象データベースに対する Azure SQL Database の互換性が維持されます。 

## <a name="command-line-syntax"></a>コマンドライン構文

**SqlPackage.exe** は、コマンド ラインで指定されたパラメーター、プロパティ、および SQLCMD 変数を使用して指定された操作を開始します。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>Export 操作のパラメーター

|パラメーター|短い形式|値|説明|
|---|---|---|---|
|**/Action:**|**/a**|エクスポート|実行する操作を指定します。 |
|**/AccessToken:**|**/at**|{string}| ターゲット データベースに接続するときに使用するトークン ベースの認証アクセス トークンを指定します。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|診断ログがコンソールへの出力かどうかを指定します。 既定値は False です。 |
|**/DiagnosticsFile:**|**/df**|{string}|診断ログを保存するファイルを指定します。 |
|**/MaxParallelism:**|**/mp**|{int}| 1 つのデータベースに対して実行される同時実行操作の並列処理の次数を指定します。 既定値は 8 です。 |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|sqlpackage.exe が既存のファイルを上書きするかどうかを指定します。 False を指定すると、既存のファイルが検出された場合に sqlpackage.exe の操作が中止します。 既定値は True です。 |
|**/Properties:**|**/p**|{PropertyName}={Value}|操作固有のプロパティの名前と値のペア ({PropertyName}={Value}) を指定します。 操作のプロパティ名については、特定の操作のヘルプを参照してください。 例: sqlpackage.exe /Action:Export /?。|
|**/Quiet:**|**/q**|{True&#124;False}|詳細なフィードバックを非表示にするかどうかを指定します。 既定値は False です。|
|**/SourceConnectionString:**|**/scs**|{string}|ソース データベースの有効な SQL Server または SQL Azure 接続文字列を指定します。 このパラメーターを指定する場合、他のどのソース パラメーターとも同時には使用できません。 |
|**/SourceDatabaseName:**|**/sdn**|{string}|ソース データベースの名前を定義します。 |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|ソース データベース接続に SQL 暗号化を使用するかどうかを指定します。 |
|**/SourcePassword:**|**/sp**|{string}|SQL Server 認証シナリオでは、ソース データベースへのアクセスに使用するパスワードを定義します。 |
|**/SourceServerName:**|**/ssn**|{string}|ソース データベースをホストしているサーバーの名前を定義します。 |
|**/SourceTimeout:**|**/st**|{int}|ソース データベースへの接続を確立する際のタイムアウトを秒単位で指定します。 |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|TLS を使用してソース データベースへの接続を暗号化し、証明書チェーンを検証せずに信頼を確認するかどうかを指定します。 |
|**/SourceUser:**|**/su**|{string}|SQL Server 認証シナリオでは、ソース データベースへのアクセスに使用する SQL Server ユーザーを定義します。 |
|**/TargetFile:**|**/tf**|{string}| データベースではなく、ターゲット ファイル (.dacpac ファイル) をアクションのターゲットとして使用するように指定します。 このパラメーターを使用した場合、他のターゲット パラメーターは無効になります。 このパラメーターは、データベース ターゲットのみをサポートするアクションに対しては無効です。|
|**/TenantId:**|**/tid**|{string}|Azure AD のテナント ID またはドメイン名を表します。 このオプションは、ゲスト、またはインポートされた Azure AD ユーザーと Microsoft アカウント (outlook.com、hotmail.com、live.com など) をサポートする必要があります。 このパラメーターを省略すると、Azure AD の既定のテナント ID が、認証済みユーザーがこの AD のネイティブ ユーザーであることを前提として使用されます。 ただし、この場合、ゲスト、またはこの Azure AD でホストされているインポートされたユーザーと Microsoft アカウントはサポートされず、操作は失敗します。 <br/> Active Directory ユニバーサル認証の詳細については、[SQL Database と Azure Synapse Analytics を使用したユニバーサル認証 (SSMS での MFA のサポート)](/azure/sql-database/sql-database-ssms-mfa-authentication) に関するページをご覧ください。|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|ユニバーサル認証を使用するかどうかを指定します。 True に設定すると、対話型認証プロトコルがアクティブになり、MFA がサポートされます。 このオプションは、ユーザーによるユーザー名とパスワードの入力を必要とする対話型プロトコルまたは統合認証 (Windows 資格情報) を使用した、MFA なしの Azure AD 認証に使用することもできます。 /UniversalAuthentication が True に設定されているときは、SourceConnectionString (scs) で Azure AD 認証を指定することはできません。 /UniversalAuthentication が False に設定されているときは、SourceConnectionString (scs) で Azure AD 認証を指定する必要があります。 <br/> Active Directory ユニバーサル認証の詳細については、[SQL Database と Azure Synapse Analytics を使用したユニバーサル認証 (SSMS での MFA のサポート)](/azure/sql-database/sql-database-ssms-mfa-authentication) に関するページをご覧ください。|

## <a name="properties-specific-to-the-export-action"></a>エクスポート アクションに固有のプロパティ

|プロパティ|[値]|説明|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|SQL Server に対してクエリを実行するときのコマンドのタイムアウト (秒) を指定します。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| SQLServer に対してクエリを実行するときのデータベース ロックのタイムアウトを秒単位で指定します。 無期限に待機するには、-1 を使用します。|
|**/p:**|LongRunningCommandTimeout=(INT32)| SQL Server に対してクエリを実行するときの実行時間の長いコマンドのタイムアウトを秒単位で指定します。 無期限に待機するには、0 を使用します。|
|**/p:**|Storage=({File&#124;Memory} 'File')|抽出時に使用されるスキーマ モデルのバックアップ用ストレージの種類を指定します。|
|**/p:**|TableData=(STRING)|データの抽出元テーブルを示します。 テーブル名は次の形式で指定してください: schema_name.table_identifier。名前部分はかっこで囲んでも囲まなくても構いません。 このオプションは、複数回指定できます。|
|**/p:**|TempDirectoryForTableData=(STRING)|パッケージ ファイルに書き込む前にテーブル データをバッファーするために使用する、一時ディレクトリを指定します。|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|予期されるターゲット エンジンのバージョンを指定します。 これにより、生成された bacpac で、メモリ最適化テーブルなどの V12 機能を持つ Azure SQL Database サーバーによってサポートされるオブジェクトを許可するかどうかが決まります。|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Microsoft Azure SQL Database v12 のサポートされているフルテキスト ドキュメントの種類を確認するかどうかを指定します。|

## <a name="next-steps"></a>次の手順

- [sqlpackage](sqlpackage.md) について詳しく学習する