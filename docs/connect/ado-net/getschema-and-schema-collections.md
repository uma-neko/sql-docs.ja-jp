---
title: スキーマとスキーマ コレクションを取得する
description: GetSchema メソッドを使用してデータベースからスキーマ情報を取得し、制限する方法について説明します。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051401"
---
# <a name="get-schema-and-schema-collections"></a>スキーマとスキーマ コレクションを取得する

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Microsoft SqlClient Data Provider for SQL Server の **SqlConnection** クラスには **GetSchema** が実装されています。これを使用すると、現在接続されているデータベースに関するスキーマ情報を取得できます。また、**GetSchema** メソッドからは <xref:System.Data.DataTable> の形式でスキーマ情報が返されます。 **GetSchema** メソッドはオーバーロードされたメソッドであり、オプションのパラメーターで、取得するスキーマ コレクションを指定し、返される情報の量を制限することができます。

## <a name="specifying-the-schema-collections"></a>スキーマ コレクションの指定

**GetSchema** メソッドの最初のオプション パラメーターは、文字列として指定されるコレクションの名前です。 スキーマ コレクションには 2 種類あります。すべてのプロバイダーに共通している一般的なスキーマ コレクションと、各プロバイダーによって固有のスキーマ コレクションです。  

Microsoft SqlClient Data Provider for SQL Server に対してクエリを実行し、サポートされているスキーマ コレクションの一覧を確認するには、引数を指定せずに、またはスキーマ コレクション名 "MetaDataCollections" を指定して **GetSchema** メソッドを呼び出します。 これにより、サポートされるスキーマ コレクションの一覧、それぞれがサポートする制限数、および使用する識別子部分の数と共に、<xref:System.Data.DataTable> が返されます。  

### <a name="retrieving-schema-collections-example"></a>スキーマ コレクションの取得例

Microsoft SqlClient Data Provider for SQL Server の <xref:Microsoft.Data.SqlClient.SqlConnection> クラスの <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> メソッドを使って、**AdventureWorks** サンプル データベースに含まれるすべてのテーブルに関するスキーマ情報を取得する方法について、いくつかの例を次に示します。  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>関連項目

- [データベース スキーマ情報の取得](retrieving-database-schema-information.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
