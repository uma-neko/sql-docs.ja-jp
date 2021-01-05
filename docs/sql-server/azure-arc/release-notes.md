---
title: Azure Arc 対応 SQL Server - リリース ノート
description: 最新のリリース ノート
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 10/29/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 3dcbc6b17d5abe87aabe923d70746d65595b9de6
ms.sourcegitcommit: dc3ea1696b8a4332934568439aed6cce4e9737eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93244654"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>リリース ノート ‐ Azure Arc 対応 SQL Server (プレビュー)

> [!NOTE]
> この記事で紹介しているテクノロジはプレビュー機能であり、「[Microsoft Azure プレビューの追加利用規約](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)」に従うことを条件として提供されます。

## <a name="september-2020"></a>2020 年 9 月

Azure Arc 対応 SQL Server は、パブリック プレビューとしてリリースされています。 Azure Arc 対応 SQL Server は、Azure 外のお客様のデータセンター、エッジ、またはマルチクラウド環境にホストされている SQL Server インスタンスに Azure サービスを拡張します。

詳細については、[Azure Arc 対応 SQL Server の概要](overview.md)に関するページを参照してください。

### <a name="known-issues"></a>既知の問題

9 月リリースには次の問題があります。

* **[Register Azure Arc enabled SQL Server]** \(Azure Arc 対応 SQL Server の登録\) ブレードで、カスタム タグの構成がサポートされていません。 カスタム タグを追加するには、登録後に **[SQL Server - Azure Arc]** \(SQL Server - Azure Arc\) リソースを開き、 **[概要]** ページでタグを変更します。

* SQL Server インスタンスを Azure Arc に接続するには、広範な一連の権限があるアカウントが必要です。 詳細については、「[必要なアクセス許可](overview.md#required-permissions)」を参照してください。

## <a name="october-2020"></a>2020 年 10 月

10 月の更新プログラムには、次の強化機能が含まれています。

* [Register Azure Arc enabled SQL Server]\(Azure Arc 対応 SQL Server の登録\) ブレードに **[タグ]** タブが含まれるようになりました。タグは登録スクリプトに含まれており、 **[SQL Server - Azure Arc]** に反映されます。 詳細については、「[SQL Server を Azure Arc に接続する](connect.md)」を参照してください。

* **[Environment Health]** \(環境の正常性\) からの入力で、 *CustomScriptExtension* を展開して、ポータルから **SQL Assessment** をアクティブ化できるようになりました。 詳細については、[SQL Assessment の構成](assess.md#run-on-demand-sql-assessment)に関するページを参照してください。

### <a name="known-issues"></a>既知の問題

10 月リリースには次の問題があります。

* SQL Server インスタンスを Azure Arc に接続するには、広範な一連の権限があるアカウントが必要です。 詳細については、「[必要なアクセス許可](overview.md#required-permissions)」を参照してください。

## <a name="next-steps"></a>次のステップ

**試してみたい場合**  [Azure Arc 対応 SQL Server のジャンプスタート](https://aka.ms/AzureArcSqlServerJumpstart)を使用すると、すばやく開始できます。
