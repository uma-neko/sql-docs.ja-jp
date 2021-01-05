---
title: データの取得と変更
description: .NET では、Microsoft SqlClient Data Provider for SQL Server は、データを読み取って更新するための、アプリケーションとデータ ソース間のブリッジとして機能します。
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c3cf3766ffc6c8acf6025b58aa0adbaafafa2188
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038960"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>ADO.NET でのデータの取得と変更

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

データベース アプリケーションの主な機能は、データ ソースとの接続およびデータベースに格納されているデータの取得です。 SqlClient データ プロバイダーは、アプリケーションとデータ ソースの間のブリッジとして機能し、**DataReader** または **DataAdapter** を使用することで、コマンドを実行したり、データを取得したりできます。 データベースに格納されているデータを更新する機能は、データベース アプリケーションの重要な機能の 1 つです。 Microsoft SqlClient Data Provider for SQL Server でデータを更新するには、**DataAdapter** と <xref:System.Data.DataSet>、および **Command** オブジェクトを使用する必要があります。また、トランザクションを使用することが必要になる場合もあります。

## <a name="in-this-section"></a>このセクションの内容

[データ ソースへの接続](connecting-to-data-source.md)  
データ ソースへの接続を確立する方法、および接続イベントを使用する方法について説明します。

[接続文字列](connection-strings.md)  
接続文字列のキーワード、セキュリティ情報、セキュリティ情報の格納や取得など、接続文字列を使用するうえでのさまざまな側面について説明します。

[接続プール](connection-pooling.md)  
Microsoft SqlClient Data Provider for SQL Server の接続プールについて説明します。

[コマンドおよびパラメーター](commands-parameters.md)  
コマンドおよびコマンド ビルダーを作成する方法、パラメーターを構成する方法、およびコマンドを実行してデータを取得および変更する方法について説明します。

[DataAdapter と DataReader](dataadapters-datareaders.md)  
DataReaders、DataAdapters、パラメーター、DataAdapter イベントの処理、およびバッチ操作の実行について説明します。

[トランザクションとコンカレンシー](transactions-and-concurrency.md) ローカル トランザクションの実行方法、分散トランザクション、オプティミスティック同時実行制御の使用について説明するトピックが含まれています。

[データベース スキーマ情報の取得](retrieving-database-schema-information.md) 使用可能なデータベースまたはカタログ、データベース内のテーブルとビュー、テーブルに存在する制約、およびその他のスキーマ情報をデータ ソースから取得する方法について説明します。

## <a name="see-also"></a>関連項目

- [ADO.NET のデータ型のマッピング](data-type-mappings-ado-net.md)
- [SQL Server と ADO.NET](./sql/index.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
