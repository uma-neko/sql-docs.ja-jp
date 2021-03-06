---
title: マップ (レポート ビルダー) | Microsoft Docs
description: レポート ビルダーの自分のページ分割されたレポートで、ビジネス データの地理的な背景を表示するマップを、自分のページ分割されたレポートに追加する方法について説明します。
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10508"
- MICROSOFT.REPORTDESIGNER.MAPBINDINGFIELDPAIR.FIELDNAME
- sql13.rtp.rptdesigner.mapproperties.general.f1
- MICROSOFT.REPORTDESIGNER.MAPPOLYGON.CENTERPOINTTEMPLATE
- "10500"
- sql13.rtp.rptdesigner.maptitleproperties.general.f1
ms.assetid: b5e9ef21-11b7-4ed2-838e-d8eecdb5c5f0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9e6dde5a520b845cac47fbfd3c4820d35958c9ba
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907270"
---
# <a name="maps-report-builder-and-ssrs"></a>マップ (レポート ビルダーおよび SSRS)
  [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] のページ分割されたレポートにマップを追加すると、地理的背景に対してビジネス データを視覚化することができます。 選択するマップの種類は、レポートでどのような情報を伝えるかによって異なります。 追加できるマップの種類としては、単に場所を表示するマップのほか、バブル サイズが領域内の世帯数に応じて変化するバブル マップ、店舗ごとの最も利益率の高い製品に基づいてマーカーのスタイルが変わるマーカー マップ、店舗間の経路を表示するライン マップがあります。  
  
 マップは、タイトル、ビューポート (中心点やスケールを指定する)、ビューポート用 Bing マップのタイルの背景 (オプション)、1 つまたは複数のレイヤー (空間データを表示する)、各種の凡例 (視覚化されたデータの意味を表示する) などで構成されます。 次の図に、基本的なマップの構成要素を示します。  
  
 ![rs_MapElements](../../reporting-services/report-design/media/rs-mapelements.gif "rs_MapElements")  
  
 今すぐマップを使い始めるには、「[チュートリアル:マップ レポート &#40;レポート ビルダー&#41;](../../reporting-services/tutorial-map-report-report-builder.md)」または「[レポート サンプル (レポート ビルダーおよび SSRS)](https://go.microsoft.com/fwlink/?LinkId=198283)」を参照してください。  
  
> [!NOTE]  
>  マップは、レポート パーツとして、レポートとは別に保存できます。 [レポート パーツ](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)の詳細を参照してください。  
  
##  <a name="adding-a-map-to-your-report"></a><a name="Process"></a> レポートへのマップの追加  
 レポートにマップを追加する一般的な手順は次のとおりです。  
  
-   表示する分析データと必要な空間データの種類を決めます。 たとえば、店舗ごとの相対年間売上をバブル マップに表示するには、分析データとして店舗名と店舗の売上が、さらに、空間データとして店舗名と店舗所在地 (緯度と経度) が必要となります。  
  
-   マップのスタイルを決めます。 基本マップは単に場所を表示するマップです。 バブル マップは、単一の分析値に基づいてバブル サイズが変化するマップです。 カラー分析マップは、分析データの範囲に基づいてマップ要素が変化するマップです。 どのスタイルを選択するかは、視覚化するデータと、使用する空間データの種類とによって異なります。  
  
-   空間データ ソース、空間データ、分析データ ソース、および分析データを指定する際に必要となる情報を収集します。 たとえば、空間データ ソースに対する接続文字列、必要な空間データの種類のほか、空間データと分析データを関連付けるための対応フィールドがレポート データに含まれているかどうか、といった情報を確認する必要があります。  
  
-   マップ ウィザードを実行してマップをレポートに追加します。 これにより、マップに最初のマップ レイヤーが追加されます。 追加のレイヤーを作成したり、既存のレイヤーを変更するには、マップ レイヤー ウィザードを実行します。 ウィザードの指示に従いながら、必要な手順を簡単に実行できます。 詳細については、「 [マップ ウィザードおよびマップ レイヤー ウィザードのページ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)」を参照してください。  
  
-   レポートでマップをプレビューした後、マップ ビューを調整する、レイヤーごとのデータの表示方法を変更する、データの意味を示す凡例を追加する、ユーザーにとって見やすいマップを作成するために解像度を調整するなど、マップにさまざまな調整を施すことができます。  
  
 詳細については、「 [マップ レポートの計画 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="adding-data-to-a-map"></a><a name="AddingData"></a> マップへのデータの追加  
 マップには、空間データと分析データという 2 種類のデータが使用されます。 空間データはマップの外観を定義し、分析データはマップに関連付けられている値を提供します。 たとえば、領域内の市区町村の場所は空間データによって定義され、市区町村ごとの人口は分析データによって提供されます。  
  
 マップには必ず空間データが必要です。これに対し、分析データはオプションです。 たとえば、市区町村内の店舗所在地という、単に場所を表示するマップを追加することができます。  
  
 マップ上にデータを視覚化するためには、分析データと空間データとの間に関係が存在しなければなりません。 空間データと分析データが同じソースから取得される場合、その関係は既に決まっています。 空間データと分析データのソースが異なる場合は、両者を関連付けるための対応フィールドを指定する必要があります。  
  
### <a name="spatial-data"></a>空間データ  
 空間データは、一連の座標 (座標セット) で構成されます。 データ ソースから取得される空間データには、単一のポイント、複数のポイント、単一の線、複数の線、一連の多角形などがあります。 各座標セットは *マップ要素* (郡の輪郭を表す多角形、道路を表す線、市区町村の場所を表すポイントなど) を定義します。  
  
 空間データには、次のいずれかの座標系が使用されます。  
  
-   **地理** 経度と緯度を使用して球面上の測地座標を指定します。 "地理的" の空間データを使用する場合は、投影法を指定する必要があります。 投影法は、球座標を持った物体を平面に対してどのように描画するかを指定する一連のルールです。 比較したり結合したりできるのは、同じ投影法の地理データだけです。  
  
-   **平面** X と Y を使用して平面上の幾何学的な座標を指定します。  
  
 それぞれのマップ レイヤーには、1 種類の空間データ (多角形、線、またはポイント) が表示されます。 複数種類の空間データを表示するには、複数のレイヤーをマップに追加します。 Microsoft Bing マップのタイル レイヤーを追加することもできます。 タイル レイヤーは空間データに依存しません。 タイル レイヤーには、マップ ビューポートの座標に対応する画像タイルが表示されます。  
  
#### <a name="sources-of-spatial-data"></a>空間データのソース  
 空間データには次のソースがサポートされます。  
  
-   **マップ ギャラリーのレポート** 空間データはマップ ギャラリーにあるレポートに埋め込まれます。 既定では、マップ ギャラリーは *\<drive>* :\Program Files\Microsoft SQL Server\Report Builder \MapGallery にインストールされます。  
  
    > [!NOTE]  
    >  この [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] マッピング機能は、米国の国勢調査局 ([https://www.census.gov/](https://www.census.gov/)) から無料で入手できます。 TIGER/Line シェープファイルは、Census MAF/TIGER データベースからの選択された地理的情報および地図情報の抜粋です。 TIGER/Line シェープファイルは、米国の国勢調査局から無料で入手できます。 TIGER/Line シェープファイルに関する詳細情報については、「[TIGER/Line Shapefiles and TIGER/Line Files Technical Documentation (TIGER/Line シェープファイルと TIGER/Line ファイルに関する技術ドキュメント)](https://www.census.gov/programs-surveys/geography/technical-documentation/complete-technical-documentation/tiger-geo-line.html)」をご覧ください。 TIGER/Line シェープファイル内の境界情報は、統計データの収集および集計を唯一の目的としています。統計目的のための表現および表示は、法的管轄機関、所有権、または権利の付与の決定となるものではなく、また法的な土地の記載でもありません。 Census TIGER および TIGER/Line は、米国の国勢調査局の登録商標です。  
  
-   **ESRI シェープファイル** ESRI シェープファイルには、Environmental Systems Research Institute, Inc. (ESRI) シェープファイル空間データ形式に準拠するデータが格納されたファイルのセットです。 ESRI シェープファイルは、一連のファイルを参照します。 .shp ファイル内のデータにより、地形学的または幾何学的な形状が指定されます。 .dbf ファイル内のデータは、形状の属性を示します。 マップをデザイン ビューで表示したり、レポート サーバーからマップを実行したりするには、この両方のファイルを同じフォルダーに置く必要があります。 ローカルのファイル システム上にある .shp ファイルから空間データを追加すると、空間データがレポートに埋め込まれます。 実行時に空間データを動的に取得するには、レポート サーバーにシェープファイルをアップロードし、空間データの参照元として指定します。 詳細については、「 [マップに使用する ESRI シェープファイルの検索](https://go.microsoft.com/fwlink/?linkid=178814)」を参照してください。  
  
-   **データベースに格納されている SQL Server 空間データ** **リレーショナル データベース内の** SQLGeometry **データ型または** SQLGeography [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] データ型を指定するクエリを使用できます。 詳細については、「[空間データ型の概要](../../relational-databases/spatial/spatial-data-types-overview.md)」を参照してください。  
  
     クエリ デザイナーに表示される結果セットでは、空間データの各行が 1 単位として扱われ、1 つのマップ要素に格納されます。 たとえば、結果セットの 1 つの行に複数のポイントが定義されている場合、そのマップ要素のすべてのポイントに表示プロパティが適用されます。  
  
-   **独自に作成した場所** 埋め込まれたポイント レイヤーに対して、埋め込まれたポイントとして場所を手動で追加することができます。 詳細については、「 [カスタムの場所のマップへの追加 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-custom-locations-to-a-map-report-builder-and-ssrs.md)」を参照してください。  
  
#### <a name="spatial-data-in-design-view"></a>デザイン ビューでの空間データ  
 デザイン ビューには、マップ レイヤーをデザインしやすくするためのサンプル空間データがレポート プロセッサによって表示されます。 表示されるデータは、空間データが利用できるかどうかによって異なります。  
  
-   **埋め込みデータ** レポート内のマップ レイヤーに埋め込まれたマップ要素からサンプル データが取得されます。  
  
-   **ESRI シェープファイルにリンク** ESRI シェープファイル (.shp) およびサポート ファイル (.dbf) が利用可能である場合、サンプル データがシェープファイルから読み込まれます。 それ以外の場合、レポート プロセッサによってサンプル データが生成され、 **"使用できる空間データがありません。サンプル空間データがマップに表示されます。"** というメッセージが表示されます。  
  
-   **SQL Server 空間データ** データ ソースが利用可能であり、資格情報が有効である場合は、データベース内の空間データからサンプル データが読み込まれます。 それ以外の場合、レポート プロセッサによってサンプル データが生成され、 **"使用できる空間データがありません。サンプル空間データがマップに表示されます。"** というメッセージが表示されます。  
  
#### <a name="embedding-spatial-data-in-the-report-definition"></a>レポート定義への空間データの埋め込み  
 マップ レイヤーの空間データには、分析データとは異なり、レポート定義に埋め込むことができるという選択肢があります。 空間データを埋め込む場合、マップ レイヤーに使用するマップ要素を埋め込むことになります。  
  
 埋め込んだ要素によってレポート定義のサイズは大きくなりますが、プレビューであれ、レポート サーバー上のレポートを実行する際であれ、確実に空間データが利用できます。 データが増えるため、当然、より多くのストレージが要求され、処理時間も長くなります。 空間データはもちろん、それ以外のレポート データも、レポートに必要なデータのみに制限することをお勧めします。  
  
#### <a name="controlling-map-resolution-at-run-time"></a>マップの解像度を実行時に制御する  
 空間データの解像度を変更するということは、マップに描画される線の精細度を指定するということです。 たとえば、領域について、地球表面の数百メートル単位までの精細度が必要か、1 マイル単位の解像度で十分かを検討します。  
  
 空間データをレポートに埋め込む場合、使用する解像度によって、レポート定義に含まれるマップ要素の数が左右されます。 解像度が高くなるほど、その解像度で境界線を描画するのに必要な要素の数が増えます。 空間データをレポートに埋め込まなかった場合、レポートを表示するたびに、その解像度で境界線を描画するのに必要な線がレポート サーバーによって計算されます。 表示解像度とレポートのレンダリング時間にはトレードオフが存在します。許容できる時間内でレポートをレンダリングできるように、バランスよくレポートをデザインするには、レポートの中で分析データを視覚化するために必要なレベルまで、マップの解像度を低くして単純化するようにしてください。  
  
### <a name="analytical-data"></a>分析データ  
 分析データは、市区町村の人口や店舗の売上合計など、マップ上に視覚化する対象のデータです。 分析データは、次のいずれかのソースから取得されます。  
  
-   **データセット フィールド :** レポート データ ペインのデータセットのフィールドです。  
  
-   **空間データ ソース フィールド :** 空間データに含まれている空間データ ソースのフィールドです。 たとえば、ESRI シェープファイルには、空間データと分析データの両方が含まれています。 空間データ ソースからのフィールド名は # で始まります。このフィールド名は、レイヤーのルールに使用するデータ フィールドを指定する際に、フィールドのドロップダウン リストに表示されます。  
  
-   **マップ要素の埋め込みデータ :** レポートに多角形、線、またはポイントを埋め込んだ後で、個々のマップ要素のデータ フィールドをオーバーライドしてカスタム値を設定できます。  
  
 レイヤーのルールを指定し、分析データ フィールドを選択すると、そのデータが数値型である場合、レポート プロセッサが自動的に既定の Sum 関数を使用して、そのマップ要素の集計値を計算します。 フィールドのデータが数値型でない場合、この集計関数は指定されず、暗黙的に First 集計関数が使用されます。 既定の式を変更するには、レイヤーのルールに対するオプションを変更します。 詳細については、「 [ルールおよび分析データを使用した多角形、線、およびポイントの表示の変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)」を参照してください。  
  
### <a name="match-fields"></a>対応フィールド  
 分析データをレイヤー上のマップ要素に関連付けるには、 *対応フィールド* を指定する必要があります。 対応フィールドは、マップ要素と分析データ間の関係を構築するために使用されます。 対応付けには、空間ロケーションごとの分析値を一意に示すものである限り、1 つまたは複数のフィールドを使用できます。  
  
 たとえば、市区町村の人口によってバブル サイズが変化するバブル マップの場合、次のデータが必要です。  
  
-   空間データ ソースから取得されるデータ  
  
    -   **SpatialData :** 市区町村の緯度と経度を指定する空間データが格納されるフィールドです。  
  
    -   **名前。** 市区町村の名前が格納されるフィールドです。  
  
    -   **Area :** 領域の名前が格納されるフィールドです。  
  
-   分析データ ソースから取得されるデータ  
  
    -   **Population :** 市区町村の人口が格納されるフィールドです。  
  
    -   **City :** 市区町村の名前が格納されるフィールドです。  
  
    -   **Area :** 区域、都道府県、または領域の名前が格納されるフィールドです。  
  
 この例で、市区町村名だけで人口を一意に識別することはできません。 たとえば、米国には "アルバニー" という名前の都市が多数存在します。 特定の市区町村を指定するには、市区町村名に加えて領域を指定する必要があります。  
  
##  <a name="understanding-the-map-viewport"></a><a name="Viewport"></a> マップ ビューポートについて  
 レポートのマップ データを指定した後、マップの表示領域を制限するには、マップ *ビューポート* を指定します。 既定では、ビューポートには、マップ全体と同じ領域が使用されます。 マップをトリミングする際は、レポートに含める領域を定義する最大座標と最小座標、中心、ズーム レベルを指定できます。 レポートでマップをより見やすく表示するために、凡例、距離スケール、カラー スケールをビューポートの外側に移動することができます。 ビューポートを次の図に示します。  
  
 ![rs_MapViewport](../../reporting-services/report-design/media/rs-mapviewport.gif "rs_MapViewport")  
  
##  <a name="adding-a-bing-map-tiles-layer"></a><a name="TileLayer"></a> Bing マップのタイル レイヤーの追加  
 ビューポートによって定義されている現在のマップ ビューの地理的背景として、Bing マップのタイル レイヤーを追加できます。 タイル レイヤーを追加するには、座標系として **[地理]** を、投影法として **[Mercator]** を指定する必要があります。 ビューポートの中心およびズーム レベルの選択内容に応じて、対応するタイルが自動的に Bing Maps Web サービスから取得されます。  
  
 レイヤーは、次のオプションを指定してカスタマイズできます。  
  
-   タイルの種類。 次のスタイルがサポートされています。  
  
    -   **[道路] :** 白色の背景、道路、およびラベル テキストから成る道路地図スタイルで表示します。  
  
    -   **[航空写真] :** テキストなしの航空画像スタイルで表示します。  
  
    -   **ハイブリッド。** **[道路]** スタイルと **[航空写真]** スタイルを組み合わせて表示します。  
  
-   タイルの表示テキストに使用する言語。  
  
-   Bing Maps Web サービスからタイルを取得する際にセキュリティで保護された接続を使用するかどうか。  
  
 実行手順については、「 [マップまたはマップ レイヤーの追加、変更、または削除 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)」を参照してください。  
  
 タイルの詳細については、「 [Bing Maps のタイル システム](https://go.microsoft.com/fwlink/?linkid=147315)」を参照してください。 レポート内での Bing のマップ タイルの使用については、「 [追加使用条件](https://go.microsoft.com/fwlink/?LinkId=151371)」を参照してください。  
  
##  <a name="understanding-map-layers-and-map-elements"></a><a name="MapLayers"></a> マップ レイヤーとマップ要素について  
 マップには、複数のレイヤーを追加できます。 レイヤーには 3 つの種類があります。 1 つのレイヤーに表示できる空間データの種類は 1 つだけです。  
  
-   **多角形レイヤー :** 多角形の中心点のマーカーや領域の輪郭を表示します。中心点は多角形ごとに自動的に計算されます。  
  
-   **線レイヤー :** パスまたはルートの線を表示します。  
  
-   **ポイント レイヤー :** ポイントの場所に対するマーカーを表示します。  
  
 レイヤーに対する空間データのソースを指定すると、ウィザードによって、空間データ フィールドがチェックされ、その型に基づいてレイヤーの種類が設定されます。 レイヤーには、データ ソースから取得された値ごとにマップ要素が追加されます。  
  
 たとえば、中央倉庫から特定の店舗までの配達ルートを表示するには、2 つのレイヤーを追加します。店舗所在地を表示する画鋲マーカー付きのポイント レイヤーと、倉庫から各店舗への配達ルートを表示する線レイヤーです。 ポイント レイヤーには、店舗所在地を指定する空間データとして "ポイント" データが、線レイヤーには、配達ルートを指定する空間データとして "線" データが必要です。  
  
 レイヤーには、第 4 の種類としてタイル レイヤーがあります。 タイル レイヤーは、マップ ビューポートの中心とズーム レベルに対応する Bing マップ タイルの背景を追加します。  
  
 このレイヤーを使用するには、レポートのデザイン画面でマップを選択し、マップ ペインを表示します。 マップ ペインには、マップに定義されているレイヤーの一覧が表示されます。 このペインを使用して、さまざまな操作 (オプションの変更対象レイヤーを選択する、レイヤーの描画順序を変更する、レイヤーを追加する、マップ レイヤー ウィザードを実行する、レイヤーを表示/非表示にする、マップ ビューポートの表示の中心やズーム レベルを変更するなど) を実行できます。 ビューポートを次の図に示します。  
  
 ![レイヤー ツールバー、レイヤーの表示、レイヤー名、空間データ ソースの種類、レイヤーの種類、ズーム レベルの調整、ビューの中心の調整オプションを示す [マップ レイヤー] セクションのスクリーンショット。](../../reporting-services/report-design/media/rsmaplayerzone.gif "rsMapLayerZone")  
  
 マップ レイヤーの詳細については、「 [マップまたはマップ レイヤーの追加、変更、または削除 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)」を参照してください。  
  
### <a name="varying-display-properties-for-points-lines-and-polygons"></a>ポイント、線、および多角形の表示プロパティを変更する  
 マップ要素の表示オプションは、レイヤーのルールを使用してレイヤー レベルで設定するか、または個々の要素レベルで設定できます。 たとえば、レイヤーに対するすべてのポイントの表示プロパティを設定 (または、埋め込まれているかどうかに関係なく、レイヤーに対するすべてのポイントの表示プロパティを制御するルールを設定) できるほか、埋め込まれている特定のポイントに対する表示プロパティの設定をオーバーライドすることもできます。  
  
 レポートの閲覧時、表示される値は、次の優先順位 (昇順) で制御されます。 大きい番号ほど、優先順位が高くなります。  
  
1.  **レイヤーのプロパティ :** レイヤー全体に適用されるプロパティ。 たとえば、レイヤーのプロパティを使用して、分析データのソースまたはレイヤー全体の可視性を設定します。  
  
2.  **多角形、線、ポイントのプロパティおよび埋め込みの多角形、線、ポイントのプロパティ :** 要素のソースが動的空間データであるか、埋め込み空間データであるかに関係なく、レイヤー上のすべてのマップ要素に適用されるプロパティです。 たとえば、バブルの塗りつぶしの色をグラデーションに設定し、上から下へと徐々に青色が薄くなっていくようにバブル領域を塗りつぶすには、多角形の中心点のプロパティを使用します。  
  
3.  **色ルール、サイズ ルール、幅ルール、マーカーの種類ルール :** 分析データと関係のあるマップ要素がレイヤーに存在する場合、ルールに従ってレイヤーにプロパティが適用されます。 ルールの種類は、レイヤーの種類によって異なります。 たとえば、人口に基づいてバブル サイズを変化させる場合は、ポイント サイズ ルールを使用します。  
  
4.  **埋め込みの多角形、線、またはポイントのプロパティのオーバーライド :** 埋め込まれているマップ要素については、オーバーライド オプションを選択して、あらゆるプロパティやデータ値を変更できます (ルールをオーバーライドし、個々の要素に対して適用された変更は元に戻せません)。 たとえば、画鋲マーカーを使用して、特定の店舗を強調表示することができます。  
  
 詳細については、「 [ルールおよび分析データを使用した多角形、線、およびポイントの表示の変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)」を参照してください。  
  
 マップ要素の外観を変化させることに加え、ポイント、線、多角形、またはレイヤーに対して、次のような対話機能を追加することができます。  
  
-   ツールヒントを作成する。ユーザーがマップ上にポインターを置いたときに、対応するマップ要素の詳細を表示します。  
  
-   ドリルスルー アクションを追加する。同じレポート内の他の場所、他のレポート、Web ページなどにリンクします。  
  
-   レイヤーの可視性を定義するパラメーターを式に追加する。ユーザーが特定のマップ レイヤーを表示したり非表示にしたりできるようにします。  
  
 詳細については、「[Interactive Sort, Document Maps, and Links (Report Builder and SSRS)](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)」(対話的な並べ替え、ドキュメント マップ、およびリンク (レポート ビルダーおよび SSRS)) を参照してください。  
  
##  <a name="understanding-map-legends-color-scale-and-distance-scale"></a><a name="Legends"></a> マップの凡例、カラー スケール、および距離スケールについて  
 マップを見やすくするために、レポートには各種の凡例を追加できます。 マップに追加できるアイテムは次のとおりです。  
  
-   **凡例 :** 凡例は複数作成することができます。 凡例に一覧表示されるアイテムは、各レイヤーのマップ要素に指定されたルールに従って自動的に生成されます。 各ルールについて、関連したアイテムをどの凡例に表示するかを指定します。 これにより、複数のレイヤーからのアイテムを、同じ凡例に割り当てることも、異なる凡例に割り当てることもできます。  
  
-   **カラー スケール :** 作成できるカラー スケールは 1 つです。 そのため、色ルールに対して凡例を設定し、色ルールのアイテムをカラー スケールに表示することもできます。 カラー スケールには、複数の色ルールを適用することができます。  
  
-   **距離スケール :** 表示できる距離スケールは 1 つです。 距離スケールには、現在のマップ ビューの尺度がキロメートル単位とマイル単位の両方で表示されます。  
  
 凡例、カラー スケール、および距離スケールは、ビューポートの内側または外側に、個別に配置することができます。 詳細については、「 [マップの凡例、カラー スケール、および関連付けられているルールの変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="troubleshooting-maps"></a><a name="Troubleshooting"></a> マップのトラブルシューティング  
 マップ レポートには、さまざまなデータ ソースからの空間データと分析データが使用されます。 それぞれのマップ レイヤーには、異なるデータ ソースを使用できます。 各レイヤーの表示プロパティには、レイヤーのプロパティ、ルール、マップ要素のプロパティに基づいた、特定の優先順位があります。  
  
 マップ レポートを閲覧する際に期待した結果が表示されない根本原因は多種多様です。 それぞれの問題を特定して把握するには、扱うレイヤーは一度に 1 つずつとすることをお勧めします。 マップ ペインを使用すると、特定のレイヤーを選択し、その表示/非表示を簡単に切り替えることができます。  
  
 マップ レポートの問題の詳細については、「[レポートのトラブルシューティング:マップ レポート &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> 操作方法に関するトピック  
 レポートでマップやマップ レイヤーを扱う際の詳細な手順を紹介しているトピックの一覧を次に示します。  
  
-   [マップまたはマップ レイヤーの追加、変更、または削除 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)  
  
-   [マップの凡例、カラー スケール、および関連付けられているルールの変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)  
  
-   [カスタムの場所のマップへの追加 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-custom-locations-to-a-map-report-builder-and-ssrs.md)  
  
##  <a name="in-this-section"></a><a name="Section"></a> トピックの内容  
 [マップ レポートの計画 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)  
  
 [マップ ウィザードおよびマップ レイヤー ウィザードのページ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)  
  
 [マップまたはマップ レイヤーのデータと表示のカスタマイズ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md)  
  
 [ルールおよび分析データを使用した多角形、線、およびポイントの表示の変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)  
  
 [マップまたはマップ レイヤーの追加、変更、または削除 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)  
  
 [マップの凡例、カラー スケール、および関連付けられているルールの変更 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)  
  
 [カスタムの場所のマップへの追加 &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/add-custom-locations-to-a-map-report-builder-and-ssrs.md)  
  
 [レポートのトラブルシューティング: マップ レポート &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)  
  
  
