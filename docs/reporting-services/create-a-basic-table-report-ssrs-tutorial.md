---
title: 基本的なテーブル レポートの作成 (SSRS チュートリアル) | Microsoft Docs
description: Visual Studio/SQL Server Data Tools (SSDT) のレポート デザイナー ツールを使用して、SQL Server Reporting Services (SSRS) のページ分割されたレポートを作成します。
ms.date: 04/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- walkthroughs [Reporting Services]
- tutorials [Reporting Services]
- reports [Reporting Services], creating
ms.assetid: 3b539b4b-26f2-4c0b-b506-80f175679a46
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f1549d3ba775d598902fd0fd4b5cc33bab2f54de
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87245141"
---
# <a name="create-a-basic-table-report-ssrs-tutorial"></a>基本的なテーブル レポートの作成 (SSRS チュートリアル)

このチュートリアルでは、*レポート デザイナー* ツールを Visual Studio または SQL Server Data Tools (SSDT) で使用します。 SQL Server Reporting Services (SSRS) のページ分割されたレポートを作成します。 レポートには、AdventureWorks2016 データベースのデータから作成されたクエリ テーブルが含まれます。

このチュートリアルを進めるにつれ、次の内容を学習することになります。
  
- レポート プロジェクトを作成する。
- データ接続を設定する。
- クエリを定義する。
- テーブル データ領域を追加する。
- レポートを書式設定する。
- フィールドをグループ化し合計する。
- レポートをプレビューする。
- レポートをパブリッシュする (任意)。

## <a name="requirements"></a>必要条件

このチュートリアルを行うには、システムに次のコンポーネントがインストールされている必要があります。

- [!INCLUDE[msconame-md](../includes/msconame-md.md)] SQL Server データベース エンジン。  
- SQL Server 2016 Reporting Services 以降 (SSRS)。
- AdventureWorks2016 データベース。  詳細については、「[AdventureWorks Sample Databases](https://github.com/Microsoft/sql-server-samples/releases)」 (AdventureWorks サンプル データベース) を参照してください。
- *レポート デザイナー*にアクセスできるようにするには、Visual Studio 向けの [SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md) を Reporting Services 拡張機能とともにインストールする必要があります。
  
AdventureWorks2016 データベースからデータを取得するには読み取り専用権限も必要です。

**このチュートリアルの推定所要時間:** 30 分。

## <a name="next-steps"></a>次のステップ

[レッスン 1:レポート サーバー プロジェクトの作成 &#40;Reporting Services&#41;](lesson-1-creating-a-report-server-project-reporting-services.md)

[レッスン 2:接続情報の指定 &#40;Reporting Services&#41;](lesson-2-specifying-connection-information-reporting-services.md)

[レッスン 3:テーブル レポートのデータセットの定義 &#40;Reporting Services&#41;](lesson-3-defining-a-dataset-for-the-table-report-reporting-services.md)

[レッスン 4:レポートへのテーブルの追加 &#40;Reporting Services&#41;](lesson-4-adding-a-table-to-the-report-reporting-services.md)

[レッスン 5: レポートの書式設定 &#40;Reporting Services&#41;](lesson-5-formatting-a-report-reporting-services.md)

[レッスン 6:グループと合計の追加 &#40;Reporting Services&#41;](lesson-6-adding-grouping-and-totals-reporting-services.md)

## <a name="see-also"></a>関連項目

[Reporting Services チュートリアル](reporting-services-tutorials-ssrs.md) その他のご不明な点は、 [Reporting Services のフォーラムに質問してみてください](https://go.microsoft.com/fwlink/?LinkId=620231)
