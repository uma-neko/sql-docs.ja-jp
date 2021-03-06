---
description: sp_fulltext_database (Transact-sql)
title: sp_fulltext_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_fulltext_database_TSQL
- sp_fulltext_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_database
ms.assetid: eeb1e151-eb00-484c-8fd1-5641e621ffc6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: daa24fdd0bbba0ff949b7b43a74e312d114d4e0d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439447"
---
# <a name="sp_fulltext_database-transact-sql"></a>sp_fulltext_database (Transact-sql)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  以降のバージョンのフルテキストカタログには影響しません [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。旧バージョンとの互換性のためにのみサポートされています。 **sp_fulltext_database** は、特定のデータベースの Full-Text エンジンを無効にしません。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] でユーザーが作成したすべてのデータベースでは、常にフルテキスト インデックスが有効になっています。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 代わりに [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] を使用します。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```  
  
sp_fulltext_database [@action=] 'action'  
```  
  
## <a name="arguments"></a>引数  
`[ @action = ] 'action'` 実行するアクションを指定します。 **アクション** は **varchar (20)**,、これらの値のいずれかを指定することができます。  
  
|値|説明|  
|-----------|-----------------|  
|**enable**|旧バージョンとの互換性のためにのみサポートされています。 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降のバージョンのフルテキスト カタログには影響しません。|  
|**disable**|旧バージョンとの互換性のためにのみサポートされています。 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降のバージョンのフルテキスト カタログには影響しません。|  
  
## <a name="return-code-values"></a>リターン コードの値  
 0 (成功) または 1 (失敗)  
  
## <a name="result-sets"></a>結果セット  
 なし  
  
## <a name="remarks"></a>解説  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降のバージョンでは、フルテキスト インデックスを無効にすることはできません。 フルテキストインデックス作成を無効にしても、 **sysfulltextcatalogs** から行は削除されず、フルテキストインデックスが有効になっているテーブルにはフルテキストインデックスが設定されていないことが示されません。 すべてのフルテキストメタデータ定義は、システムテーブルに残ります。  
  
## <a name="permissions"></a>アクセス許可  
 **Sp_fulltext_database** を実行できるのは、 **sysadmin** 固定サーバーロールおよび **db_owner** 固定データベースロールのメンバーだけです。  
  
## <a name="see-also"></a>参照  
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [システム ストアド プロシージャ &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
