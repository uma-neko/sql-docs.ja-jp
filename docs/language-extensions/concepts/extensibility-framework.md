---
title: SQL Server 言語拡張の機能拡張アーキテクチャ
titleSuffix: ''
description: SQL Server 言語拡張機能に使用される機能拡張アーキテクチャについて説明します。これにより、SQL Server で外部コードを実行できるようになります。 SQL Server 2019 では、Java、Python、R がサポートされています。 このコードは、言語ランタイム環境でコア データベース エンジンの拡張機能として実行されます。
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 40fd6b73bf28b6a201a1c0fedd1624a09d67b9c0
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935371"
---
# <a name="extensibility-architecture-in-sql-server-language-extensions"></a>SQL Server 言語拡張の機能拡張アーキテクチャ

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

SQL Server 言語拡張機能に使用される機能拡張アーキテクチャについて説明します。これにより、SQL Server で外部コードを実行できるようになります。 SQL Server 2019 では、Java、Python、R がサポートされています。 このコードは、言語ランタイム環境でコア データベース エンジンの拡張機能として実行されます。

## <a name="background"></a>バックグラウンド

機能拡張フレームワークの目的は、SQL Server と外部言語の間にインターフェイスを提供することです。 SQL Server によって管理される安全なフレームワーク内で信頼できる言語を実行することで、データベース管理者は、データ科学者による企業データへのアクセスを許可しながら、セキュリティを維持することができます。

<!-- We need to get a diagram like the one below.
The following diagram visually describes opportunities and benefits of the extensible architecture.

  ![Goals of integration with SQL Server](../media/ml-service-value-add.png "Machine Learning Services Value Add")
-->

サポートされている外部言語は、ストアド プロシージャを呼び出すことで実行できます。また、その結果は表形式の結果として直接 SQL Server に返されます。 これにより、SQL クエリを送信して結果を処理する機能を持つ任意のアプリケーションから、外部言語を簡単に使用できます。

## <a name="architecture-diagrams"></a>アーキテクチャ図

このアーキテクチャは、外部コードが SQL Server とは別のプロセスで実行されるが、コンポーネントによって SQL Server のデータと操作に対する要求のチェーンが内部で管理されるように設計されています。 
  
  ***Windows のコンポーネント アーキテクチャ:***

  ![Windows 上のコンポーネント アーキテクチャ](../media/generic-architecture-windows.png "Windows 上のコンポーネント アーキテクチャ")
  
  ***Linux のコンポーネント アーキテクチャ:***
  
  ![Linux 上のコンポーネント アーキテクチャ](../media/generic-architecture-linux.png "Linux 上のコンポーネント アーキテクチャ")
  
コンポーネントには**スタート パッド** サービスが含まれています。これは、インタープリターとライブラリを読み込むための外部ランタイム (Java など) およびライブラリ固有のロジックを呼び出すために使用されます。

<a name="launchpad"></a>

## <a name="launchpad"></a>スタート パッド

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] は、スクリプトの実行を担当する外部プロセスのライフタイム、リソース、およびセキュリティ境界を管理するサービスです。 これは、フルテキスト インデックス作成およびクエリ サービスがフルテキスト クエリを処理する際に別のホストを起動する方法と似ています。 スタート パッド サービスでは、Microsoft によって公開された信頼できるランチャーか、パフォーマンスとリソース管理の要件を満たしているものとして Microsoft に認定されたランチャーのみを起動できます。

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] サービスは、実行の分離のために、[AppContainers](/windows/desktop/secauthz/appcontainer-isolation) を使用する **SQLRUserGroup** 以下で実行されます。

SQL Server Machine 言語拡張を追加したデータベース エンジン インスタンスごとに、個別の [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] サービスが作成されます。 データベース エンジン インスタンスごとに 1 つのスタート パッド サービスが存在するため、外部スクリプトをサポートするインスタンスが複数ある場合は、インスタンスごとに 1 つずつスタート パッド サービスが存在します。 データベース エンジン インスタンスは、それに対して作成されたスタート パッド サービスにバインドされます。 ストアド プロシージャまたは T-SQL で外部スクリプトが呼び出されるたびに、SQL Server サービスにより、同じインスタンスに対して作成されたスタート パッド サービスが呼び出されます。

サポートされている特定の言語でタスクを実行するために、スタート パッドにより、セキュリティで保護されたワーカー アカウントがプールから取得され、外部ランタイムを管理するサテライト プロセスが開始されます。 各サテライト プロセスにより、スタート パッドのユーザー アカウントが継承され、スクリプトの実行中にそのワーカー アカウントが使用されます。 スクリプトで並列プロセスが使用される場合、これらのプロセスは 1 つの同じワーカー アカウントで作成されます。

## <a name="communication-channels-between-components"></a>コンポーネント間の通信チャネル

このセクションでは、コンポーネント間およびデータ プラットフォーム間の通信プロトコルについて説明します。

+ **TCP/IP**

  既定では、SQL Server と SQL サテライトの間の内部通信には TCP/IP が使用されます。

+ **ODBC**

  外部データ サイエンス クライアントとリモート SQL Server インスタンス間の通信には、ODBC が使用されます。 SQL Server にスクリプト ジョブを送信するアカウントには、インスタンスへの接続権限と、外部スクリプトの実行権限の両方が付与されている必要があります。

  また、タスクによっては、アカウントに次の権限が必要になる場合があります。

  + ジョブによって使用されるデータを読み取る
  + テーブルへのデータの書き込み (たとえば、テーブルに結果を保存する場合)
  + データベース オブジェクトの作成 (たとえば、外部スクリプトを新しいストアド プロシージャの一部として保存する場合)

  SQL Server が、リモート クライアントから実行されるスクリプトの計算コンテキストとして使用され、実行可能ファイルによって外部ソースからデータを取得する必要がある場合は、ODBC が書き戻しに使用されます。 SQL Server により、リモート コマンドを発行しているユーザーの ID が、現在のインスタンス上のユーザーの ID にマップされ、そのユーザーの資格情報を使用して ODBC コマンドが実行されます。 この ODBC 呼び出しを実行するために必要な接続文字列は、クライアント コードから取得されます。

+ **その他のプロトコル**

  "チャンク" で動作するか、またはリモート クライアントにデータを転送する必要のあるプロセスでは、[XDF ファイル形式](/machine-learning-server/r/concept-what-is-xdf)も使用できます。 実際のデータ転送は、エンコードされた BLOB を使用して行われます。

## <a name="next-steps"></a>次のステップ

+ [言語拡張とは](../language-extensions-overview.md)