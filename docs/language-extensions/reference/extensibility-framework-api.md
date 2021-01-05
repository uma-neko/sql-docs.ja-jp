---
title: 機能拡張フレームワーク API
titleSuffix: SQL Server Language Extensions
description: 機能拡張フレームワークを使用すると、SQL Server のプログラミング言語拡張機能を記述できます。 Microsoft SQL Server 用の機能拡張フレームワーク API は、SQL Server とやりとりしたり、データを交換したりするために言語拡張機能で使用できる API です。
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 3cc4e75f044476579859443b6a7407d01c3e92ea
ms.sourcegitcommit: afb02c275b7c79fbd90fac4bfcfd92b00a399019
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2020
ms.locfileid: "91956863"
---
# <a name="extensibility-framework-api-for-sql-server"></a>SQL Server 用の機能拡張フレームワーク API
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

機能拡張フレームワークを使用すると、SQL Server のプログラミング言語拡張機能を記述できます。 Microsoft SQL Server 用の機能拡張フレームワーク API は、SQL Server とやりとりしたり、データを交換したりするために言語拡張機能で使用できる API です。

言語拡張機能の作成者は、このリファレンスをオープンソースの言語拡張機能と共に使用して、独自のものを記述するための API の使用方法を理解することができます。 言語拡張機能のソース コードは、[aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions) で見つけることができます。

すべての API 関数の構文と引数に関する情報については、以下を参照してください。

## <a name="return-value"></a>戻り値

すべての関数からは、*SQLRETURN* パラメーターが返されます。 値が *SQL_SUCCESS* 以外の場合はエラーとして扱われ、スクリプトの実行が停止します。

## <a name="standard-output"></a>標準出力

拡張機能による標準出力ストリームまたはエラー ストリームへの出力は、セッションのログ ファイルにトレースされ、最終的には SQL Server までトレースされます (SSMS の [メッセージ] タブに表示されるものと同様)。


## <a name="init"></a>Init

この関数は 1 回だけ呼び出され、実行用にランタイムを初期化するために使用されます。 

### <a name="syntax"></a>構文

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>引数

*ExtensionParams*  
\[入力\] [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) または [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md) で指定した `PARAMETERS` 値を含む、null で終了する文字列。

*ExtensionParamsLength*  
\[入力\] *ExtensionParams* のバイト単位の長さ (null 終端文字を除く)。

*ExtensionPath*  
\[入力\] 拡張機能のインストール ディレクトリの絶対パスを含む、null で終了する UTF-8 文字列。

*ExtensionPathLength*  
\[入力\] *ExtensionPath* のバイト単位の長さ (null 終端文字を除く)。

*PublicLibraryPath*  
\[入力\] この外部言語のパブリック外部ライブラリ ディレクトリの絶対パスを含む、null で終了する UTF-8 文字列。

*PublicLibraryPathLength*  
\[入力\] *PublicLibraryPath* のバイト単位の長さ (null 終端文字を除く)。

*PrivateLibraryPath*  
\[入力\] このユーザーとこの外部言語のプライベート外部ライブラリ ディレクトリの絶対パスを含む、null で終了する UTF-8 文字列。

*PrivateLibraryPathLength*  
\[入力\] *PrivateLibraryPath* のバイト単位の長さ (null 終端文字を除く)。

## <a name="initsession"></a>InitSession

この関数は、セッションごとに 1 回呼び出され、セッション固有の設定を初期化します。

### <a name="syntax"></a>構文

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*[スクリプト]*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` を含む、null で終了する UTF-8 文字列。

*ScriptLength*  
\[入力\] *ScriptScript* のバイト単位の長さ (null 終端文字を除く)。

*InputSchemaColumnsNumber*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1` の結果セットの列数。

*ParametersNumber*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@params` からの入力パラメーターの数。

*InputDataName*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1_name` を含む、null で終了する UTF-8 文字列。

*InputDataNameLength*  
\[入力\] *InputDataName* のバイト単位の長さ (null 終端文字を除く)。

*OutputDataName*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@output_data_1_name` を含む、null で終了する UTF-8 文字列。

*OutputDataNameLength*  
\[入力\] *OutputDataName* のバイト単位の長さ (null 終端文字を除く)。

## <a name="initcolumn"></a>InitColumn

特定のセッションの指定された列の情報を初期化します。

この関数は、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1` の結果セットの各列に対して呼び出されます。

この結果セットの列構造を "*入力スキーマ*" と呼びます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*ColumnNumber*  
\[入力\] 入力スキーマにおけるこの列のインデックスを識別する整数。 列には 0 から始まる昇順の連続番号が付けられます。

*[ColumnName]*  
\[入力\] 列の名前を含む、null で終了する UTF-8 文字列。

*ColumnNameLength*  
\[入力\] *ColumnName* のバイト単位の長さ (null 終端文字を除く)。

*DataType*  
\[入力\] この列のデータ型を識別する ODBC C 型。

*ColumnSize*  
\[入力\] この列の基になるデータの最大サイズ (バイト単位)。

データ型が SQL_C_CHAR、SQL_C_WCHAR、SQL_C_BINARY の場合、8,000 より大きい値は、この列が LOB オブジェクトを表し、サイズが最大 2 GB であることを示します。

*DecimalDigits*  
\[入力\] この列の基になるデータの小数点以下の桁数 ([小数点以下の桁数](../../odbc/reference/appendixes/decimal-digits.md)に関する説明を参照)。

*NULL 値の使用*  
\[入力\] この列に NULL 値を含めることができるかどうかを示す値。 指定できる値

- SQL_NO_NULLS: 列に NULL 値を含めることはできません。
- SQL_NULLABLE: 列に NULL 値を含めることができます。

*PartitionByNumber*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内のこの列の `@input_data_1_partition_by_columns` シーケンスにおけるインデックスを示す値。 列には 0 から始まる昇順の連続番号が付けられます。 この列がシーケンスに含まれていない場合、値は -1 になります。

*OrderByNumber*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内のこの列の `@input_data_1_order_by_columns` シーケンスにおけるインデックスを示す値。 列には 0 から始まる昇順の連続番号が付けられます。 この列がシーケンスに含まれていない場合、値は -1 になります。

## <a name="initparam"></a>InitParam

特定のセッションの指定された入力パラメーターに関する情報を初期化します。

この関数は、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@params` の各パラメーターに対して呼び出されます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*ParamNumber*  
\[入力\] このパラメーターのインデックスを識別する整数。 パラメーターには 0 から始まる昇順の連続番号が付けられます。

*ParamName*  
\[入力\] パラメーターの名前を含む、null で終了する UTF-8 文字列。

*ParamNameLength*  
\[入力\] *ParamName* のバイト単位の長さ (null 終端文字を除く)。

*DataType*  
\[入力\] このパラメーターのデータ型を識別する ODBC C 型。

*ParamSize*  
\[入力\] このパラメーターの基になるデータの最大サイズ (バイト単位)。

データ型が SQL_C_CHAR、SQL_C_WCHAR、SQL_C_BINARY の場合、8,000 より大きい値は、このパラメーターが LOB オブジェクトを表し、サイズが最大 2 GB であることを示します。

*DecimalDigits*  
\[入力\] このパラメーターの基になるデータの小数点以下の桁数 ([小数点以下の桁数](../../odbc/reference/appendixes/decimal-digits.md)に関する説明を参照)。

*ParamValue*  
\[入力\] パラメーターの値を含むバッファーへのポインター。

*StrLen_or_Ind*  
\[入力\] *ParamValue* の長さ (バイト単位) を示す整数値、またはデータが NULL であることを示す SQL_NULL_DATA。

列が null 許容でなく、次のいずれかのデータ型を表していない場合、StrLen_or_Ind\[col\] は無視できます: SQL_C_CHAR、SQL_C_WCHAR、SQL_C_BINARY、SQL_C_NUMERIC、SQL_C_TYPE_TIMESTAMP。 それ以外の場合は、\[RowsNumber\] 要素を含む有効な配列を指します。各要素には、長さまたは null インジケーターのデータが含まれます。

*InputOutputType*  
\[入力\] パラメーターの型。 *InputOutputType* 引数は、次にいずれかの値になります。

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>実行

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` を実行します。

この関数は複数回呼び出すことができます。 各ストリーム チャンクにつき 1 回と、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1_partition_by_columns` の各パーティションに対して 1 回です。


### <a name="syntax"></a>構文

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*RowsNumber*  
\[入力\] *Data* 内の行の数。

*データ*  
\[入力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1` の結果セットを含む 2 次元配列。

列の合計数は、[InitSession](#initsession) 呼び出しで受け取った *InputSchemaColumnsNumber* です。 各列には、[InitColumn](#initcolumn) の列の型に従って解釈される *RowsNumber* 要素が含まれます。

*StrLen_or_Ind* で NULL と指定された要素は有効であるとは限らないため、無視する必要があります。

*StrLen_or_Ind*  
\[入力\] *Data* 内にある各値の長さまたは NULL インジケーターを含む 2 次元配列。 各セルの使用可能な値:

- n (n > 0)。 データの長さ (バイト単位) を示します。
- SQL_NULL_DATA (NULL 値を示します)。

列の合計数は、[InitSession](#initsession) 呼び出しで受け取った *InputSchemaColumnsNumber* です。 各列には、[InitColumn](#initcolumn) の列の型に従って解釈される *RowsNumber* 要素が含まれます。

列が null 許容でなく、次のいずれかのデータ型を表していない場合、StrLen_or_Ind\[col\] は無視できます: SQL_C_CHAR、SQL_C_WCHAR、SQL_C_BINARY、SQL_C_NUMERIC、SQL_C_TYPE_TIMESTAMP。 それ以外の場合は、*RowsNumber* 要素を含む有効な配列を指します。各要素には、長さまたは null インジケーターのデータが含まれます。

*OutputSchemaColumnsNumber*  
\[出力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` の予想される結果セット内の列数を返すバッファーへのポインター。

## <a name="getresultcolumn"></a>GetResultColumn

特定のセッションの指定された出力列に関する情報を取得します。

この関数は、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` の結果セットの各列に対して呼び出されます。
この結果セットの列構造を '出力スキーマ' と呼びます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*ColumnNumber*  
\[入力\] 出力スキーマにおけるこの列のインデックスを識別する整数。 列には 0 から始まる昇順の連続番号が付けられます。

*DataType*  
\[出力\] この列のデータ型を識別する ODBC C 型を含むバッファーへのポインター。

*ColumnSize*  
\[出力\] この列の基になるデータの最大サイズ (バイト単位) を含むバッファーへのポインター。

*DecimalDigits*  
\[出力\] この列の基になるデータの小数点以下の桁数 ([小数点以下の桁数](../../odbc/reference/appendixes/decimal-digits.md)に関する説明を参照) を含むバッファーへのポインター。 小数点以下の桁数を特定できない場合、または適用できない場合、値は破棄されます。

*NULL 値の使用*  
\[出力\] この列に NULL 値を含めることができるかどうかを示す値を含むバッファーへのポインター。 指定できる値

- SQL_NO_NULLS: 列に NULL 値を含めることはできません。
- SQL_NULLABLE: 列に NULL 値を含めることができます。

他の値が渡された場合、実行は停止します。

## <a name="getresults"></a>GetResults

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` を実行した結果セットを取得します。

この関数は複数回呼び出すことができます。 各ストリーム チャンクにつき 1 回と、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@input_data_1_partition_by_columns` の各パーティションに対して 1 回です。


### <a name="syntax"></a>構文

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER***   StrLen_or_Ind
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*RowsNumber*  
\[出力\] *Data* 内の行数を含むバッファーへのポインター。

*データ*  
\[出力\] [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@script` の結果セットを含む、拡張機能によって割り当てられる 2 次元配列へのポインター。

列の合計数は、[Execute](#execute) 呼び出しで取得された *OutputSchemaColumnsNumber* とする必要があります。 各列には、[GetResultColumn](#getresultcolumn) の列の型に従って解釈される *RowsNumber* 要素が含まれている必要があります。

*StrLen_or_Ind*  
\[出力\] *Data* 内にある各値の長さまたは NULL インジケーターを含む、拡張機能によって割り当てられる 2 次元配列へのポインター。 各セルの使用可能な値:

- n (n > 0)。 データの長さ (バイト単位) を示します。
- SQL_NULL_DATA (NULL 値を示します)。

列の合計数は、[Execute](#execute) 呼び出しで受け取った *OutputSchemaColumnsNumber* とする必要があります。 各列には、[GetResultColumn](#getresultcolumn) の列の型に従って解釈される *RowsNumber* 要素が含まれます。

列が null 許容でなく、次のいずれかのデータ型を表していない場合、StrLen_or_Ind\[col\] は無視されます: SQL_C_CHAR、SQL_C_WCHAR、SQL_C_BINARY [add dates]。 それ以外の場合は、*RowsNumber* 要素を含む有効な配列を指します。各要素には、長さまたは null インジケーターのデータが含まれます。

## <a name="getoutputparam"></a>GetOutputParam

特定のセッションの指定された出力パラメーターに関する情報を取得します。

この関数は、[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の OUTPUT でマークされた `@params` の各パラメーターに対して呼び出されます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*ParamValue*  
\[出力\] パラメーターの値を含むバッファーへのポインター。

*StrLen_or_Ind* \[出力\] *ParamValue* の長さをバイト単位で示す整数値を含むバッファーへのポインター、またはデータが NULL であることを示す SQL_NULL_DATA。

## <a name="getinterfaceversion"></a>GetInterfaceVersion

インターフェイスのバージョンを取得します。
この関数からは、拡張機能のインターフェイスのバージョンを表す整数が返されます。 サポートされる値:
1. バージョン 1 は初期 API バージョンです。 SQL Server 2019 RTM でサポートされています。
1. バージョン 2 では InstallExternalLibrary と UninstallExternalLibrary API のサポートが追加され、SQL Server 2019 CU3 からサポートされています。                            

### <a name="syntax"></a>構文

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

セッションごとの情報をクリーンアップします。

### <a name="syntax"></a>構文

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

## <a name="cleanup"></a>クリーンアップ

グローバルな共有情報 (JVM など) をクリーンアップします。

### <a name="syntax"></a>構文

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

拡張機能からテレメトリ (キーと値のペア) データを取得します。 この関数はオプションであり、実装する必要はありません。 テレメトリは `dm_db_external_script_execution_stats` 動的管理ビュー (DMV) によって公開されます。

script_executions という名前のカウンターがあります。これはフレームワークによって送信されます。 拡張機能では、この名前を使用しないでください。

各テレメトリ エントリはキーと値のペアです。 キーは文字列で、値は 64 ビットの整数 (カウンター) です。 したがって、出力は 2 つの論理配列 (名前とそれに対応するカウンター) で構成されます。 各配列は出力です。

各配列の長さは、出力である *RowsNumber* です。 最初の論理出力は、文字列へのポインターを含むため、2 つの配列で表されます。*CounterNames* (実際の文字列データ) と *CounterNamesLength* (各文字列の長さ) です。 2 番目の論理出力は *CounterValues* ポインターに格納されます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>引数

*SessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*TaskId*  
\[入力\] この実行プロセスを一意に識別する整数。

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 内の `@parallel = 1` の場合、この値の範囲は、0 からクエリの並列処理の次数までです。

*RowsNumber*  
\[出力\] キーと値のペアの数。

*CounterNames*  
\[出力\] キーを含む文字列データ。

*CounterNamesLength*  
\[出力\] 各キー文字列の長さ。

*CounterValues*  
\[出力\] 値を含む 64 ビットの整数データ。

## <a name="installexternallibrary"></a>InstallExternalLibrary

ライブラリをインストールします。 この関数はオプションであり、実装する必要はありません。 既定の実装では、ライブラリの内容を適切な場所のファイルにコピーします ([CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) を参照)。 ファイル名はライブラリ名です。

### <a name="syntax"></a>構文

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>引数

*SetupSessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*LibraryName*  
\[入力\] ライブラリ名。

*LibraryNameLength*  
\[入力\] ライブラリ名の長さ。

*LibraryFile*  
\[入力\] [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) によって指定されたるイナリの内容を含む、ライブラリ ファイルのパス (文字列)。

*LibraryFileLength*  
\[入力\] LibraryFile 文字列の長さ。

*LibraryInstallDirectory:*  
\[入力\] ライブラリをインストールするルート ディレクトリ。

*LibraryInstallDirectoryLength*  
\[入力\] LibraryInstallDirectory 文字列の長さ。

*LibraryError*  
\[出力\] オプションの出力パラメーター。 ライブラリのインストール中にエラーが発生した場合は、LibraryError によって、エラーを説明する文字列がポイントされます。

*LibraryErrorLength*  
\[出力\] LibraryError 文字列の長さ。

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

ライブラリをアンインストールします。 この関数はオプションであり、実装する必要はありません。 既定の実装では、InstallExternalLibrary の既定の実装によって行われた作業を元に戻します。 既定の実装では、*LibraryInstallDirectory* の下にある *LibraryName* ファイルの内容が削除されます。

### <a name="syntax"></a>構文

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>引数

*SetupSessionId*  
\[入力\] このスクリプト セッションを一意に識別する GUID。

*LibraryName*  
\[入力\] ライブラリ名

*LibraryNameLength*  
\[入力\] ライブラリ名の長さ

*LibraryFile*  
\[入力\] CREATE EXTERNAL LIBRARY によって指定されたバイナリの内容を含むライブラリ ファイルのパス (文字列)

*LibraryFileLength*  
\[入力\] LibraryFile 文字列の長さ

*LibraryInstallDirectory*  
\[入力\] ライブラリをインストールするルート ディレクトリ

*LibraryInstallDirectoryLength*  
\[入力\] LibraryInstallDirectory 文字列の長さ。

*LibraryError*  
\[出力\] ライブラリ エラー文字列。

*LibraryErrorLength*  
\[出力\] LibraryError 文字列の長さ。

## <a name="next-steps"></a>次のステップ

- [SQL Server 用の Microsoft Extensibility SDK for Java](../how-to/extensibility-sdk-java-sql-server.md)
- [Python カスタム ランタイム](../../machine-learning/install/custom-runtime-python.md)
- [R カスタム ランタイム](../../machine-learning/install/custom-runtime-r.md)