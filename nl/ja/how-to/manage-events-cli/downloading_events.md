---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# イベントのダウンロード
{: #downloading_events}

イベントをローカル・ファイルにダウンロードしたり、データを別のプログラムにパイプしたりできます。イベントのダウンロードは、1 つのセッションのコンテキスト内で行います。どのイベントがダウンロードされるのかはセッションによって指定されます。イベントのダウンロードが中断された場合、セッションは中断した箇所からのダウンロードの再開を可能にします。ダウンロードが完了した後、セッションを削除する必要があります。
{:shortdesc}

{{site.data.keyword.Bluemix_notm}} スペースで使用可能なイベントをローカル・ファイルにダウンロードするには、以下のステップを実行します。

## ステップ 1: Bluemix にログインする
{: #prereq}

始める前に、{{site.data.keyword.Bluemix_notm}} 地域、組織、および、{{site.data.keyword.cloudaccesstrailshort}} サービスがプロビジョンされたスペースにログインします。 

```
bx login -a Endpoint
```
{: codeblock}
	
ここで、*Endpoint* は、{{site.data.keyword.Bluemix_notm}} にログインするための URL です。この URL は地域ごとに異なります。
	
<table>
    <caption>{{site.data.keyword.Bluemix_notm}} にアクセスするためのエンドポイントのリスト</caption>
	<tr>
	  <th>地域</th>
	  <th>URL</th>
	</tr>
	<tr>
	  <td>米国南部</td>
	  <td>api.ng.bluemix.net</td>
	</tr>
</table>

表示される指示に従います。 

例えば、米国南部地域にログインするには、次のコマンドを実行します。
	
```
bx login -a api.ng.bluemix.net
```
{: codeblock}

次に、組織およびスペースを設定します。以下のコマンドを実行します。

```
bx target -o OrgName -s SpaceName
```
{: codeblock}

ここで、

* OrgName は、組織の名前です。
* SpaceName は、スペースの名前です。

## ステップ 2: 使用可能なイベントを識別する
{: #step2}

1. 使用可能なイベントを確認するには、`cf at status` コマンドを使用します。

    例えば、過去 2 週間の使用可能なイベントを表示するには、次のコマンドを実行します。

    ```
    $ bx cf at status
    ```
    {: codeblock}
    
    例えば、このコマンド実行の出力は以下のようになります。
    
    ```
    +------------+--------+-------+--------------------+------------+
    |    DATE    |  COUNT | SIZE  |       TYPES        | SEARCHABLE |
    +------------+--------+-------+--------------------+------------+
    | 2017-07-24 |    16  | 3020  | ActivityTracker    |   None     |
    +------------+--------+-------+--------------------+------------+
    | 2017-07-25 |   1224 | 76115 | ActivityTracker    |    All     |
    +------------+--------+-------+--------------------+------------+
    ```
    {: screen}

    **注:** {{site.data.keyword.cloudaccesstrailshort}} サービスは、協定世界時 (UTC) を使用するグローバル・サービスです。日付は UTC 日付で定義されます。現地時間の特定の 1 日のイベントを取得するために、複数の UTC 日のダウンロードが必要になることがあります。
	
	詳しくは、[cf at status](/docs/services/cloud-activity-tracker/cli/at_cli.html#status) を参照してください。


## ステップ 3: セッションを作成する
{: #step3}

ダウンロードに使用可能なイベント・データのスコープを定義するため、および、ダウンロードの状況を保持するために、セッションが必要です。 

セッションを作成するには、コマンド [cf at session create](/docs/services/cloud-activity-tracker/cli/at_cli.html#session_create) を使用します。オプションで、セッションの作成時に開始日と終了日を指定できます。 

**注:** 開始日と終了日を指定すると、セッションはそれらの日付の間 (開始日と終了日を含む) のイベントへのアクセスを提供します。 

現在日付のイベントをダウンロードするために使用されるセッションを作成するには、次のコマンドを実行します。

```
bx cf at session create 
```
{: codeblock}

セッションは、以下の情報を返します。

* ダウンロードされる日付範囲。デフォルトは、現在の UTC 日付です。
* アカウント全体での使用可能なイベントを含めるのか、現行スペースでの使用可能なイベントのみを含めるのかについての情報。デフォルトでは、ログインしているスペースのイベントが取得されます。
* イベントをダウンロードするために必要なセッション ID。
* セッションを作成するユーザー ID。

以下に例を示します。

```
$ bx cf at session create 
+--------------+-------------------------------------------+
| Name         | Value                                     |
+--------------+-------------------------------------------+
| username     | xxx@yyy.com                               |
| create-time  | 2017-07-25T10:29:28.541092735Z            |
| access-time  | 2017-07-25T10:29:28.541092735Z            |
| date-range   | {"End":"2017-07-25","Start":"2017-07-25"} |
| type-account | {"Type":"ActivityTracker"}                |
| id           | 32c657c5-31c0-4a3c-a139-b380871c737a      |
| space        | 66fe4565-abab-46c9-cdcd-qf4565342345      |
+--------------+-------------------------------------------+
```
{: screen}

**ヒント:** アクティブ・セッションのリストを表示するには、コマンド [cf at session list](/docs/services/cloud-activity-tracker/cli/at_cli.html#session_list) を実行します。

以下に例を示します。

```
bx cf at session list
+--------------------------------------+--------------------------------------+---------------------+--------------------------------+--------------------------------+
| Id                                   | Space                                |Username             | Create-time                    | Access-time                    |
+--------------------------------------+--------------------------------------+---------------------+--------------------------------+--------------------------------+
| 32c657c5-31c0-4a3c-a139-b380871c737a | 66fe4565-abab-46c9-cdcd-qf4565342345 |xxx@yyy.com          | 2017-07-25T10:29:28.541092735Z | 2017-07-25T10:29:28.541092735Z |
+--------------------------------------+--------------------------------------+---------------------+--------------------------------+--------------------------------+
```
{: screen} 


## ステップ 4: イベントをファイルにダウンロードする
{: #step4}

セッション・パラメーターで指定されたイベントをダウンロードするには、次のコマンドを実行します。

```
bx cf at download -o Events_File_Name Session_ID
```
{: codeblock}

ここで、

* Events_File_Name は、出力ファイルの名前です。
* Session_ID は、セッションの GUID です。この値は前のステップで取得します。**Id** フィールドの値を使用します。

以下に例を示します。

```
bx cf at download -o Events_File_Name.log 32c657c5-31c0-4a3c-a139-b380871c737a
 29.89 KiB / 12.19 KiB [================================] 245.14% 9.73 MiB/s 0s
Download completed successfully
```
{: screen}

進行状況表示は、イベントがダウンロードされるに従って 0% から 100% に進んでいきます。

**注:** 

* ダウンロードされたデータのフォーマットは、圧縮 JSON です。例えば、Linux システムでイベントをダウンロードする場合、.gz ファイルを unzip し、それを開いて JSON フォーマットのデータを表示します。 
* 圧縮 JSON は、ElasticSearch または Logstash による取り込みに適しています。例えば、Linux システムで作業していて、-o が指定されていない場合、データは stdout に出力されるので、自分の Elastic スタックに直接パイプすることができます。
* JSON を構文解析できる任意のプログラムでデータを処理することもできます。 

## ステップ 4: セッションを削除する

ダウンロードが完了した後、[cf at session delete](/docs/services/cloud-activity-tracker/cli/at_cli.html#session_delete) コマンドを使用してセッションを削除する必要があります。 

セッションを削除するには、次のコマンドを実行します。

```
bx cf at session delete Session_ID
```
{: codeblock}

ここで、Session_ID は、前のステップで作成したセッションの GUID です。**Id** フィールドの値を使用します。

以下に例を示します。

```
bx cf at session delete 32c657c5-31c0-4a3c-a139-b380871c737a
+---------+------------------------+
| Name    | Value                  |
+---------+------------------------+
| message | Delete session success |
+---------+------------------------+
```
{: screen}



