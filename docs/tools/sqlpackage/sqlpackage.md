---
title: SqlPackage.exe
description: SqlPackage.exe を使用してデータベース開発タスクを自動化する方法について説明します。 例と使用可能なパラメーター、プロパティ、および SQLCMD 変数を表示します。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577898"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** は、次のデータベース開発タスクを自動化するコマンド ライン ユーティリティです。  
  
- [バージョン](#version):SqlPackage アプリケーションのビルド番号が返されます。  バージョン 18.6 で追加されました。

- [Extract](sqlpackage-extract.md): ライブ SQL Server または Azure SQL Database からデータベース スナップショット (.dacpac) ファイルを作成します。  
  
- [発行](sqlpackage-publish.md):ソース .dacpac ファイルのスキーマに合わせてデータベース スキーマの増分更新を行います。 データベースがサーバーに存在しない場合は、公開操作によって作成されます。 存在する場合は、既存のデータベースが更新されます。  
  
- [Export](sqlpackage-export.md):SQL Server または Azure SQL Database のライブ データベース (データベース スキーマとユーザー データを含む) を BACPAC パッケージ (.bacpac ファイル) にエクスポートします。  
  
- [Import](sqlpackage-import.md):BACPAC パッケージのスキーマとテーブル データを、SQL Server または Azure SQL Database インスタンス内の新しいユーザー データベースにインポートします。  
  
- [DeployReport](sqlpackage-deploy-drift-report.md): 公開操作によって行われる変更の XML レポートを作成します。  
  
- [DriftReport](sqlpackage-deploy-drift-report.md):登録されたデータベースに対して最終登録以降に行われた変更に関する XML レポートを作成します。  
  
- [Script](sqlpackage-script.md): ソースのスキーマに合わせてターゲットのスキーマを更新する、Transact-SQL の増分更新スクリプトを作成します。  
  
**SqlPackage.exe** コマンド ラインでは、これらの操作と共に、操作固有のパラメーターおよびプロパティを指定できます。  

**[最新バージョンをダウンロード](sqlpackage-download.md)** します。 最新リリースに関する詳細については、[リリース ノート](release-notes-sqlpackage.md)をご覧ください。
  
## <a name="command-line-syntax"></a>コマンド ライン構文

**SqlPackage.exe** は、コマンド ラインで指定されたパラメーター、プロパティ、および SQLCMD 変数を使用して指定された操作を開始します。  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>使用例

**SQL スクリプト出力で .dacpac ファイルを使用してデータベース間の比較を生成する**

まず、最新のデータベース変更の .dacpac ファイルを作成します。

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
(変更がない) データベース ターゲットの .dacpac ファイルを作成します。

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

2 つの .dacpac ファイルの相違点を生成する SQL スクリプトを作成します。

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Version

sqlpackage のバージョンをビルド番号として表示します。  対話型プロンプトだけでなく、[自動化されたパイプライン](sqlpackage-pipelines.md)でも使用できます。

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>終了コード

次の終了コードを返すコマンド:

- 0 = 成功
- 0 以外 = 失敗


## <a name="next-steps"></a>次のステップ

- [SqlPackage Extract](sqlpackage-extract.md) について詳しく学習する
- [SqlPackage Publish](sqlpackage-publish.md) について詳しく学習する
- [SqlPackage Export](sqlpackage-export.md) について詳しく学習する
- [SqlPackage Import](sqlpackage-import.md) について詳しく学習する
