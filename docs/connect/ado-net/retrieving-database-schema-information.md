---
title: データベース スキーマ情報の取得
description: Microsoft SqlClient Data Provider for SQL Server を使用してデータベース スキーマ情報を取得する方法について説明します。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051393"
---
# <a name="retrieving-database-schema-information"></a>データベース スキーマ情報の取得

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

データベースからのスキーマ情報の取得は、スキーマの検出プロセスによって行われます。 スキーマの検出により、アプリケーションでは、特定のデータベースのデータベース スキーマ ("*メタデータ*" とも呼ばれます) に関する情報を検索して返すように、マネージド プロバイダーに要求できます。 テーブル、列、ストアド プロシージャなどの各種のデータベース スキーマ要素は、スキーマ コレクションを通じて公開されます。 各スキーマ コレクションには、使用されているプロバイダーに固有の各種のスキーマ情報が含まれています。

Microsoft SqlClient Data Provider for SQL Server の **SqlConnection** クラスには **GetSchema** メソッドが実装されており、**GetSchema** メソッドからは <xref:System.Data.DataTable> の形式でスキーマ情報が返されます。 **GetSchema** メソッドはオーバーロードされたメソッドであり、オプションのパラメーターで、取得するスキーマ コレクションを指定し、返される情報の量を制限することができます。 SqlClient データ プロバイダーには、**SqlDataReader** の列メタデータが記述された DataTable を返す **GetSchemaTable** メソッドも用意されています。

## <a name="in-this-section"></a>このセクションの内容

[GetSchema およびスキーマ コレクション](getschema-and-schema-collections.md)  
**GetSchema** メソッドと、このメソッドを使ってデータベースからスキーマ情報を取得して制限する方法について説明します。

[スキーマの制限](schema-restrictions.md)  
**GetSchema** で使用できるスキーマの制限について説明します。 

[共通のスキーマ コレクション](common-schema-collections.md)  
すべての .NET マネージド プロバイダーでサポートされるすべての共通のスキーマ コレクションについて説明します。  
  
[SQL Server スキーマ コレクション](sql-server-schema-collections.md)  
Microsoft SqlClient Data Provider for SQL Server によってサポートされている追加のスキーマ コレクションについて説明します。 

## <a name="reference"></a>関連項目

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
<xref:System.Data.Common.DbConnection> クラスの **GetSchema** メソッドについて説明します。

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
<xref:Microsoft.Data.SqlClient.SqlConnection> クラスの **GetSchema** メソッドについて説明します。

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
<xref:System.Data.Common.DbDataReader> クラスの **GetSchemaTable** メソッドについて説明します。 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
<xref:Microsoft.Data.SqlClient.SqlDataReader> クラスの **GetSchemaTable** メソッドについて説明します。

## <a name="see-also"></a>関連項目

- [ADO.NET でのデータの取得と変更](retrieving-modifying-data.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
