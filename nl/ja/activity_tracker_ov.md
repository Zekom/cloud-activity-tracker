---

copyright:
  years: 2016, 2017

lastupdated: "2017-09-25"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Activity Tracker
{: #activity_tracker_ov}

{{site.data.keyword.cloudaccesstrailfull}} サービスを使用して、アプリケーションが {{site.data.keyword.Bluemix}} サービスとどのように対話するのかをトラッキングします。{{site.data.keyword.cloudaccesstrailshort}} を使用して異常なアクティビティーをモニターすることで、規制監査要件に準拠することができます。収集されるイベントは、Cloud Auditing Data Federation (CADF) 標準に準拠しています。
{:shortdesc}

* {{site.data.keyword.cloudaccesstrailshort}} は、クラウド内の IT リソースに対する高水準のセキュリティー・ガバナンスを実現します。
* {{site.data.keyword.cloudaccesstrailshort}} は、ネットワーク管理者が 1 つの場所で API アクティビティーの取り込み、保管、表示、検索、およびモニターを行えるようにするソリューションを提供します。
* {{site.data.keyword.cloudaccesstrailshort}} は、イベントをダウンロードする機能を提供します。ダウンロードしたイベントは監査証跡レポートを生成するために使用できます。組織が社内規定および外部の業界や国の規制に準拠するために、これらのレポートが必要になることがあります。

アプリケーションの実行場所がオンプレミス、ハイブリッド・クラウド、またはパブリック・クラウドのいずれであるかに関係なく、社内のポリシーおよび業界の規定に準拠することは、すべての組織の戦略にとって重要な要件です。{{site.data.keyword.cloudaccesstrailshort}} サービスは、API 呼び出しのモニターおよび証拠の作成を行って、企業のポリシーおよび市場の業界固有の規定に準拠するためのフレームワークおよび機能を提供します。

クラウド環境 ({{site.data.keyword.Bluemix_notm}} など) で作業をする場合、社内のポリシーと業界や国ベースの準拠要件に従って、ワークロードやデータの監査とモニターに関するクラウド戦略を立てる必要があります。{{site.data.keyword.cloudaccesstrailshort}} サービスを通じて登録された情報を使用することで、セキュリティー・インシデントを特定し、無許可アクセスを検出して、規制上および社内の監査要件に準拠することができます。

例えば、{{site.data.keyword.cloudaccesstrailshort}} アクティビティー・ログを使用して、以下の情報を識別できます。

* クラウド・サービスに対して API 呼び出しを行ったユーザー。
* API 呼び出しが行われたソース IP アドレス。
* API 呼び出しが行われたタイム・スタンプ。
* API 呼び出しの状況。


## Bluemix での Activity Tracker のプロビジョン
{: #provision}

スペース内で実行中のクラウド・サービスに対する API アクティビティーをモニターしたい {{site.data.keyword.Bluemix_notm}} アカウントの各スペース内で {{site.data.keyword.cloudaccesstrailshort}} サービスをプロビジョンする必要があります。

{{site.data.keyword.cloudaccesstrailshort}} サービスをプロビジョンする方法について詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} サービスのプロビジョン](/docs/services/cloud-activity-tracker/how-to/provision.html#provision)を参照してください。



## アクティビティー・ログの収集
{: #collect}

{{site.data.keyword.cloudaccesstrailshort}} サービスは、{{site.data.keyword.Bluemix_notm}} 内の選択されたクラウド・サービスに対して行われる API 呼び出しおよびその他のアクションに関連したアクティビティー・データのみを取り込みます。サービスのリストについては、[サポートされるクラウド・サービス](/docs/services/cloud-activity-tracker/cloud_services.html#cloud_services)を参照してください。

* イベントは自動的に収集されます。 
* {{site.data.keyword.cloudaccesstrailshort}} で収集されるイベントは、Cloud Auditing Data Federation (CADF) 標準に準拠しています。CADF 標準は、クラウド環境にあるアプリケーションのセキュリティーの認証、管理、および監査に必要な情報が含まれた、フルイベント・モデルを定義しています。

CADF イベント・モデルには、以下のコンポーネントが含まれています。

<table>
  <caption>表 1. CADF イベント・モデル内のコンポーネント</caption>
  <tr>
    <th>コンポーネント</th>
	<th>説明</th>
  </tr>
  <tr>
    <td>アクション</td>
	<td>アクションは、イニシエーターが実行する、実行を試みる、あるいは完了を待っている、操作またはアクティビティーです。</td>
  </tr>
  <tr>
    <td>イニシエーター</td>
	<td>イニシエーターは、API 呼び出しを行って CADF イベントを生成するリソースです。起動されるイベントは、API 呼び出しで要求されるアクションによって異なります。</td>
  </tr>
  <tr>
    <td>オブザーバー</td>
	<td>オブザーバーは、CADF イベントで利用可能な情報から CADF レコードを作成して保管するリソースです。</td>
  </tr>
  <tr>
    <td>結果</td>
	<td>結果は、ターゲットに対するアクションの状況です。</td>
  </tr>
  <tr>
    <td>ターゲット</td>
	<td>ターゲットは、アクションが実行される、実行を試みられる、あるいは完了の処理待ち中の、対象のリソースです。</td>
  </tr>
</table>


{{site.data.keyword.IBM_notm}} パブリック・クラウド内の {{site.data.keyword.cloudaccesstrailshort}} ログを使用するときには、以下の事項を考慮してください。

* {{site.data.keyword.IBM_notm}} パブリック・クラウドで実行されるリソースに対する API 呼び出しの監査レコードのみを保管できます。
* {{site.data.keyword.IBM_notm}} パブリック・クラウドのストレージのみがイベント収集のために使用されます。
* 情報は 3 日間保管されます。その後、情報は先入れ先出し法に基づいて削除されます。
* タイプが*アクティビティー* である CADF イベントが {{site.data.keyword.cloudaccesstrailshort}} サービスによってサポートされます。



## アクティビティー・ログの分析
{: #analyze}

アクティビティー・ログの分析は、{{site.data.keyword.Bluemix_notm}} 内で {{site.data.keyword.cloudaccesstrailshort}} UI を介して行うか、オープン・ソース・ツールである Kibana を使用して行うことができます。特定のスペースで使用可能なイベントをモニターするか、アカウント・レベルで使用可能なイベントをモニターすることができます。

{{site.data.keyword.Bluemix_notm}} 内で {{site.data.keyword.cloudaccesstrailshort}} UI を介して、過去 24 時間のアクティビティー・ログの検索、分析、およびモニターを行うことができます。詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} UI へのナビゲート](/docs/services/cloud-activity-tracker/how-to/manage-events-ui/launch_at_ui.html#launch_at_ui)を参照してください。

{{site.data.keyword.cloudaccesstrailshort}} Kibana ダッシュボードを使用するか、または、ユーザー独自のカスタム・ダッシュボードを作成することによって、Kibana を介して過去 3 日間のアクティビティー・ログの検索、分析、およびモニターを行うことができます。**注:** この機能を使用できるのは、**プレミアム**・プランのユーザーです。

* Kibana の起動方法について詳しくは、[Kibana ダッシュボードへのナビゲート](/docs/services/cloud-activity-tracker/how-to/manage-events-ui/launch_kibana.html#launch_kibana)を参照してください。 
* Kibana でイベントを分析するために使用できるフィールドのリストについては、[{{site.data.keyword.cloudaccesstrailshort}} イベントのフィールド](/docs/services/cloud-activity-tracker/reference/at_event.html#at_event)を参照してください。



## 地域
{: #regions}

{{site.data.keyword.cloudaccesstrailshort}} サービスは、以下の地域で使用可能です。

* 米国南部
* 英国 (ベータ)


## サービス・プラン
{: #plan}

{{site.data.keyword.cloudaccesstrailshort}} サービスには複数のプランが用意されています。

プランは {{site.data.keyword.Bluemix_notm}} UI またはコマンド・ラインを使用して変更できます。プランのアップグレード、またはライト・プランへの切り替えは、いつでも実行できます。
{{site.data.keyword.Bluemix_notm}} でのサービス・プランのアップグレードについて詳しくは、[プランの変更](/docs/services/cloud-activity-tracker/plan/change_plan.html#change_plan)を参照してください。 

以下の表に、各サービス・プランで使用可能な機能の概要を示します。

<table>
    <caption>表 1. イベントの取り込み、イベントの保存、およびイベントのエクスポートの機能</caption>
      <tr>
        <th>プラン</th>
        <th>イベントの取り込み</th>
        <th>イベントの保存</th>
		<th>イベントのエクスポート</th>
      </tr>
      <tr>
        <td>ライト (デフォルト)</td>
        <td>いいえ</td>
        <td>過去 3 日間</td>
		<td>いいえ</td>
      </tr>
      <tr>
        <td>プレミアム</td>
        <td>はい</td>
        <td>構成可能な日数。</td>
		<td>はい</td>
      </tr>
</table>

<table>
    <caption>表 2. イベントの管理および表示の機能</caption>
      <tr>
        <th>プラン</th>
		<th>API</th>
		<th>CLI</th>
        <th>Kibana</th>
      </tr>
      <tr>
        <td>ライト (デフォルト)</td>
		<td>いいえ</td>
		<td>いいえ</td>
        <td>いいえ</td>
      </tr>
      <tr>
        <td>プレミアム</td>
		<td>はい</td>
		<td>はい</td>
        <td>はい</td>
      </tr>
</table>

**注:** イベント・ストレージの 1 カ月のコストは、請求サイクルの平均として計算されます。

## セキュリティー
{: #security}

{{site.data.keyword.cloudaccesstrailshort}} サービスを使用するときには、セキュリティーに関する以下の事項を考慮してください。

* {{site.data.keyword.cloudaccesstrailshort}} イベントを生成する IBM サービスは、{{site.data.keyword.IBM_notm}} クラウドのセキュリティー・ポリシーに従います。詳しくは、[Trust the security and privacy of IBM Cloud ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/learn-more/why-ibm-cloud/security/){: new_window} を参照してください。
* {{site.data.keyword.cloudaccesstrailshort}} サービスは、クラウド・サービスの状態を変更するユーザー開始アクションを取り込みます。この情報からデータベースまたはアプリケーションに直接アクセスすることはできません。
* 許可されたユーザーのみが {{site.data.keyword.cloudaccesstrailshort}} イベント・ログの表示およびモニターを行うことができます。各ユーザーは、{{site.data.keyword.Bluemix_notm}} での固有の ID によって識別されます。
