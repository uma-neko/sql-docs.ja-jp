---
title: Azure Arc への接続
titleSuffix: ''
description: SQL Server のインスタンスを Azure Arc に接続する
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 5f0401081e6d437bcc0290c111bb5325e476650e
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103193"
---
# <a name="connect-your-sql-server-to-azure-arc"></a>SQL Server を Azure Arc に接続する

これらの手順に従って、オンプレミスの SQL Server インスタンスを Azure Arc に接続することができます。

## <a name="prerequisites"></a>前提条件

* マシンに SQL Server の 1 つ以上のインスタンスがインストールされていること
* 次のいずれかの方法を使用して、**Microsoft.AzureArcData** リソース プロバイダーが登録されていること  
    * Azure portal の使用:
        - **[サブスクリプション]** を選択します 
        - サブスクリプションの選択
        - **[設定]** で、 **[リソース プロバイダー]** を選択します
        - `Microsoft.AzureArcData` を検索し、 **[登録]** を選択します
    * PowerShell を使用して、`Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData` を実行します。
    * CLI を使用して、`az provider register --namespace 'Microsoft.AzureArcData` を実行します。

## <a name="generate-a-registration-script-for-sql-server"></a>SQL Server の登録スクリプトを生成する

この手順では、マシンにインストールされているすべての SQL Server インスタンスを検出し、__SQL Server - Azure Arc__ リソースとして登録するスクリプトを生成します。 ホストしている物理または仮想マシンが Azure Arc に登録されていない場合、スクリプトによって自動的に行われます。

1. __SQL Server - Azure Arc__ リソースの種類を検索し、作成ブレードを使用して新しいものを追加します。

![作成を開始する](media/join/start-creation-of-sql-server-azure-arc-resource.png)

2. 前提条件を確認し、 **[サーバーの詳細]** タブに移動します。  

3. サブスクリプション、リソース グループ、Azure リージョン、およびホスト オペレーティング システムを選択します。 必要に応じて、ネットワークでインターネットへの接続に使用するプロキシも指定します。

> [!IMPORTANT]
> SQL Server インスタンスをホストしているマシンが既に [Azure Arc に接続されている](/azure/azure-arc/servers/onboard-portal)場合は、必ず、対応する __マシン - Azure Arc__ リソースを含む同じリソース グループを選択してください。

![サーバーの詳細](media/join/server-details-sql-server-azure-arc.png)

4. **[スクリプトの実行]** タブに移動し、表示された登録スクリプトをダウンロードします。 指定したホストしている OS のスクリプトがポータルによって生成されます。

![スクリプトをダウンロードする](media/join/download-script-sql-server-azure-arc.png)

## <a name="connect-the-installed-sql-server-instances-to-azure-arc"></a>インストールされている SQL Server インスタンスを Azure Arc に接続する

この手順では、Azure portal からダウンロードしたスクリプトを使用し、ターゲットの物理または仮想マシンで実行します。 その結果、マシンにインストールされている各 SQL Server インスタンスが、__SQL Server - Azure Arc__ リソースとして登録されます。 さらに、マシン自体にゲスト構成エージェントがインストールされていない場合は、自動的にインストールされ、__マシン - Azure Arc__ リソースとして登録されます。

> [!NOTE]
> 必ず、「[前提条件](overview.md#prerequisites)」で説明されている最小限のアクセス許可要件を満たすアカウントを使用して、スクリプトを実行してください。

### <a name="windows"></a>Windows

1. __powershell.exe__ の管理者インスタンスを起動し、Azure の資格情報を使用して PowerShell モジュールにサインインします。 [サインイン手順](/powershell/azure/install-az-ps#sign-in)に従ってください。

2. ダウンロードしたスクリプトを実行します

   ```powershell
   & '.\RegisterSqlServerArc.ps1'
   ```

   > [!NOTE]
   > Powershell 用の AZ モジュールをまだインストールしていない場合は、初回時に問題が発生する可能性があります。 その場合は、スクリプトの指示に従ってインストールし、ご利用のアカウントを接続して、スクリプトをもう一度実行してください。

### <a name="linux"></a>Linux

1. Azure CLI を使用し、Azure の資格情報を使ってサインインします。 [サインイン手順](/cli/azure/authenticate-azure-cli)に従ってください

2. ダウンロードしたスクリプトに実行アクセス許可を付与して実行します。

   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="register-sql-server-instances-on-multiple-machines"></a>複数のマシン上の SQL Server インスタンスを登録する

複数の Windows または Linux マシンにインストールされている複数の SQL Server インスタンスは、単一のマシンに対して生成したものと同じスクリプトを使用して、Azure Arc に接続することができます。 [SQL Server インスタンスを大規模に Azure Arc に接続する](connect-at-scale.md)手順に従ってください。

## <a name="validate-the-sql-server---azure-arc-resources"></a>SQL Server - Azure Arc リソースを確認する

[Azure portal](https://ms.portal.azure.com/#home) に移動し、新しく登録された __SQL Server - Azure Arc__ リソースを開いて確認します。

![接続されている SQL Server を確認する ](media/join/validate-sql-server-azure-arc.png)

## <a name="disconnect-your-sql-server-instance"></a>SQL Server インスタンスを切断する

SQL Server インスタンスを Azure Arc から切断するには、Azure portal に移動し、そのインスタンスの __SQL Server - Azure Arc__ リソースを開き、 **[登録解除]** ボタンをクリックします。

![SQL Server の登録を解除する](media/join/unregister-sql-server-azure-arc.png)

## <a name="next-steps"></a>次のステップ

* [SQL Server インスタンスの高度なデータ セキュリティを構成する](configure-advanced-data-security.md)

* [SQL Server インスタンスのオンデマンド SQL 評価を構成する](assess.md)