---
title: Azure Arc 対応 SQL Server - リリース ノート
description: 最新のリリース ノート
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 4d3890a29905057eb800fac823d27f149adb2ac0
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559254"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>リリース ノート ‐ Azure Arc 対応 SQL Server (プレビュー)

> [!NOTE]
> この記事で紹介しているテクノロジはプレビュー機能であり、「[Microsoft Azure プレビューの追加利用規約](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)」に従うことを条件として提供されます。

## <a name="december-2020"></a>2020 年 12 月

### <a name="breaking-change"></a>互換性に影響する変更点

このリリースでは、`Microsoft.AzureArcData` という名称の更新された[リソース プロバイダー](/azure/azure-resource-manager/management/azure-services-resource-providers)が導入されました。 Azure Arc 対応 SQL Server を引き続き利用するには、このリソース プロバイダーを先に登録する必要があります。 リソース プロバイダーの登録手順については、「[前提条件](connect.md#prerequisites)」セクションを参照してください。

既存の SQL Server - Azure Arc リソースがある場合、次の手順で Microsoft.AzureArcData 名前空間に移行します。

1. [Cloud Shell](https://shell.azure.com/) を起動します。 詳細については、[Cloud Shell の PowerShell](https://aka.ms/pscloudshell/docs) に関するページを参照してください。

2. 次のコマンドを使用してスクリプトをシェルにアップロードします。

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. スクリプトを実行します。  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - シェルにコマンドを貼り付けるには、Windows の場合は `Ctrl-Shift-V` を、MacOS の場合は `Cmd-v` を使用します。
> - このスクリプトは、Cloud Shell セッションに関連付けられているホーム フォルダーに直接アップロードされます。
> - 移行が完了すると、スクリプトによってリソース グループ名の入力が求められ、メッセージが出力されます。

### <a name="other-changes"></a>その他の変更点

* **SQL Server - Azure Arc** リソースの種類の *TCPPorts* プロパティは、*TCPStaticPorts* に名前が変更されました。
* 必要なアクセス許可は、以前ほど広範囲ではありません。 詳しくは、「[必要なアクセス許可](overview.md#required-permissions)」セクションをご覧ください。

### <a name="known-issues"></a>既知の問題

* AzureArcData 名前空間で、**SQL Server - Azure Arc** リソースを含め、新しく作成されたリソースに *CreateTime* プロパティが追加されません。

## <a name="october-2020"></a>2020 年 10 月

10 月の更新プログラムには、次の強化機能が含まれています。

* [Register Azure Arc enabled SQL Server]\(Azure Arc 対応 SQL Server の登録\) ブレードに **[タグ]** タブが含まれるようになりました。タグは登録スクリプトに含まれており、 **[SQL Server - Azure Arc]** に反映されます。 詳細については、「[SQL Server を Azure Arc に接続する](connect.md)」を参照してください。

* **[Environment Health]** \(環境の正常性\) からの入力で、*CustomScriptExtension* を展開して、ポータルから **SQL Assessment** をアクティブ化できるようになりました。 詳細については、[SQL Assessment の構成](assess.md#run-on-demand-sql-assessment)に関するページを参照してください。

### <a name="known-issues"></a>既知の問題

10 月リリースには次の問題があります。

* SQL Server インスタンスを Azure Arc に接続するには、広範な一連の権限があるアカウントが必要です。 詳細については、「[必要なアクセス許可](overview.md#required-permissions)」を参照してください。

## <a name="september-2020"></a>2020 年 9 月

Azure Arc 対応 SQL Server は、パブリック プレビューとしてリリースされています。 Azure Arc 対応 SQL Server は、Azure 外のお客様のデータセンター、エッジ、またはマルチクラウド環境にホストされている SQL Server インスタンスに Azure サービスを拡張します。

詳細については、[Azure Arc 対応 SQL Server の概要](overview.md)に関するページを参照してください。

### <a name="known-issues"></a>既知の問題

9 月リリースには次の問題があります。

* **[Register Azure Arc enabled SQL Server]** \(Azure Arc 対応 SQL Server の登録\) ブレードで、カスタム タグの構成がサポートされていません。 カスタム タグを追加するには、登録後に **[SQL Server - Azure Arc]** \(SQL Server - Azure Arc\) リソースを開き、 **[概要]** ページでタグを変更します。

* SQL Server インスタンスを Azure Arc に接続するには、広範な一連の権限があるアカウントが必要です。 詳細については、「[必要なアクセス許可](overview.md#required-permissions)」を参照してください。

## <a name="next-steps"></a>次のステップ

**試してみたい場合**  [Azure Arc 対応 SQL Server のジャンプスタート](https://aka.ms/AzureArcSqlServerJumpstart)を使用すると、すばやく開始できます。
