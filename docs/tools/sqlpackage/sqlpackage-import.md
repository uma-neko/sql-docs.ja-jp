---
title: SqlPackage のインポート
description: SqlPackage.exe Import を使用してデータベース開発タスクを自動化する方法について説明します。 例と使用可能なパラメーター、プロパティ、および SQLCMD 変数を表示します。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 0e9acab737de04b002debf9d8c1b230a5cb01b14
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577914"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>SqlPackage Import のパラメーターとプロパティ
SqlPackage.exe の Import 操作を実行すると、BACPAC パッケージ (.bacpac ファイル) のスキーマとテーブル データが、SQL Server または Azure SQL Database の新規または空のデータベースにインポートされます。 既存のデータベースへのインポート操作時に、ターゲット データベースにユーザー定義のスキーマ オブジェクトを含めることはできません。  

## <a name="command-line-syntax"></a>コマンドライン構文

**SqlPackage.exe** は、コマンド ラインで指定されたパラメーター、プロパティ、および SQLCMD 変数を使用して指定された操作を開始します。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>Import 操作のパラメーター

|パラメーター|短い形式|値|説明|
|---|---|---|---|
|**/Action:**|**/a**|[インポート]|実行する操作を指定します。 |
|**/AccessToken:**|**/at**|{string}| ターゲット データベースに接続するときに使用するトークン ベースの認証アクセス トークンを指定します。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|診断ログがコンソールへの出力かどうかを指定します。 既定値は False です。 |
|**/DiagnosticsFile:**|**/df**|{string}|診断ログを保存するファイルを指定します。 |
|**/MaxParallelism:**|**/mp**|{int}| 1 つのデータベースに対して実行される同時実行操作の並列処理の次数を指定します。 既定値は 8 です。 |
|**/Properties:**|**/p**|{PropertyName}={Value}|操作固有のプロパティの名前と値のペア ({PropertyName}={Value}) を指定します。 操作のプロパティ名については、特定の操作のヘルプを参照してください。 例: sqlpackage.exe /Action:Import /?。|
|**/Quiet:**|**/q**|{True&#124;False}|詳細なフィードバックを非表示にするかどうかを指定します。 既定値は False です。|
|**/SourceFile:**|**/sf**|{string}|ソース ファイルを操作のソースとして使用するように指定します。 このパラメーターを使用した場合、他のソース パラメーターは無効になります。 |
|**/TargetConnectionString:**|**/tcs**|{string}|ターゲット データベースの有効な SQL Server または Azure 接続文字列を指定します。 このパラメーターを指定する場合、他のどのターゲット パラメーターとも同時には使用できません。 |
|**/TargetDatabaseName:**|**/tdn**|{string}|sqlpackage.exe の動作のターゲットとなるデータベースの名前のオーバーライドを指定します。 |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|ターゲット データベース接続に SQL 暗号化を使用するかどうかを指定します。 |
|**/TargetPassword:**|**/tp**|{string}|SQL Server 認証シナリオでは、ターゲット データベースへのアクセスに使用するパスワードを定義します。 |
|**/TargetServerName:**|**/tsn**|{string}|ターゲット データベースをホストしているサーバーの名前を定義します。 |
|**/TargetTimeout:**|**/tt**|{int}|ターゲット データベースへの接続を確立するためのタイムアウトを秒単位で指定します。 Azure AD の場合、この値は 30 秒以上にすることをお勧めします。|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|TLS を使用してターゲット データベースへの接続を暗号化し、証明書チェーンを検証せずに信頼を確認するかどうかを指定します。 |
|**/TargetUser:**|**/tu**|{string}|SQL Server 認証シナリオでは、ターゲット データベースへのアクセスに使用する SQL Server ユーザーを定義します。 |
|**/TenantId:**|**/tid**|{string}|Azure AD のテナント ID またはドメイン名を表します。 このオプションは、ゲスト、またはインポートされた Azure AD ユーザーと Microsoft アカウント (outlook.com、hotmail.com、live.com など) をサポートする必要があります。 このパラメーターを省略すると、Azure AD の既定のテナント ID が、認証済みユーザーがこの AD のネイティブ ユーザーであることを前提として使用されます。 ただし、この場合、ゲスト、またはこの Azure AD でホストされているインポートされたユーザーと Microsoft アカウントはサポートされず、操作は失敗します。 <br/> Active Directory ユニバーサル認証の詳細については、[SQL Database と Azure Synapse Analytics を使用したユニバーサル認証 (SSMS での MFA のサポート)](/azure/sql-database/sql-database-ssms-mfa-authentication) に関するページをご覧ください。|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|ユニバーサル認証を使用するかどうかを指定します。 True に設定すると、対話型認証プロトコルがアクティブになり、MFA がサポートされます。 このオプションは、ユーザーによるユーザー名とパスワードの入力を必要とする対話型プロトコルまたは統合認証 (Windows 資格情報) を使用した、MFA なしの Azure AD 認証に使用することもできます。 /UniversalAuthentication が True に設定されているときは、SourceConnectionString (scs) で Azure AD 認証を指定することはできません。 /UniversalAuthentication が False に設定されているときは、SourceConnectionString (scs) で Azure AD 認証を指定する必要があります。 <br/> Active Directory ユニバーサル認証の詳細については、[SQL Database と Azure Synapse Analytics を使用したユニバーサル認証 (SSMS での MFA のサポート)](/azure/sql-database/sql-database-ssms-mfa-authentication) に関するページをご覧ください。|

## <a name="properties-specific-to-the-import-action"></a>Import 操作に固有のプロパティ

|プロパティ|[値]|説明|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|SQL Server に対してクエリを実行するときのコマンドのタイムアウト (秒) を指定します。|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Azure SQL Database のエディションを定義します。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| SQLServer に対してクエリを実行するときのデータベース ロックのタイムアウトを秒単位で指定します。 無期限に待機するには、-1 を使用します。|
|**/p:**|DatabaseMaximumSize=(INT32)|Azure SQL Database の最大サイズを GB 単位で定義します。|
|**/p:**|DatabaseServiceObjective=(STRING)|Azure SQL Database のパフォーマンス レベル ("P0" や "S1" など) を定義します。|
|**/p:**|ImportContributorArguments=(STRING)|配置コントリビューターに配置コントリビューター引数を指定します。 複数の値を指定する場合は、セミコロンで区切ります。|
|**/p:**|ImportContributors=(STRING)|bacpac をインポートするときに実行する配置コントリビューターを指定します。 このとき、セミコロン区切りで、完全修飾ビルド コントリビューター名または ID を指定する必要があります。|
|**/p:**|ImportContributorPaths=(STRING)|追加の配置コントリビューターを読み込むためのパスを指定します。 複数の値を指定する場合は、セミコロンで区切ります。 |
|**/p:**|LongRunningCommandTimeout=(INT32)| SQL Server に対してクエリを実行するときの実行時間の長いコマンドのタイムアウトを秒単位で指定します。 無期限に待機するには、0 を使用します。|
|**/p:**|Storage=({File&#124;Memory})|データベース モデルの構築時に要素をどのように格納するかを指定します。 パフォーマンス上の理由から、既定値は InMemory です。 大規模なデータベースの場合は、File バックアップ ストレージが必要です。|
  

## <a name="next-steps"></a>次の手順

- [sqlpackage](sqlpackage.md) について詳しく学習する