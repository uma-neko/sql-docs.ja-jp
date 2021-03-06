---
title: インジケーター (レポート ビルダー) | Microsoft Docs
description: レポート ビルダーのページ分割されたレポートの、1 つのデータ値の状態を示す小さなゲージであるインジケーターについて説明します。
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10545"
- "10547"
- sql13.rtp.rptdesigner.indicatorproperties.action.f1
- "10546"
- sql13.rtp.rptdesigner.indicatorproperties.validateandstates.f1
- sql13.rtp.rptdesigner.indicatorproperties.general.f1
ms.assetid: 2edbd279-be39-4d97-b1b6-ddbc5b17c422
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2b440f82dc7d758e7f6eb17bdf0f13e818348423
ms.sourcegitcommit: e572f1642f588b8c4c75bc9ea6adf4ccd48a353b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/15/2020
ms.locfileid: "84779514"
---
# <a name="indicators-report-builder-and-ssrs"></a>インジケーター (レポート ビルダーおよび SSRS)
  [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)]のページ分割されたレポートでのインジケーターとは、1 つのデータ値の状態をひとめでわかるようにした小さなゲージです。 インジケーターとその状態を表すアイコンは単純で、小さなサイズでもわかりやすくなっています。  
  
 レポートの状態インジケーターを使用して、次の内容を表示できます。  
  
-   **傾向** (上昇、平坦 (変化なし)、または下降の矢印を使用)  
  
-   **状態** (チェックマークや感嘆符などの一般的な記号を使用)  
  
-   **条件** (信号機や交通標識などの一般的な図形を使用)  
  
-   **評価** (正方形および星形の四分区間数などで進行状況を表示する一般的な図形および記号を使用)  
  
 インジケーターはダッシュボードや自由形式のレポートで単独でも使用できますが、多くの場合、行または列のデータを視覚化するためにテーブルまたはマトリックスで使用します。 次の図は、販売員および販売区域ごとの年度累計売上を示す信号機インジケーターを使用したテーブルです。  
   
 ![rs_IndicatorTableTrafficLight](../../reporting-services/report-design/media/rs-indicatortabletrafficlight.gif "rs_IndicatorTableTrafficLight")  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] そのまま使用できるインジケーター セットとインジケーター アイコンが組み込まれていますが、必要に応じて、個々のインジケーター アイコンとインジケーター セットをカスタマイズできます。  
  
 インジケーターを KPI として使用する方法の詳細については、「[チュートリアル:レポートへの KPI の追加 &#40;レポート ビルダー&#41;](../../reporting-services/tutorial-adding-a-kpi-to-your-report-report-builder.md)」を参照してください。  
  
> [!NOTE]  
>  インジケーターは、レポート パーツとしてレポートとは別にパブリッシュできます。 [レポート パーツ](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)の詳細を参照してください。  
  
##  <a name="comparing-indicators-to-gauges"></a><a name="ComparingIndicatorsToGauges"></a> インジケーターとゲージの比較  
 インジケーターとゲージは非常に異なるように見えますが、インジケーターはゲージを単純にしたものとも言えます。 インジケーターとゲージには 1 つのデータ値が表示されます。 大きな違いは、ゲージにはフレームやポインターなどの要素が含まれていることです。 インジケーターには、状態、アイコン、ラベル (オプション) のみが含まれています。 インジケーターの状態は、ゲージの範囲に似ています。  
  
 ゲージと同様に、インジケーターはゲージ パネル内に配置されます。 **[インジケーターのプロパティ]** ダイアログ ボックスまたはプロパティ ペインを使用してインジケーターを構成するには、パネルの代わりにインジケーターを選択する必要があります。 それ以外の場合、使用可能なオプションがゲージ パネル オプションに適用されるため、インジケーターを構成することができません。 次の図は、ゲージ パネルで選択されたインジケーターを示します。  
  
 ![rs_GaugePanelWithIndicator](../../reporting-services/report-design/media/rs-gaugepanelwithindicator.gif "rs_GaugePanelWithIndicator")  
  
 データ値を示す方法によっては、インジケーターではなくゲージを使用した方がより適切な場合もあります。 詳しくは、「 [ゲージ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)」をご覧ください。  
  
##  <a name="choosing-the-indicator-type-to-use"></a><a name="ChoosingIndicatorTypes"></a> 使用するインジケーター タイプの選択  
 データがテーブルまたはマトリックスの詳細行、行グループ、列グループのいずれにある場合も、単独でレポート本文またはダッシュボードにある場合も、適切なインジケーター セットを使用することで、データの意味をすばやく伝えることができます。 組み込みのインジケーター セットには 3 つ以上のアイコンがあります。 アイコンの形や色はさまざまです。 各アイコンは、異なるデータ状態を示します。  
  
 次の表に、組み込みインジケーター セットの一覧と、その一般的な使用方法を示します。  
  
|インジケーター セット|インジケーター タイプ|  
|-------------------|--------------------|  
|![Rs_DirectionalIcons](../../reporting-services/report-design/media/rs-directionalicons.gif "Rs_DirectionalIcons")|方向: 上向き、下向き、平坦 (変化なし)、上昇、または下降の矢印を使用して傾向を示します。|  
|![Rs_SymbolIcons](../../reporting-services/report-design/media/rs-symbolicons.gif "Rs_SymbolIcons")|記号: チェックマークや感嘆符などの一般的な記号を使用して状態を示します。|  
|![Rs_ShapeIcons](../../reporting-services/report-design/media/rs-shapeicons.gif "Rs_ShapeIcons")|図形: 交通標識や菱形などの一般的な図形を使用して条件を示します。|  
|![rs_RatingIcons](../../reporting-services/report-design/media/rs-ratingicons.gif "rs_RatingIcons")|評価: 正方形の四分区間数など、連続する値を表示する一般的な図形および記号を使用して評価を示します。|  
  
 インジケーター セットを選択したら、インジケーターのダイアログ ボックスまたはプロパティ ペインでプロパティを設定して、そのセット内の各インジケーター アイコンの外観をカスタマイズできます。 組み込みの色、アイコン、サイズ、または式を使用して、インジケーターを構成できます。  
  
##  <a name="customizing-indicators"></a><a name="CustomizingIndicators"></a> インジケーターのカスタマイズ  
 インジケーターは、必要に応じてカスタマイズできます。 インジケーター セットとそのセット内の各インジケーター アイコンは、以下の方法で変更できます。  
  
-   インジケーター アイコンの色を変更する。 たとえば、インジケーター セットの配色を単色に変更したり、既定の色以外の色を使用したりできます。  
  
-   インジケーター セットのアイコンを変更する。 たとえば、1 つのインジケーター セットで星形、円形、および四角形のアイコンを使用できます。  
  
-   インジケーターの開始値と終了値を指定する。 たとえば、インジケーター値の 75% について 1 つのアイコンを使用して、スキュー データを表示できます。  
  
-   インジケーター セットにアイコンを追加する。 たとえば、インジケーター セットにアイコンを追加して、より詳細にインジケーター値を区別できるようにします。  
  
-   インジケーター セットからアイコンを削除して、少ないアイコンでより単純にデータを表示する。  
  
 詳細については、「 [インジケーター アイコンとインジケーター セットの変更 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/change-indicator-icons-and-indicator-sets-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="using-indicators-in-tables-and-matrices"></a><a name="UsingIndicatorsInTablesMatrices"></a> テーブルおよびマトリックスでのインジケーターの使用  
 インジケーターをテーブルおよびマトリックスで使用する場合、単純な図形が理想的です。 インジケーターは、サイズが小さくても効果があります。 そのため、レポートの詳細やグループの行で使用できます。  
  
 次の図は、売上を示すために方向インジケーター セット **[4 つの矢印 (色付き)]** を使用するテーブルを含むレポートを示しています。 レポートのインジケーター アイコンは、既定の赤、黄、緑の代わりに青の網掛けを使用するように構成されています。  
  
 ![rs_IndicatorReportBlueArrows](../../reporting-services/report-design/media/rs-indicatorreportbluearrows.gif "rs_IndicatorReportBlueArrows")  
  
 インジケーターの追加、変更、削除の詳細については、「 [インジケーターの追加または削除 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/add-or-delete-an-indicator-report-builder-and-ssrs.md)」を参照してください。  
  
 レポートにインジケーターを最初に追加すると、既定値を使用するように構成されます。 この値は、データがインジケーターに希望どおりに表示されるように変更できます。 また、インジケーター アイコンの外観、使用するアイコンをインジケーターが選択する方法、およびインジケーター セットによって使用されるアイコンを変更できます。 詳細については、「 [インジケーター アイコンとインジケーター セットの変更 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/change-indicator-icons-and-indicator-sets-report-builder-and-ssrs.md)」を参照してください。  
  
 既定では、インジケーターは測定単位としてパーセンテージを使用し、データの最小値と最大値を自動的に検出するように構成されます。 インジケーター内の各アイコンにはパーセンテージ範囲があります。 パーセンテージ範囲の数は、アイコン セット内のアイコンの数によって異なりますが、範囲は同じサイズで、連続しています。 たとえば、アイコン セットに 5 つのアイコンが含まれている場合は、それぞれ 20% のサイズのパーセンテージ範囲が 5 つ存在することになります。 最初の範囲は 0 で始まり 20 で終わります。同様に 2 番目の範囲は 20 で始まり、40 で終わります。それ以降の範囲も同様です。 レポートのインジケーターは、パーセンテージ範囲にインジケーター データ値が含まれるインジケーター セットのアイコンを使用します。 セット内の各アイコンのパーセンテージ範囲は変更可能です。 最小値と最大値は、値または式を指定することにより、明示的に設定できます。 さらに、測定単位が数値になるように変更することができます。 その場合、データの最小値または最大値は指定しません。 代わりに、インジケーターが使用する各アイコンに対して開始値と終了値のみを指定します。 詳細については、「 [測定単位の設定および構成 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/set-and-configure-measurement-units-report-builder-and-ssrs.md)」を参照してください。  
  
 インジケーターは、指定されたスコープ内のインジケーター データ値全体を同期することによってデータ値を示します。 既定では、このスコープは、インジケーターを含んでいるテーブルやマトリックスなどのインジケーターの親コンテナーです。 インジケーターの同期を変更するには、レポートのレイアウトに応じて、異なるスコープを選択します。 インジケーターの同期は省略できます。 詳細については、「 [同期スコープの設定 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/set-synchronization-scope-report-builder-and-ssrs.md)」を参照してください。  
  
 レポート内のスコープの説明および設定方法については、「[合計、集計、および組み込みコレクションの式のスコープ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)」を参照してください。  
  
 インジケーターでは、1 つの値のみを使用します。 複数のデータ値を表示する必要がある場合は、インジケーターではなくスパークラインやデータ バーを使用します。 スパークラインやデータ バーは、複数のデータ値を表示できますが、小さなサイズでも単純で理解しやすく、テーブルやマトリックスでも適切に機能します。 詳細については、「 [スパークラインとデータ バー (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="sizing-indicators-to-maximize-visual-impact"></a><a name="SizingIndicatators"></a> 視覚的効果を高めるためのインジケーターのサイズ変更  
 インジケーターは、色、方向、形状のほか、サイズを変更して、視覚的効果を高めることができます。 あるレポートで、さまざまな種類の自転車に関する顧客満足度を示すインジケーターを使用するとします。 インジケーターで使用するアイコンを、顧客満足度に応じて異なるサイズになるように構成します。 顧客満足度が大きくなるほど、レポートに表示されるアイコンのサイズも大きくなります。 次の図は、自転車の売上のレポートを示し、アイコンのサイズは売上高に対応しています。  
  
 式を使用することで、星形のサイズを、インジケーターで使用されるフィールドの値に基づいて動的に設定できます。 詳細については、「 [式を使用したインジケーターのサイズの指定 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/specify-the-size-of-an-indicator-using-an-expression-report-builder-and-ssrs.md)」を参照してください。  
  
 式の記述と使用の詳細については、「[式 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="including-indicators-and-gauges-in-gauge-panels"></a><a name="IncludingIndicatorsInGauges"></a> ゲージ パネルへのインジケーターおよびゲージの配置  
 インジケーターは常にゲージ パネル内に配置されます。 ゲージ パネルは、1 つまたは複数のゲージと状態インジケーターが含まれる最上位のコンテナーです。 ゲージ パネルには、子または隣接のゲージまたはインジケーターを含めることができます。 インジケーターをゲージの子として使用する場合、ゲージに表示されるデータ値の状態を示すことによって、データを詳細に視覚化できます。 たとえば、ゲージ内のインジケーターでは、ゲージの値が値の範囲の上限 33% を指していることを示す緑の円を表示できます。 ゲージとインジケーターを同時に使用することによって、データをさまざまな方法で表すことができます。 いずれの場合も、インジケーターとゲージは同じデータ フィールドまたは異なるデータ フィールドを使用できます。  
  
 次の図は、インジケーターとゲージを同時に使用した場合およびインジケーターをゲージ内に配置した場合を示しています。  
  
 ![rs_GaugePanelWithIndicatorAndGauge](../../reporting-services/report-design/media/rs-gaugepanelwithindicatorandgauge.gif "rs_GaugePanelWithIndicatorAndGauge")  
  
 詳細については、「 [ゲージ パネルへのインジケーターとゲージの配置 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/include-indicators-and-gauges-in-a-gauge-panel-report-builder-and-ssrs.md)」を参照してください。  
  
 ゲージの使用の詳細については、「 [ゲージ (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)」を参照してください。  
  
##  <a name="sequence-of-indicator-states"></a><a name="SequenceIndicatorStates"></a> インジケーターの状態のシーケンス  
 **[インジケーターのプロパティ]** ダイアログ ボックスの **[値と状態]** タブのインジケーターの状態のシーケンスは、インジケーターの状態の開始値と終了値が一致する場合にデータ値に対して表示されるインジケーター アイコンに影響します。  
  
 これは、パーセントまたは数値の状態測定単位を使用する場合に発生します。 特に、数値の状態測定単位を使用する場合は、この測定単位に特定の値を指定するため、発生する可能性が高くなります。 また、レポートのデータ値を丸める場合も、値の個別性が低くなる傾向があるため、発生する可能性が高くなります。  
  
 次のシナリオでは、 **[3 つの矢印 (色分け)]** 方向インジケーター セットの 3 つの状態のシーケンスを変更する場合に、データの視覚エフェクトにどのような影響があるかを示します。 既定では、シーケンスは次のとおりです。  
  
1.  赤の下向き矢印  
  
2.  黄の水平矢印  
  
3.  緑の上向き矢印  
  
 次のシナリオでは、4 つの異なる状態シーケンスとそれらの値の範囲、およびシーケンスがデータの視覚エフェクトにどのように影響するかを示します。  
  
 これらのシナリオでは、 **[3 つの矢印 (色分け)]** インジケーターは、数値の状態測定単位を使用しています。  
  
|状態のシーケンス|開始値|終了値|  
|--------------------|-----------------|---------------|  
|[赤]|0|3500|  
|黄|3500|5000|  
|[緑]|5000|10000|  
  
 赤の下向き矢印は値 3500 を表し、黄の水平矢印は 5000 を表します。  
  
|状態のシーケンス|開始値|終了値|  
|--------------------|-----------------|---------------|  
|[緑]|5000|10000|  
|黄|3500|5000|  
|[赤]|0|3500|  
  
 黄の水平矢印は値 3500 を表し、緑の上向き矢印は 5000 を表します。  
  
|状態のシーケンス|開始値|終了値|  
|--------------------|-----------------|---------------|  
|[緑]|5000|10000|  
|[赤]|0|3500|  
|黄|3500|5000|  
  
 赤の下向き矢印は値 3500 を表し、緑の上向き矢印は 5000 を表します。  
  
|状態のシーケンス|開始値|終了値|  
|--------------------|-----------------|---------------|  
|黄|3500|5000|  
|[赤]|0|3500|  
|[緑]|5000|10000|  
  
 黄の下向き矢印は値 3500 および 5000 の両方を表します。  
  
 要約すると、評価はインジケーターの状態リストの先頭から開始され、データが値の範囲に含まれる最初のインジケーターの状態に関連付けられているインジケーター アイコンがレポートに表示されます。 このため、インジケーターの状態のシーケンスを変更することによって、データ値の視覚エフェクトに影響があります。  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> 操作方法に関するトピック  
 インジケーターの追加、変更、および削除方法、インジケーターの構成およびカスタマイズ方法、ゲージ内でのインジケーターの使用方法について説明しているトピックの一覧を次に示します。  
  
-   [インジケーターの追加または削除 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/add-or-delete-an-indicator-report-builder-and-ssrs.md)  
  
-   [インジケーター アイコンとインジケーター セットの変更 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/change-indicator-icons-and-indicator-sets-report-builder-and-ssrs.md)  
  
-   [測定単位の設定および構成 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/set-and-configure-measurement-units-report-builder-and-ssrs.md)  
  
-   [同期スコープの設定 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/set-synchronization-scope-report-builder-and-ssrs.md)  
  
-   [式を使用したインジケーターのサイズの指定 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/specify-the-size-of-an-indicator-using-an-expression-report-builder-and-ssrs.md)  
  
-   [ゲージ パネルへのインジケーターとゲージの配置 (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/include-indicators-and-gauges-in-a-gauge-panel-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>参照  
 [ゲージ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)   
 [スパークラインとデータ バー (レポート ビルダーおよび SSRS)](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)   
 [グラフ &#40;レポート ビルダーおよび SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
