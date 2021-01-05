---
title: Azure Arc 対応 SQL Server
titleSuffix: ''
description: Azure Arc 対応 SQL Server を使用して SQL Server のインスタンスを管理する
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 10/07/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: 59a3dab4136749f85e1f752ee823f8815080fd76
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987988"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>Azure Arc 対応 SQL Server (プレビュー)

Azure Arc 対応 SQL Server は、Azure Arc for servers の一部です。 これにより、お客様のデータセンター、エッジ、またはマルチクラウド環境という Azure の外部でホストされている SQL Server インスタンスに Azure サービスが拡張されます。

Azure サービスを有効にするには、Azure portal と登録スクリプトを使用して、実行中の SQL Server インスタンスを Azure Arc に登録する必要があります。 登録後、インスタンスは Azure 上で __SQL Server - Azure Arc__ リソースと表示されます。 このリソースのプロパティは、SQL Server 構成設定のサブセットを反映しています。

SQL Server は、Connected Machine エージェントを介して Azure Arc に接続されている Windows または Linux を実行している仮想マシンまたは物理マシンにインストールできます。 エージェントがインストールされ、マシンは SQL Server インスタンス登録の一部として自動的に登録されます。 Connected Machine エージェントは、TCP ポート 443 を介して Azure Arc へのアウトバウンド通信を安全に行います。 マシンがファイアウォールまたは HTTP プロキシ サーバーを介して接続し、インターネット経由で通信する場合は、[Connected Machine エージェントのネットワーク構成要件](/azure/azure-arc/servers/agent-overview#prerequisites)を確認します。

Azure Arc 対応 SQL Server のパブリック プレビューでは、Microsoft Monitoring Agent (MMA) サーバーの拡張機能をインストールし、データ収集とレポートのために Azure Log Analytics ワークスペースに接続する必要がある一連のソリューションをサポートしています。 これらのソリューションには、Azure Security Center と Azure Sentinel を使用した高度なデータ セキュリティと、オンデマンド SQL Assessment 機能を使用した SQL 環境の正常性チェックが含まれます。

次の図は、Azure Arc 対応 SQL Server のアーキテクチャを示しています。

![パブリック プレビューのアーキテクチャ](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>前提条件

### <a name="supported-sql-versions-and-operating-systems"></a>サポートされている SQL バージョンとオペレーティング システム

Azure Arc 対応 SQL Server は、Windows または Linux オペレーティング システムの次のバージョンのいずれかで実行されている SQL Server 2012 以降をサポートしています。

- Windows Server 2012 R2 以降
- Ubuntu 16.04 および 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>必要なアクセス許可

SQL Server インスタンスとホスティングを Azure Arc に接続するには、次の操作を実行する特権のあるアカウントが必要です。
   * Microsoft.AzureData/*
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

セキュリティを最適化するために、最小限のアクセス許可が登録されているカスタム ロールを Azure で作成することをお勧めします。 これらのアクセス許可を持つカスタム ロールを Azure で作成する方法については、[カスタム ロールの概要](/azure/active-directory/users-groups-roles/roles-custom-overview)に関するページを参照してください。 ロールの割り当てを追加するには、「[Azure portal を使用して Azure ロールの割り当てを追加または削除する](/azure/role-based-access-control/role-assignments-portal)」、または [Azure RBAC と Azure CLI を使用したロールの割り当ての追加または削除](/azure/role-based-access-control/role-assignments-cli)に関するページを参照してください。

### <a name="azure-subscription-and-service-limits"></a>Azure サブスクリプションとサービスの制限

Azure Arc で SQL サーバー インスタンスとマシンを構成する前に、Azure Resource Manager の[サブスクリプションの制限](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits)と[リソース グループの制限](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits)を確認して、接続されるコンピューターの数を計画してください。

### <a name="networking-configuration-and-resource-providers"></a>ネットワーク構成とリソース プロバイダー

Connected Machine エージェントに必要な[ネットワーク構成、トランスポート層セキュリティ、およびリソース プロバイダー](/azure/azure-arc/servers/agent-overview#prerequisites)を確認します。

### <a name="supported-azure-regions"></a>サポート対象の Azure リージョン

パブリック プレビューは、次のリージョンで使用できます。
- 米国東部
- 米国東部 2
- 米国西部 2
- オーストラリア東部
- 東南アジア
- 北ヨーロッパ
- 西ヨーロッパ
- 英国南部

## <a name="next-steps"></a>次のステップ

- [SQL Server を Azure Arc に接続する](connect.md)
- [オンデマンド SQL 評価を使用して、環境の正常性チェックを定期的に行うように SQL Server インスタンスを構成する](assess.md)
- [SQL Server インスタンスの高度なデータ セキュリティを構成する](configure-advanced-data-security.md)