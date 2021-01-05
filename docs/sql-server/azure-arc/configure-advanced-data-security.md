---
title: Advanced Data Security の構成
titleSuffix: Azure Arc
description: Azure Arc が有効な SQL Server インスタンスの高度なデータ セキュリティを構成する
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2bd589ebacd9ea35e15881eaaeb022d4f2302986
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988028"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Azure Arc が有効な SQL Server インスタンスの高度なデータ セキュリティを構成する

次の手順に従って、オンプレミスの SQL Server インスタンスの高度なデータ セキュリティを有効にすることができます。

## <a name="prerequisites"></a>前提条件

* SQL Server インスタンスが Arc 対応の SQL Server にオンボードされていること。 次の手順に従って、[SQL Server インスタンスを Arc 対応の SQL Server にオンボード](connect.md)します。

* ご自分のユーザー アカウントに [Security Center ロール (RBAC)](/azure/security-center/security-center-permissions) の 1 つが割り当てられていること

## <a name="create-a-log-analytics-workspace"></a>Log Analytics ワークスペースを作成する

1. __Log Analytics ワークスペース__ リソースの種類を検索し、作成ブレードを使用して新しいものを追加します。

   ![新しいワークスペースの作成](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > Log Analytics ワークスペースはどのリージョンでも使用できます。したがって、既にある場合は、それを使用できます。 ただし、__マシン-Azure Arc__ リソースが作成されているのと同じリージョンに作成することをお勧めします。

1. Log Analytics ワークスペース リソースの概要ページにアクセスし、[Windows、Linux、およびその他のソース] を選択します。 後で使用するために、ワークスペース ID と主キーをコピーします。

   ![Log Analytics ワークスペース ブレード](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Microsoft Monitoring Agent (MMA) のインストール

次の手順は、リモート マシンで MMA エージェントをまだ構成していない場合にのみ必要です。

1. SQL Server インスタンスがインストールされている仮想または物理サーバーに対して__マシン - Azure Arc__ リソースを選択し、 **[拡張機能]** 機能を使用して拡張機能 __Microsoft Monitoring Agent - Azure Arc__ を追加します。 Log Analytics ワークスペースの構成を求められたら、前の手順で保存したワークスペース ID と主キーを使用します。

   ![MMA のインストール](media/configure-advanced-data-security/install-mma-extension.png)

1. 検証が成功したら、 **[作成]**  をクリックして、MMA Arc 拡張機能のデプロイ ワークフローを開始します。 デプロイが完了すると、その状態は **[成功]** に更新されます。

1. 詳細については、[Azure Arc を使用した拡張機能の管理](/azure/azure-arc/servers/manage-vm-extensions)に関するページを参照してください

## <a name="enable-advanced-data-security"></a>高度なデータ セキュリティを有効にする

次に、SQL Server インスタンスの高度なデータ セキュリティを有効にする必要があります。

1. Security Center に移動して、サイドバーから **[価格と設定]** ページを開きます。

1. 前の手順で MMA 拡張機能用に構成したワークスペースを選択します

1. **[Standard]** を選択します。 **[SQL servers on machines (Preview)]\(マシン上の SQL サーバー (プレビュー)\)** のオプションが有効になっていることを確認します。

   ![ワークスペースのアップグレード](media/configure-advanced-data-security/upgrade-log-analytics-workspace.png)

 > [!NOTE]
   > 脆弱性評価を生成する最初のスキャンは、高度なデータ セキュリティを有効にした後 24 時間以内に行われます。 その後、自動スキャンが毎週日曜日に実行されます。

## <a name="explore"></a>探索

Azure Security Center でセキュリティの異常と脅威について調べます。

1. ご利用の SQL Server – Azure Arc リソースを開いて、左側のメニューで **[セキュリティ]** を選択します。 そのインスタンスの推奨事項とアラートを確認できます。

   ![セキュリティ見出しの選択](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. 推奨事項のいずれかをクリックすると、__Security Center__ に脆弱性の詳細が表示されます。

   ![脆弱性のレポート](media/configure-advanced-data-security/vulnerabilities-report.png)

1. 任意のセキュリティ アラートをクリックして詳細を確認し、[Azure Sentinel](/azure/sentinel/overview) で攻撃についてさらに調べます。 ブルート フォース アラートの例を次の図に示します。

   ![ブルート フォース アラート](media/configure-advanced-data-security/brute-force-alert.png)

1. アラートを軽減するには、 **[アクションの実行]** をクリックします。

   ![アラートの軽減](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> ページの上部にある汎用の __Security Center__ リンクではプレビュー ポータルの URL は使用されないため、__SQL Server - Azure Arc__ リソースはそこには表示されません。 個々の推奨事項またはアラートのリンクを使用することをお勧めします。

## <a name="next-steps"></a>次のステップ

[Azure Sentinel](/azure/sentinel/overview) を使用して、セキュリティのアラートと攻撃をさらに詳しく調査できます。 ここの手順に従って [Azure Sentinel をオンボード](/azure/sentinel/connect-data-sources)します。