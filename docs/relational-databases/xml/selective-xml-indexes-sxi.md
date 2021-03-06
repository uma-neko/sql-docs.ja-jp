---
title: 選択的 XML インデックス (SXI) | Microsoft Docs
description: クエリのパフォーマンスを向上させ、インデックス作成の高速化をサポートし、XML インデックスのストレージ コストを削減するために、選択的 XML インデックス (SXI) を使用する方法について学習します。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 598ecdcd-084b-4032-81b2-eed6ae9f5d44
author: MightyPen
ms.author: genemi
ms.openlocfilehash: ec99528c375126e06bc48ac7b241df98f62952a4
ms.sourcegitcommit: 75f767c7b1ead31f33a870fddab6bef52f99906b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87331953"
---
# <a name="selective-xml-indexes-sxi"></a>選択的 XML インデックス (SXI)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  選択的 XML インデックスは、通常の XML インデックスに加えて使用できる、別の種類の XML インデックスです。 選択的 XML インデックス機能の目標を次に示します。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]に格納された XML データに対するクエリのパフォーマンスを向上させる。  
  
-   大規模な XML データのワークロードのより速いインデックス設定をサポートする。  
  
-   XML インデックスのストレージ コストの削減によってスケーラビリティを向上させる。  
  
 通常の XML インデックスの大きな限界は、XML ドキュメント全体にインデックスを設定することです。 このため、クエリのパフォーマンスの低下やインデックスのメンテナンス コストの増加などのさまざまな欠点が生じ、そのほとんどがインデックスのストレージ コストに関連しています。  
  
 選択的 XML インデックス機能を使用して、XML ドキュメントの特定のパスをインデックスに昇格できます。 インデックスの作成時に、これらのパスが評価され、パスがポイントしているノードが細分化され、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]のリレーショナル テーブルに格納されます。 この機能は、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Research と [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 製品チームのコラボレーションによって開発された効率的なマッピング アルゴリズムを使用します。 このアルゴリズムは、XML ノードを単一のリレーショナル テーブルにマップすることで、非常に高いパフォーマンスを実現しますが、ストレージ スペースは少ししか必要ありません。  
  
 選択的 XML インデックス機能は、選択的 XML インデックスによってインデックス設定されているノードに対するセカンダリ選択的 XML インデックスもサポートします。 これらのセカンダリ選択的インデックスは効率的であり、クエリのパフォーマンスがさらに向上します。  
  
##  <a name="benefits-of-selective-xml-indexes"></a><a name="benefits"></a> 選択的 XML インデックスの利点  
 選択的 XML インデックスには、次のような利点があります。  
  
1.  一般的なクエリのロードでの XML データ型に対するクエリのパフォーマンスが大幅に向上します。  
  
2.  通常の XML インデックスよりストレージ要件が小さくなります。  
  
3.  通常の XML インデックスより、インデックスのメンテナンス コストが削減されます。  
  
4.  選択的 XML インデックスを使用するためにアプリケーションを更新する必要はありません。  
  
  
##  <a name="selective-xml-indexes-and-primary-xml-indexes"></a><a name="compare"></a> 選択的 XML インデックスとプライマリ XML インデックス  
  
> [!IMPORTANT]  
>  ほとんどの場合、通常の XML インデックスではなく選択的 XML インデックスを作成すると、パフォーマンスが向上し、効率的な保管が可能になります。  
  
 ただし、選択的 XML インデックスは、次の条件のいずれかに該当する場合はお勧めしません。  
  
-   大量のノード パスをマップする。  
  
-   ドキュメント構造内の不明な要素または不明な位置にある要素のクエリをサポートする。  
  
  
##  <a name="simple-example-of-a-selective-xml-index"></a><a name="example"></a> 選択的 XML インデックスの単純な例  
 次の XML フラグメントを、約 500,000 行ある表形式の XML ドキュメントと考えてください。  
  
```xml  
<book>  
    <created>2004-03-01</created>   
    <authors>Various</authors>  
    <subjects>  
        <subject>English wit and humor -- Periodicals</subject>  
        <subject>AP</subject>   
    </subjects>  
    <title>Punch, or the London Charivari, Volume 156, April 2, 1919</title>  
    <id>etext11617</id>  
</book>  
```  
  
 この単純なスキーマのそれほど多くの行に対するプライマリ XML インデックスの作成には、長い時間がかかります。 このデータのクエリも、プライマリ XML インデックスが選択的インデックスをサポートしていないため、同じように時間がかかります。  
  
 `/book/title` パスと `/book/subjects` パスのデータのみをクエリする必要がある場合は、次の選択的 XML インデックスを作成できます。  
  
```sql  
CREATE SELECTIVE XML INDEX SXI_index  
ON Tbl(xmlcol)  
FOR   
(  
    pathTitle = '/book/title/text()' AS XQUERY 'xs:string',   
    pathAuthors = '/book/authors' AS XQUERY 'node()',  
    pathId = '/book/id' AS SQL NVARCHAR(100)  
)  
```  
  
 上に示したステートメントは、選択的 XML インデックスを作成するときに使用する CREATE 構文の好例です。 この CREATE ステートメントでは、インデックスの名前を指定した後、インデックスを設定するテーブルと XML 列を識別します。 その後、インデックスのパスを指定します。 パスには、次の 3 つの部分があります。  
  
1.  パスを一意に識別する名前。  
  
2.  パスを記述する XQuery 式。  
  
3.  省略可能な最適化ヒント。  
  
 これらの要素の詳細については、「 [関連タスク](#reltasks)」を参照してください。  
  
  
## <a name="supported-features-prerequisites-and-limitations"></a>サポートされる機能、前提条件、および制限事項  
  
###  <a name="supported-xml-features"></a><a name="features"></a> サポートされる XML 機能  
 選択的 XML インデックスは、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] によって exist()、value()、および nodes() メソッドの中でサポートされる XQuery をサポートします。  
  
-   exist()、value()、および nodes() メソッドでは、式全体を変換するための十分な情報が選択的 XML インデックスに含まれます。  
  
-   query() メソッドと modify() メソッドでは、選択的 XML インデックスは、ノードのフィルター処理でのみ使用できます。  
  
-   query() メソッドでは、選択的 XML インデックスを使用して結果を取得することはありません。  
  
-   modify() メソッドでは、選択的 XML インデックスを使用して XML ドキュメントを更新することはありません。  
  
  
###  <a name="unsupported-xml-features"></a><a name="unsupported"></a> サポートされない XML 機能  
 選択的 XML インデックスは、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] の XML の実装でサポートされている次の機能はサポートしません。  
  
-   complex XS 型 (union 型、sequence 型、および list 型) のノードに対するインデックス設定。  
  
-   バイナリ XS 型 (base64Binary や hexBinary など) のノードに対するインデックス設定。  
  
-   末尾にワイルドカード文字 `*` を含む XPath 式を使用した、インデックスを設定するノードの指定。例: `/a/b/c/*`、`/a//b/*`、`/a/b/*:c`。  
  
-   子、属性、または子孫以外の軸に対するインデックスの設定。 `//<step>` は特殊なケースとして使用できます。  
  
-   XML 処理命令とコメントに対するインデックスの設定。  
  
-   id() 関数の使用によるノードの識別子の指定と取得。  
  
  
###  <a name="prerequisites"></a><a name="prereq"></a> 前提条件  
 ユーザー テーブルの XML 列に対して選択的 XML インデックスを作成する前に、次の前提条件を満たす必要があります。  
  
-   クラスター化インデックスは、ユーザー テーブルの主キー上に存在する必要がある。  
  
-   選択的 XML インデックスで使用する場合、ユーザー テーブルのプライマリ キーのサイズは 128 バイトに制限されます。  
  
-   選択的 XML インデックスで使用する場合、ユーザー テーブルのクラスター化キーのサイズは 15 バイトに制限されます。  
  
  
###  <a name="limitations"></a><a name="limits"></a> 制限事項  
 **一般的な要件と制限事項**  
  
-   各選択的 XML インデックスは、単一の XML 列にのみ作成できます。  
  
-   選択的 XML インデックスは、XML 以外の列には作成できません。  
  
-   テーブルの各 XML 列には、選択的 XML インデックスを 1 つだけ作成できます。  
  
-   各テーブルには、最大 249 の選択的 XML インデックスを作成できます。  
  
 **サポートされるオブジェクトに関する制限事項**  
  
 選択的 XML インデックスは、次のオブジェクトには作成できません。  
  
1.  ビューの XML 列。  
  
2.  XML 列を含むテーブル値変数。  
  
3.  XML 型の変数。  
  
4.  XML の計算列。  
  
5.  ノードの入れ子の深さが 128 を超えている XML 列。  
  
 **ストレージに関する制限事項**  
  
 インデックスに追加できる XML ドキュメントのノード数は有限です。 選択的 XML インデックスは、XML ドキュメントを単一のリレーショナル テーブルにマップします。 したがって、テーブルの特定の行には、1024 を超える NULL 以外の列が存在することはできません。 さらに、選択的 XML インデックスはスパース列をストレージ用に使用するため、スパース列に関する制限事項の多くがこのインデックスにも適用されます。  
  
 特定の行でサポートされる NULL 以外の列の最大数は、列内のデータのサイズによって決まります。  
  
-   最良の場合、すべての列の型が **bit**のとき、1024 の NULL 以外の列がサポートされます。  
  
-   最悪の場合、すべての列が **varchar**型のラージ オブジェクトのとき、236 の NULL 以外の列のみがサポートされます。  
  
 選択的 XML インデックスは、インデックスが設定されているすべてのノード パスに対して、内部的に 1 列から 4 列を使用します。 インデックスを設定できるノードの総数は、インデックス付きパス内のデータの実際のサイズに応じて、60 から数千になります。  
  
-   最悪の場合、ノード パス定義内の `//` を使用して一部のノードまたはすべてのノードがマップされるとき、インデックス付きノードの最大数は 60 です。  
  
-   最良の場合、ノード パス定義内の `//` を使用しないで一部のノードまたはすべてのノードがマップされるとき、インデックス付きノードの最大数は 200 です。  
  
 **選択的 XML インデックスは、インデックスの CREATE または ALTER 時に再構築されます。**  
  
 選択的 XML インデックスを CREATE または ALTER すると、シングル スレッドのオフライン モードで再構築されます。 ALTER ステートメントの頻繁な使用は、インデックス付き XML ドキュメントに対するクエリのパフォーマンスに悪影響を与えます。  
  
 **その他の制限事項**  
  
-   選択的 XML インデックスは、クエリ ヒントではサポートされません。  
  
-   選択的 XML インデックスとセカンダリ選択的 XML インデックスは、データベース チューニング アドバイザーではサポートされません。  
  
  
##  <a name="related-tasks"></a><a name="reltasks"></a> 関連タスク  
  
| タスク | トピック |
| ---- | ----- |
|選択的 XML インデックスを作成または変更するときに、インデックスを設定するノード パスと省略可能な最適化ヒントを指定する。|[選択的 XML インデックスのパスと最適化ヒントの指定](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)|  
|選択的 XML インデックスを作成、変更、または削除する。|[選択的 XML インデックスの作成、変更、および削除](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)|  
|セカンダリ選択的 XML インデックスを作成、変更、または削除する。|[選択的セカンダリ XML インデックスの作成、変更、および削除](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)|  
  
  
  
