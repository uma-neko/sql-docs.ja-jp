---
title: パフォーマンス カウンター - ReportServer サービス、パフォーマンス オブジェクト | Microsoft Docs
description: SQL Server 2012 配置に含まれる、ReportServer:Service および ReportServerSharePoint:Service パフォーマンス オブジェクトのパフォーマンス カウンターについて説明します。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: fe50faeeb134ca9162f5de25b6c775d3b6eea829
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "91986970"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>パフォーマンス カウンター - ReportServer サービス、パフォーマンス オブジェクト
  このトピックでは、 **の配置に含まれる** ReportServer:Service **および** ReportServerSharePoint:Service [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] パフォーマンス オブジェクトのパフォーマンス カウンターについて説明します。  
  
> [!NOTE]  
>  このパフォーマンス オブジェクトを使用して、ローカル レポート サーバー上のイベントを監視します。 スケールアウト配置でレポート サーバーを実行している場合、カウントはスケールアウト配置全体ではなく、現在のサーバーに適用されます。  
  
 パフォーマンス オブジェクトは、Windows パフォーマンス モニター (**Perfmon.exe**) で利用できます。 詳細については、Windows のマニュアルを参照してください。 [ランタイム プロファイリング](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx) 。  
  
 このトピックの内容:  
  
-   [ReportServer:Service パフォーマンス カウンター (ネイティブ モードのレポート サーバー)](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service (SharePoint モードのレポート サーバー)](#bkmk_ReportServerSharePoint)  
  
-   [PowerShell コマンドレットを使用して一覧を取得する](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> ReportServer:Service パフォーマンス カウンター (ネイティブ モードのレポート サーバー)  
 **ReportServer:Service** パフォーマンス オブジェクトには複数のカウンターが含まれおり、レポート サーバー インスタンスの HTTP 関連のイベントやメモリ関連のイベントの追跡に使用されます。 このパフォーマンス オブジェクトは、コンピューター上の [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] インスタンスごとに 1 つ存在します。各インスタンスのパフォーマンス オブジェクトに対して、カウンターを追加したり削除したりできます。 既定のインスタンスのカウンターは、 **ReportServer:Service**という形式で表示されます。 名前付きインスタンスのカウンターは、**ReportServer$\<***instance_name***>:Service** という形式で表示されます。  
  
 **ReportServer:Service** パフォーマンス オブジェクトは、[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] の新機能で、以前のバージョンの [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] でインターネット インフォメーション サービス (IIS) と [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] に含まれていたカウンターのサブセットを備えています。 これらは、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]に固有の新しいカウンターです。要求、接続、ログオン試行など、レポート サーバーにおける HTTP 関連のイベントが追跡されます。 さらに、このパフォーマンス オブジェクトには、メモリ管理イベントを追跡するカウンターも含まれています。  
  
 次の表は、 **ReportServer:Service** パフォーマンス オブジェクトに含まれているカウンターの一覧です。  
  
 ![PowerShell 関連コンテンツ](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 関連コンテンツ") 次の Windows PowerShell スクリプトは CounterSetName のパフォーマンス カウンターの一覧を返します  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|カウンター|説明|  
|-------------|-----------------|  
|**Active connections**|サーバー上で現在アクティブな接続の数。|  
|**Bytes Received Total**|サーバーが受信したバイト数。 レポート マネージャーとレポート サーバーの両方によって受信された生のバイトの合計がカウントされます。|  
|**Bytes Received/sec**|サーバーが受信した 1 秒あたりのバイト数。 このカウンターは、転送が完了したときのみ更新されます。 つまり、転送が完了して初めて、カウンターの値が 0 の状態から増加します。|  
|**Bytes Sent Total**|サーバーから送信されたバイト数。 レポート マネージャーとレポート サーバーの両方によって送信された生のバイトの合計がカウントされます。|  
|**Bytes Sent/sec**|サーバーから送信された 1 秒あたりのバイト数。 このカウンターは、転送が完了したときのみ更新されます。 つまり、転送が完了して初めて、カウンターの値が 0 の状態から増加します。|  
|**Errors Total**|HTTP 要求の処理中に発生したエラーの合計数。 このエラーには、HTTP ステータス コードの 400 番台と 500 番台が含まれます。|  
|**Errors/sec**|HTTP 要求の処理中に発生した 1 秒あたりのエラーの合計数。 このエラーには、HTTP ステータス コードの 400 番台と 500 番台が含まれます。|  
|**Logon Attempts Total**|RSWindows 認証タイプに基づくログオン試行の回数。 RSWindows 認証タイプには、RSWindowsNegotiate、RSWindowsNTLM、RSWindowsKerberos、RSWindowsBasic などがあります。 ゼロ (0) はカスタム認証を表します。|  
|**Logon Attempts/sec**|ログオン試行の割合。|  
|**Logon Successes Total**|RSWindows 認証タイプのログオンに成功した回数。 RSWindows 認証タイプには、RSWindowsNegotiate、RSWindowsNTLM、RSWindowsKerberos、RSWindowsBasic などがあります。 ゼロ (0) はカスタム認証を表します。|  
|**Logon Successes/sec**|成功したログオンの割合。|  
|**Memory Pressure State**|サーバーの現在のメモリの状態を示す 1 ～ 5 の数値。<br /><br /> 1:負荷なし<br /><br /> 2:低負荷<br /><br /> 3:中負荷<br /><br /> 4:高負荷<br /><br /> 5:負荷が限度を超えている|  
|**Memory Shrink Amount**|使用メモリ量を縮小するためにサーバーが要求したバイト数。|  
|**Memory Shrink Notifications/sec**|使用メモリ量を縮小するために、サーバーが直前の 1 秒間に送信した通知の数。 この値は、サーバーでメモリ不足が何回発生しているかを示しています。|  
|**Requests Disconnected**|通信エラーのために切断された要求の数。|  
|**Requests Executing**|現在処理中の要求の数。|  
|**Requests Not Authorized**|HTTP 401 ステータス コードで失敗した要求の数。|  
|**Requests Rejected**|サーバー リソースが不足しているために処理されなかった要求の合計数。 このカウンターは、サーバーがビジーであることを示す HTTP 503 ステータス コードを返した要求の数を表します。|  
|**Requests Total**|レポート サーバー サービスが起動後に受け取った要求の合計数。 レポート マネージャーに送信された要求およびレポート マネージャーからレポート サーバーに送信ざれた要求がカウントされます。|  
|**要求数/秒**|1 秒あたりに処理された要求の数。 この値はアプリケーションの現在のスループットを表します。|  
|**Tasks Queued**|スレッドが使用可能になるのを待機しているタスクの数。 レポート サーバーに対する要求は、それぞれ、1 つまたは複数のタスクと対応します。 このカウンターによって表されるのは、処理の準備が整っているタスクの数だけです。現在実行中のタスクの数は含まれません。|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service (SharePoint モードのレポート サーバー)  
 **ReportServerSharePoint:Service** パフォーマンス オブジェクトは、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] で追加されました。  
  
 ![PowerShell 関連コンテンツ](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 関連コンテンツ") 次の Windows PowerShell スクリプトは CounterSetName のパフォーマンス カウンターの一覧を返します  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|カウンター|説明|  
|-------------|-----------------|  
|**Memory Pressure State**||  
|**Memory Shrink Amount**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> PowerShell コマンドレットを使用して一覧を取得する  
 ![PowerShell 関連コンテンツ](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 関連コンテンツ") 次の Windows PowerShell スクリプトは CounterSetName "ReportServerSharePoint:Service" のパフォーマンス カウンターの一覧を返します。  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>参照  
 [レポート サーバーのパフォーマンスの監視](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [MSRS 2011 Web Service と MSRS 2011 Windows Service パフォーマンス オブジェクトのパフォーマンス カウンター (ネイティブ モード)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [MSRS 2011 Web Service SharePoint Mode と MSRS 2011 Windows Service SharePoint Mode パフォーマンス オブジェクトのパフォーマンス カウンター (SharePoint モード)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
