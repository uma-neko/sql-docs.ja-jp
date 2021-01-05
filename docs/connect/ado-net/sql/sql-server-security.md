---
title: SQL Server のセキュリティ
description: SQL Server のセキュリティ機能の概要と、SQL Server を対象とした安全な ADO.NET アプリケーションを作成するためのアプリケーション シナリオについて説明します。
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: ea2f8a65cb8ca16efd625309f73d7dccc3e27505
ms.sourcegitcommit: fe5c45a492e19a320a1a36b037704bf132dffd51
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2020
ms.locfileid: "80911668"
---
# <a name="sql-server-security"></a>SQL Server のセキュリティ

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server には、安全なデータベース アプリケーションの作成を支援するさまざまな機能が備わっています。  
  
データの盗難や破壊など、セキュリティに関する基本的な考慮事項は、使用している SQL Server のバージョンに関係なく当てはまります。 データの整合性も、セキュリティの問題と見なす必要があります。 データが保護されていない場合、アドホック データ操作が許可されていて、そのデータが誤って、または悪意を持って正しくない値に変更されたり、完全に削除されたりすると、そのデータは使い物にならなくなる可能性があります。 また、機密情報を適切に保存するなど、遵守しなければならない法的要件が存在することもよくあります。 特定の管轄区域に適用される法律によっては、特定の種類の個人データを保存することが完全に禁止されています。  
  
SQL Server の各バージョンにはそれぞれ異なるセキュリティ機能があり、Windows と同様、新しいバージョンほど機能が強化されています。 セキュリティ機能だけでは、データベース アプリケーションの安全性が保証されないことを理解しておくことが重要です。 データベース アプリケーションの要件、実行環境、デプロイ モデル、物理的な場所、およびユーザー数は、それぞれ独特です。 スコープ内のローカル アプリケーションの中には最小限のセキュリティしか必要としないものもありますが、その他のローカル アプリケーションや、インターネット上で展開されたアプリケーションについては、厳しいセキュリティ対策と継続的な監視と評価が必要な場合もあります。  
  
SQL Server データベース アプリケーションのセキュリティ要件は、事後的対策としてではなく設計時に考慮することが大切です。 開発サイクルの早い段階で脅威を評価すれば、脆弱性が検出されたときの損害の可能性を軽減することができます。  
  
アプリケーションの初期設計が適切であっても、システムの進化に伴って新しい脅威が発生する可能性があります。 データベースに対して複数の防御線を張ることで、セキュリティ侵害による被害を最小限に抑えることができます。 まずは必要以上の権限を決して付与しないことです。これにより攻撃対象の領域が少なくなります。  
  
このセクションの各トピックでは、開発者に関係のある SQL Server のセキュリティ機能を簡単に説明します。SQL Server オンライン ブックの関連項目へのリンクのほか、より掘り下げて解説したリソースへのリンクも示しています。  
  
## <a name="in-this-section"></a>このセクションの内容  
[SQL Server での認証](authentication-sql-server.md)  
SQL Server でのログインと認証について説明し、その他のリソースへのリンクを提供します。 
  
[SQL Server におけるアプリケーション セキュリティのシナリオ](application-security-scenarios-sql-server.md)  
ADO.NET および SQL Server アプリケーションに該当するさまざまなセキュリティ シナリオを取り上げます。  
  
[SQL Server Express のセキュリティ](sql-server-express-security.md)  
SQL Server Express のセキュリティ上の考慮事項を説明します。  
  
## <a name="related-sections"></a>関連項目  
[SQL Server データベース エンジンと Azure SQL Database のセキュリティ センター](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
SQL Server と Azure SQL Database のセキュリティ上の考慮事項について説明します。

[SQL Server インストールにおけるセキュリティの考慮事項](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
SQL Server をインストールする前に考慮すべきセキュリティの問題について説明します。

## <a name="next-steps"></a>次のステップ
- [SQL Server と ADO.NET](index.md)
