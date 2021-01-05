---
title: SQL Server ビッグ データ クラスターのリリース ノート
titleSuffix: SQL Server big data clusters
description: この記事では、SQL Server ビッグ データ クラスターの最新の更新プログラムと既知の問題について説明します。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b3265c99e185f4a8fcbe68e400ab1277b1e72671
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442882"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>SQL Server 2019 ビッグ データ クラスターのリリース ノート

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

次のリリース ノートは、[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]に適用されます。 この記事は、リリースごとのセクションに分けられています。 各リリースには、Linux パッケージのダウンロードへのリンクに加えて、CU の変更について説明するサポート記事へのリンクが含まれています。 この記事には、[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC) の最新リリースに関する[既知の問題](#known-issues)の一覧もあります。

## <a name="supported-platforms"></a>サポートされているプラットフォーム

このセクションでは、BDC でサポートされているプラットフォームについて説明します。

### <a name="kubernetes-platforms"></a>Kubernetes プラットフォーム

|プラットフォーム|サポートされているバージョン|
|---------|---------|
|バニラ (アップストリーム) Kubernetes|Kubernetes クラスターの最小バージョン 1.13 を使用して、オンプレミスに BDC をデプロイします。 「[Kubernetes バージョンとバージョン スキュー サポート ポリシー](https://kubernetes.io/docs/setup/release/version-skew-policy/)」を参照してください。|
|Red Hat OpenShift|OpenShift クラスターの最小バージョン 4.3 を使用して、オンプレミスに BDC をデプロイします。 「[Red Hat OpenShift Container Platform Life Cycle Policy](https://access.redhat.com/support/policy/updates/openshift)」(Red Hat OpenShift Container Platform のライフ サイクル ポリシー) を参照してください。<br><br> SQL Server 2019 CU5 で導入されたサポート。|
|Azure Kubernetes Service (AKS)|AKS クラスターの最小バージョン 1.13 に BDC をデプロイします。<br/>バージョンのサポート ポリシーについては、[AKS でサポートされている Kubernetes のバージョン](/azure/aks/supported-kubernetes-versions)に関するページを参照してください。|
|Azure Red Hat OpenShift (ARO)|ARO の最小バージョン 4.3 に BDC をデプロイします。 [Azure Red Hat OpenShift](/azure/openshift/)に関するページを参照してください。 <br><br> SQL Server 2019 CU5 で導入されたサポート。|

### <a name="host-os-for-kubernetes"></a>Kubernetes のホスト OS

|プラットフォーム|ホスト OS|サポートされているバージョン|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|7.3、7.4、7.5、7.6|
|OpenShift|Red Hat Enterprise Linux または CoreOS |[OpenShift のリリース ノート](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release)を参照してください|

### <a name="sql-server-editions"></a>SQL Server のエディション

|Edition|Notes|
|---------|---------|
|Enterprise<br/>Standard<br/>Developer| ビッグ データ クラスターのエディションは、SQL Server マスター インスタンスのエディションによって決まります。 展開時に、Developer エディションが既定で展開されます。 エディションは、展開後に変更できます。 [SQL Server マスター インスタンスの構成](./configure-sql-server-master-instance.md)に関するページを参照してください。 |

## <a name="tools"></a>ツール

|プラットフォーム|サポートされているバージョン|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|ベスト プラクティスとして、利用可能な最新バージョンを使用します。 SQL Server 2019 CU5 リリース以降では、[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] にサーバーから独立したセマンティック バージョンがあります。 <br/><br/>`azdata –-version` を実行して、バージョンを検証します。<br/><br/>最新バージョンについては、「[リリース履歴](#release-history)」を参照してください。|
|Azure Data Studio|[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) の最新のビルドを取得します。|

完全な一覧については、「[必要なツール](deploy-big-data-tools.md#which-tools-are-required)」を参照してください

## <a name="release-history"></a>リリース履歴

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] のリリース履歴の一覧を次の表に示します。

| リリース <sup>1</sup> | BDC のバージョン    | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] バージョン <sup>2</sup>| リリース日 |
|------------------|----------------|-----------------|--------------|
| [CU8](#cu8)      | 15.0.4073.23   | 20.2.2          | 2020-10-19   |
| [CU6](#cu6)      | 15.0.4053.23   | 20.0.1          | 2020-08-04   |
| [CU5](#cu5)      | 15.0.4043.16   | 20.0.0          | 2020-06-22   |
| [CU4](#cu4)      | 15.0.4033.1    | 15.0.4033       | 2020-03-31   |
| [CU3](#cu3)      | 15.0.4023.6    | 15.0.4023       | 2020-03-12   |
| [CU2](#cu2)      | 15.0.4013.40   | 15.0.4013       | 2020-02-13   |
| [CU1](#cu1)      | 15.0.4003.23   | 15.0.4003       | 2020-01-07   |
| [GDR1](#rtm)     | 15.0.2070.34   | 15.0.2070       | 2019-11-04   |

<sup>1</sup> CU7 は BDC では使用できません。

<sup>2</sup> [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] バージョンは、CU リリース時のツールのバージョンを反映しています。 `azdata` はサーバー リリースとは無関係でリリースされることもあります。そのため、最新のパッケージをインストールするとき、新しいバージョンが取得されることがあります。 新しいバージョンは、前にリリースされた CU との間に互換性があります。

## <a name="how-to-install-updates"></a>更新プログラムのインストール方法

更新プログラムをインストールする方法については、「[[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]をアップグレードする方法](deployment-upgrade.md)」を参照してください。

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (2020 年 9 月)

SQL Server 2019 の累積的な更新プログラム 8 (CU8) リリース。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

このリリースには、いくつかの修正といくつかの機能強化が含まれています。

### <a name="added-capabilities"></a>追加された機能

- システム マネージド キーと証明書を使用した [SQL Server ビッグ データ クラスターの保存時の暗号化](encryption-at-rest-concepts-and-configuration.md)。
   > [!CAUTION]
   > これは、SQL Server BDC の保存時の暗号化の最初のリリースです。 以下の記事を確認してください。 
   > - [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] のセキュリティの概念](concept-security.md)
   > - [保存時の暗号化の概念と構成ガイド](encryption-at-rest-concepts-and-configuration.md)
- [Oracle プロキシ ユーザー](tutorial-query-oracle.md)は、データ仮想化シナリオをサポートしています。

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (2020 年 7 月)

SQL Server 2019 の累積的な更新プログラム 6 (CU6) リリース。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

このリリースには、軽微な修正と機能強化が含まれています。 次の記事には、これらの更新プログラムに関連する情報が記されています。

- [Active Directory モードでビッグ データ クラスター アクセスを管理する](manage-user-access.md)
- [Active Directory モードで [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]を展開する](active-directory-deploy.md)
- [Active Directory モードで AKS に [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] を展開する](active-directory-deployment-aks.md)
- [Azure Kubernetes Service (AKS) プライベート クラスターでビッグ データ クラスターを展開する](private-deploy.md)
- [Azure Kubernetes Service (AKS) プライベート クラスターでビッグ データ クラスター (BDC) クラスターのエグレス トラフィックを制限する](private-restrict-egress-traffic.md)
- [高可用性を使用して SQL Server ビッグ データ クラスターを展開する](deployment-high-availability.md)
- [SQL Server ビッグ データ クラスターを構成する](configure-cluster.md)
- [ビッグ データ クラスターで Apache Spark と Apache Hadoop を構成する](configure-spark-hdfs.md)
- [SQL Server マスター インスタンスの構成プロパティ](reference-config-master-instance.md)
- [Apache Spark と Apache Hadoop (HDFS) の構成プロパティ](reference-config-spark-hadoop.md)
- [Kubernetes RBAC モデルと BDC を管理するユーザーおよびサービス アカウントへの影響](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (2020 年 6 月)

SQL Server 2019 の累積的な更新プログラム 5 (CU5) リリース。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>追加された機能

- Red Hat OpenShift でのビッグ データ クラスターのデプロイのサポート。 サポートには、オンプレミス バージョン 4.3 以降と Azure Red Hat OpenShift にデプロイされる OpenShift コンテナー プラットフォームが含まれます。 [OpenShift への SQL Server ビッグ データ クラスターのデプロイ](deploy-openshift.md)に関するページを参照してください
- BDC のデプロイ セキュリティ モデルが更新されたため、BDC の一部としてデプロイされた特権コンテナーが "*必要*" なくなりました。 SQL Server 2019 CU5 を使用するすべての新しいデプロイでは、非特権だけでなく、コンテナーも既定では非ルート ユーザーとして実行されます。 
- Active Directory ドメインに対して複数のビッグ データ クラスターをデプロイするためのサポートが追加されました。
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] には、サーバーから独立した独自のセマンティック バージョンがあります。 azdata のクライアントとサーバー バージョンの間の依存関係はすべて削除されます。 最新の拡張機能と修正プログラムの利点が確実に得られるように、クライアントとサーバーの両方に最新バージョンを使用することをお勧めします。
- 特定の外部データ ソースのイントロスペクションをサポートするために、sp_data_source_objects と sp_data_source_table_columns という 2 つの新しいストアド プロシージャが導入されました。 これらは、お客様が T-SQL を介して直接使用し、スキーマを検出したり、仮想化が可能なテーブルを確認したりすることができます。 これらの変更は、Azure Data Studio 用の[データ仮想化拡張機能](../azure-data-studio/extensions/data-virtualization-extension.md)の外部テーブル ウィザードで活用されます。これにより、SQL Server、Oracle、MongoDB、Teradata から外部テーブルを作成することができます。
- Grafana で実行されたカスタマイズを永続化するためのサポートが追加されました。 CU5 より前のお客様は、Grafana 構成でのすべての編集が、(Grafana ダッシュボードをホストする) `metricsui` ポッドの再起動時に失われることにお気付きかもしれません。 この問題は修正され、すべての構成が永続化されるようになりました。 
- (`metricsdc` ポッドでホストされている) Telegraf を使ってポッドとノードのメトリックを収集するために使用される、API に関するセキュリティの問題が修正されました。 この変更の結果として、Telegraf を使用する場合、ポッドとノードのメトリックを収集するために必要なアクセス許可をサービス アカウント、クラスター ロールおよびクラスター バインドに付与することが必要になりました。 詳細については、「[ポッドとノードのメトリックの収集に必要なクラスター ロール](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection)」を参照してください。
- ポッドとノードのメトリックの収集を制御するために、2 つの機能スイッチが追加されました。 Kubernetes インフラストラクチャの監視に別のソリューションを使用する場合は、control.json 展開構成ファイルで *allowNodeMetricsCollection* と *allowPodMetricsCollection* を false に設定し、ポッドとホスト ノードに対する組み込みのメトリック収集を無効にすることができます。 OpenShift 環境の場合、組み込みのデプロイ プロファイルではこれらの設定が既定では false に設定されます。これは、ポッドとノードのメトリックを収集するには特権機能が必要であるためです。

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (2020 年 4 月)

SQL Server 2019 の Cumulative Update 4 (CU4) リリースです。 このリリースの SQL Server データベース エンジンのバージョンは、15.0.4033.1 です。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (2020 年 3 月)

SQL Server 2019 の Cumulative Update 3 (CU3) リリースです。 このリリースの SQL Server Database エンジンのバージョンは、15.0.4023.6 です。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>解決した問題

SQL Server 2019 CU3 では、以前のリリースで発生した次の問題が解決されています。

- [プライベート リポジトリを使用した展開](#deployment-with-private-repository)
- [タイムアウトによりアップグレードが失敗することがある](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (2020 年 2 月)

これは SQL Server 2019 の Cumulative Update 2 (CU2) リリースです。 このリリースの SQL Server データベース エンジンのバージョンは、15.0.4013.40 です。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (2020 年 1 月)

これは SQL Server 2019 の Cumulative Update 1 (CU1) リリースです。 このリリースに対する SQL Server データベース エンジンのバージョンは 15.0.4003.23 です。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> GDR1 (2019 年 11 月)

SQL Server 2019 一般配布リリース 1 (GDR1) で [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)]の一般提供が開始されました。 このリリースの SQL Server データベース エンジンのバージョンは、15.0.2070.34 です。

|パッケージ バージョン | イメージ タグ |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>既知の問題

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>HA SQL Server データベースの暗号化キーの暗号化機能のローテーション

- **影響を受けるリリース**: リリースに関係なく、すべてのビッグ データ クラスター HA のデプロイ。

- **問題およびユーザーへの影響**:SQL Server が HA と共にデプロイされている場合、暗号化されたデータベースの証明書ローテーションは失敗します。 マスター プールで次のコマンドを実行すると、エラー メッセージが表示されます。
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    影響はなく、コマンドは失敗し、ターゲット データベースの暗号化は以前の証明書を使用して保持されます。
    
### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>累積更新プログラムを適用する前の空の Livy ジョブ

- **影響を受けるリリース**: CU6 までのすべてのバージョン。 CU8 に対して解決されました。

- **問題およびユーザーへの影響**:アップグレード中、`sparkhead` は 404 エラーを返します。

- **回避策**:BDC をアップグレードする前に、アクティブな Livy セッションまたはバッチ ジョブがないことを確認してください。 これを回避するには、「[サポートされているリリースからのアップグレード](deployment-upgrade.md#upgrade-from-supported-release)」の手順に従ってください。 

   アップグレード処理中に Livy から 404 エラーが返された場合は、両方の `sparkhead` ノードで Livy サーバーを再起動します。 次に例を示します。

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>ビッグ データ クラスターによって生成されたサービスアカウントのパスワードの有効期限

- **影響を受けるリリース**: Active Directory 統合を使用したすべてのビッグ データ クラスターのデプロイ (リリースには無関係)

- **問題およびユーザーへの影響**:ビッグ データ クラスターのデプロイ中に、ワークフローによって一連の [サービス アカウント](active-directory-objects.md)が生成されます。ドメイン コントローラーで設定されているパスワードの有効期限ポリシーによっては、これらのアカウントのパスワードの有効期限が切れることがあります (既定値は 42 日)。 現時点では、BDC 内のすべてのアカウントの資格情報をローテーションするメカニズムはないため、有効期限が切れるとクラスターは動作しなくなります。

- **回避策**:ドメイン コントローラーで、BDC サービス アカウントの有効期限ポリシーを "パスワードを無期限にする" に更新します。 これらのアカウントの完全な一覧については、「[Active Directory オブジェクトの自動生成](active-directory-objects.md)」をご覧ください。 この操作は、有効期限の前または後に行うことができます。 後者の場合、Active Directory によって、期限切れのパスワードが再アクティブ化されます。

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>ゲートウェイ エンドポイント経由でサービスにアクセスするための資格情報

- **影響を受けるリリース**: CU5 以降にデプロイされた新しいクラスター。

- **問題およびユーザーへの影響**:SQL Server 2019 CU5 を使用してデプロイされた新しいビッグ データ クラスターの場合、ゲートウェイのユーザー名は **ルート** ではありません。 ゲートウェイ エンドポイントへの接続に使用されるアプリケーションで間違った資格情報が使用されている場合、認証エラーが発生します。 この変更は、ビッグ データ クラスター内のアプリケーションを非ルート ユーザーとして実行した結果 (SQL Server 2019 CU5 リリース以降の新しい既定の動作) であり、CU5 を使用して新しいビッグ データ クラスターをデプロイする場合、ゲートウェイ エンドポイントのユーザー名は **AZDATA_USERNAME** 環境変数を介して渡される値に基づくものになります。 これは、コントローラーと SQL Server エンドポイントに使用されるのと同じユーザー名です。 これは新しいデプロイにのみ影響し、以前のリリースのいずれかでデプロイされた既存のビッグ データ クラスターでは引き続き、**ルート** が使用されます。 Active Directory 認証を使用するためにクラスターがデプロイされている場合、資格情報には影響はありません。 

- **回避策**:ObjectExplorer で HDFS ブラウズ エクスペリエンスを有効にするためにゲートウェイに行われた接続に対する資格情報の変更は、Azure Data Studio によって透過的に処理されます。 このユース ケースに対処する必要な変更を含む、[最新の Azure Data Studio リリース](../azure-data-studio/download-azure-data-studio.md)をインストールする必要があります。
ゲートウェイ経由でサービスにアクセスするための資格情報を提供する必要があるその他のシナリオ ([!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] でのログイン、Spark の Web ダッシュボードへのアクセスなど) では、正しい資格情報が確実に使用されるようにする必要があります。 CU5 より前にデプロイされた既存のクラスターを対象とする場合は、クラスターを CU5 にアップグレードした後でも、ゲートウェイへの接続には引き続き **ルート** ユーザー名を使用することになります。 CU5 ビルドを使用して新しいクラスターをデプロイする場合は、**AZDATA_USERNAME** 環境変数に対応するユーザー名を指定してログインします。

### <a name="pods-and-nodes-metrics-not-being-collected"></a>ポッドとノードのメトリックが収集されない

- **影響を受けるリリース**: CU5 イメージを使用している新規および既存のクラスター

- **問題およびユーザーへの影響**:メトリック ポッドとホスト ノードのメトリックを収集するために `telegraf` によって使用されていた API に関するセキュリティ修正の結果として、メトリックが収集されなくなったことにお客様はお気付きかもしれません。 これは、(CU5 へのアップグレード後に) BDC の新規および既存のデプロイの両方で発生する可能性があります。 この修正の結果として、Telegraf では、サービス アカウントにクラスター全体のロールのアクセス許可が必要になりました。 デプロイ時に、必要なサービス アカウントとクラスター ロールの作成が試行されます。しかし、クラスターをデプロイまたはアップグレードを実行するユーザーに十分なアクセス許可がない場合、デプロイまたはアップグレードは警告を伴って続行されますが、ポッドとノードのメトリックは収集されなくなります。

- **回避策**:(デプロイまたはアップグレードの前あるいは後に) ロールとサービス アカウントの作成を管理者に依頼することができます。これらは BDC で使用されます。 [この記事](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection)では、必要な成果物の作成方法について説明しています。

### <a name="azdata-bdc-copy-logs-command-failure"></a>`azdata bdc copy-logs` コマンドの失敗

- **影響を受けるリリース**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] バージョン *20.0.0*

- **問題およびユーザーへの影響**:*copy-logs* コマンドの実装では、コマンドが発行されるクライアント コンピューターに `kubectl` クライアント ツールがインストールされていることが前提となります。 `oc` ツールのみがインストールされているクライアントから、OpenShift にインストールされている BDC クラスターに対してコマンドを発行する場合、次のようなエラーが表示されます: *ログの収集中にエラーが発生しました: [WinError 2] 指定されたファイルが見つかりません*。

- **回避策**:同じクライアント コンピューターに `kubectl` ツールをインストールし、`azdata bdc copy-logs` コマンドを再実行します。 `kubectl` のインストール方法については、[こちら](deploy-big-data-tools.md)の手順を参照してください。

### <a name="deployment-with-private-repository"></a>プライベート リポジトリを使用した展開

- **影響を受けるリリース**: GDR1、CU1、CU2。 CU3 に対して解決されました。

- **問題およびユーザーへの影響**:プライベート リポジトリからアップグレードする場合に特定の要件があります。

- **回避策**:プライベート リポジトリを使用して BDC を展開またはアップグレードするためにイメージを事前にプルする場合は、現在のビルド イメージとターゲット ビルド イメージがプライベート リポジトリ内にあることを確認します。 これにより、必要な場合に正常にロールバックすることが可能になります。 また、最初の展開以降にプライベート リポジトリの資格情報を変更した場合は、アップグレードする前に Kubernetes で対応するシークレットを更新します。 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] では、`AZDATA_PASSWORD` と `AZDATA_USERNAME` の環境変数を使用した資格情報の更新はサポートされていません。 [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret) を使用してシークレットを更新します。 

別のリポジトリを使用した現在のビルドとターゲット ビルドのアップグレードはサポートされていません。

### <a name="upgrade-may-fail-due-to-timeout"></a>タイムアウトによりアップグレードが失敗することがある

- **影響を受けるリリース**: GDR1、CU1、CU2。 CU 3 に対して解決されました。

- **問題およびユーザーへの影響**:タイムアウトによりアップグレードが失敗することがあります。

   次のコードにエラー例を示します。

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   このエラーは、Azure Kubernetes Service (AKS) で BDC を更新した場合によく発生します。

- **回避策**:アップグレードのタイムアウトを増やします。 

   アップグレードのタイムアウトを増やすには、アップグレード構成マップを編集します。 アップグレード構成マップを編集するには:

   1. 次のコマンドを実行します。

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. 次のフィールドを編集します。

       **`controllerUpgradeTimeoutInMinutes`** : コントローラーまたはコントローラー db のアップグレードが完了するまでの待機時間を分単位で指定します。 既定値は 5 です。 20 以上に更新します。

       **`totalUpgradeTimeoutInMinutes`** :コントローラーとコントローラー db の両方のアップグレード (`controller` + `controllerdb` のアップグレード) が完了するまでの合計時間を指定します。 既定値は 10 です。 40 以上に更新します。

       **`componentUpgradeTimeoutInMinutes`** :これ以降のアップグレードの各フェーズの完了時間を指定します。 既定値は 30 です。 45 に更新します。

   3. 保存して終了します

   別の方法として、次の Python スクリプトでタイムアウトを設定することも可能です。

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>Azure Data Studio (ADS) または curl からの Livy ジョブの送信が 500 エラーで失敗する

- **問題およびユーザーへの影響**:HA 構成で、Spark 共有リソース `sparkhead` が複数のレプリカで構成されています。 この場合、Azure Data Studio (ADS) または `curl` からの Livy ジョブの送信でエラーが発生する可能性があります。 確認するには、`sparkhead` ポッドに `curl` を実行すると、接続が拒否されます。 たとえば、`curl https://sparkhead-0:8998/` または `curl https://sparkhead-1:8998` によって 500 エラーが返されます。

   これは、次のシナリオで発生します。

   - 各 Zookeeper インスタンスの Zookeeper ポッドまたはプロセスが数回再起動します。
   - `sparkhead` ポッドと Zookeeper ポッド間のネットワーク接続が信頼できない場合。

- **回避策**:両方の Livy サーバーを再起動します。

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>可用性グループのマスター インスタンスの場合にメモリ最適化テーブルを作成する

- **問題およびユーザーへの影響**:可用性グループ データベース (リスナー) に接続するために公開されているプライマリ エンドポイントを使用して、メモリ最適化テーブルを作成することはできません。

- **回避策**:SQL Server マスター インスタンスが可用性グループの構成である場合にメモリ最適化テーブルを作成するには、新しい接続で作成されたセッションで、[SQL Server インスタンスに接続](deployment-high-availability.md#instance-connect)し、エンドポイントを公開し、SQL Server データベースに接続して、メモリ最適化テーブルを作成します。

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>外部テーブルの Active Directory 認証モードに挿入する

- **問題およびユーザーへの影響**:SQL Server マスター インスタンスが Active Directory 認証モードになっている場合、外部テーブルのうち少なくとも 1 つがストレージプール内にある場合に外部テーブルからのみ選択され、別の外部テーブルに挿入されるクエリでは、次のように返されます。

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **回避策**:次のいずれかの方法でクエリを変更します。 ストレージ プール テーブルをローカル テーブルに結合するか、最初にローカル テーブルに挿入し、ローカル テーブルからデータを読み取ってデータ プールに挿入します。

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>Transparent Data Encryption 機能が、SQL Server のマスター インスタンスの可用性グループの一部であるデータベースで使用できない

- **問題およびユーザーへの影響**:HA 構成では、暗号化に使用されるマスター キーがレプリカごとに違うため、暗号化が有効になっているデータベースをフェールオーバー後に使用することはできません。 

- **回避策**:この問題の回避策はありません。 修正が適用されるまでは、この構成では暗号化を有効にしないことをお勧めします。

## <a name="next-steps"></a>次のステップ

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]の詳細については、[[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]の概要](big-data-cluster-overview.md)に関するページを参照してください。