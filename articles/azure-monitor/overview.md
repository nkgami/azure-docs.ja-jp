---
title: Azure Monitor の概要 | Microsoft Docs
description: Azure のサービスおよびアプリケーションの包括的な監視戦略に役立つ Microsoft のサービスと機能の概要。
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: bwren
ms.openlocfilehash: 97cc6d6e922ab0bedd729eddca754ae016f041bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958018"
---
# <a name="azure-monitor-overview"></a>Azure Monitor の概要

Azure Monitor は、クラウドおよびオンプレミス環境の利用統計情報を収集、分析し、それに対応する包括的なソリューションを提供することにより、アプリケーションの可用性とパフォーマンスを最大化します。 このツールは、ご利用のアプリケーションがどのように実行されているかを把握するのに役立ちます。さらに、このツールにより、そのアプリケーションに影響している問題点およびアプリケーションが依存しているリソースを事前に明らかにしておくことができます。

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>概要
次の図は、Azure Monitor の概要を示します。 図の中央には、Azure Monitor が使用するデータの 2 つの基本的な型であるメトリックとログを格納するデータ ストアがあります。 左側には、さまざまな監視対象リソースから利用統計情報を収集し、[データ ストア](../monitoring/monitoring-data-collection.md)に格納する[ソース](../monitoring/monitoring-data-sources.md)があります。 右側には、Azure Monitor が収集したデータを使って実行するさまざまな機能があります。たとえば、分析、アラート、および外部システムへのストリーミングなどです。


![Azure Monitor の概要](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>データ プラットフォームの監視
Azure Monitor が収集したすべてのデータは、2 つの基本的な型である[メトリックとログ](../monitoring/monitoring-data-collection.md)のどちらかに該当します。 [メトリック](../monitoring/monitoring-data-collection.md#metrics)は、特定の時点におけるシステムの何らかの側面を表す数値です。 メトリックは軽量であり、リアルタイムに近いシナリオをサポートできます。 [ログ](../monitoring/monitoring-data-collection.md#logs)には、種類ごとに異なるプロパティ セットを持つレコードに編成されたさまざまな種類のデータが含まれます。 イベントやトレースなどの利用統計情報は、組み合わせて分析できるように、パフォーマンス データとともにログとして格納されます。

Azure Monitor が収集した多くの Azure リソースのデータは、Azure portal の [Overview]\(概要\) ページで参照できます。 たとえば、任意の仮想マシンを見てみると、パフォーマンス メトリックが表示されたいくつかのグラフが表示されます。 いずれかのグラフをクリックすると、データが Azure portal の[メトリック エクスプローラー](../monitoring-and-diagnostics/monitoring-metric-charts.md)で開かれ、時系列で複数のメトリックの値を表示するグラフを確認できます。  グラフは、対話形式で表示したり、ダッシュボードにピン留めして他の視覚化と一緒に表示したりできます。

![メトリック](media/overview/metrics.png)

Azure Monitor が収集したログ データは、Log Analytics に格納されます。これには、収集されたデータを高速に検索し、統合して、分析する[多機能なクエリ言語](../log-analytics/log-analytics-queries.md)が備わっています。  クエリの作成とテストは、Azure portal の [Log Analytics ページ](../log-analytics/log-analytics-log-search-portals.md)を使って行うことができます。それらのツールを使って直接データを分析できるほか、クエリを保存して[視覚化](visualizations.md)したり、[アラート ルール](../monitoring-and-diagnostics/monitoring-overview-alerts.md)に利用したりすることができます。

Log Analytics クエリ言語は、単純なログ検索に適している一方で、集計、結合、スマート分析など高度な機能も備えています。 提供されている[さまざまなレッスン](../log-analytics/query-language/get-started-queries.md)を利用すれば、クエリ言語はすぐに覚えることができます。  既に [SQL](../log-analytics/query-language/sql-cheatsheet.md) や [Splunk](../log-analytics/query-language/splunk-cheatsheet.md) に習熟しているユーザーには、別途ガイダンスが用意されています。

![ログ](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Azure Monitor が収集するデータ
Log Analytics はさまざまなソースからデータを収集できます。 アプリケーションやそれが依存する運用やサービスから、プラットフォーム自体に至るまで、アプリケーションのさまざまな階層のデータ監視を検討することができます。 Azure Monitor は、以下のそれぞれの層からデータを収集します。

- **アプリケーション監視データ:** プラットフォームを問わず、記述したコードのパフォーマンスと機能に関するデータ。
- **ゲスト OS 監視データ:** アプリケーションが実行されているオペレーティング システムに関するデータ。 これは Azure、別のクラウド、またはオンプレミスで実行できます。 
- **Azure リソース監視データ:** Azure リソースの操作に関するデータ。
- **Azure サブスクリプション監視データ:** Azure サブスクリプションの操作および管理に関するデータと、Azure 自体の正常性および操作に関するデータ。 
- **Azure テナントの監視データ:** Azure Active Directory など、テナント レベルの Azure サービスの操作に関するデータ。

Azure サブスクリプションを作成して仮想マシンや Web アプリなどのリソースを追加すると、Azure Monitor は即座にデータの収集を開始します。  リソースが作成または変更されると、[アクティビティ ログ](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)が記録されます。 リソースの状況や消費しているリソースは、[メトリック](../monitoring-and-diagnostics/monitoring-overview-metrics.md)からわかります。 

データの収集をリソースの実運用にまで拡張するには、[診断を有効](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)にしてコンピューティング リソースに[エージェントを追加](../log-analytics/log-analytics-agent-windows.md)します。 これによって、リソースの内部操作に関する利用統計情報が収集されるようになり、さまざまな[データ ソース](../log-analytics/log-analytics-data-sources.md)を構成して Windows および Linux ゲスト オペレーティング システムからログやメトリックを収集することができます。 

[アプリケーションにインストルメンテーション パッケージを追加](../application-insights/app-insights-azure-web-apps.md)すると、Application Insights が有効になり、ページ ビュー、アプリケーションの要求、例外など、アプリケーションに関する詳細情報を収集できます。 さらにアプリケーションの可用性を検証するには、ユーザー トラフィックをシミュレートする[可用性テスト](../application-insights/app-insights-monitor-web-app-availability.md)を構成します。

### <a name="custom-sources"></a>カスタム ソース
Azure Monitor では、[Data Collector API](../log-analytics/log-analytics-data-collector-api.md) を使用して任意の REST クライアントからログ データを収集できます。 これにより、カスタム監視シナリオを作成し、他のソースに利用統計情報を公開していないリソースにまで監視を拡張することができます。



## <a name="insights"></a>洞察
データの監視が役立つのは、コンピューティング環境の運用の可視性が高まる場合のみです。 Azure Monitor には、アプリケーションやそれが依存するリソースに対する貴重な洞察を提供するいくつかの機能やツールが含まれています。 [監視ソリューション](../monitoring/monitoring-solutions.md)や [Application Insights](../application-insights/app-insights-overview.md) および Container Insights などの機能は、アプリケーションや特定の Azure サービスのさまざまな側面に対する深い洞察を提供します。 

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) は、クラウドとオンプレミスのどちらにホストされているかにかかわらず、Web アプリケーションの可用性、パフォーマンス、使用状況を監視します。 Log Analytics の強力なデータ分析プラットフォームを利用し、ユーザーの報告を待つことなく、アプリケーション運用やエラー診断に対する深い洞察を提供します。 Application Insights にはさまざまな開発ツールへの接続ポイントが含まれており、DevOps プロセスをサポートする Visual Studio とも統合されています。

![App Insights](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>コンテナーに対する Azure Monitor
コンテナーに対する Azure Monitor は、Azure Kubernetes Service (AKS) にホストされたマネージド Kubernetes クラスターにデプロイされているコンテナー ワークロードのパフォーマンスを監視するために設計された機能です。 Kubernetes で使用可能なコントローラー、ノード、およびコンテナーから Metrics API 経由でメモリやプロセッサ メトリックを収集することにより、パフォーマンスを可視化します。 コンテナーのログも収集されます。  Kubernetes クラスターから監視を有効化すると、コンテナー化されたバージョンの Linux 向けの Log Analytics エージェントを使用してこれらのメトリックとログが自動的に収集され、Log Analytics に保存されます。

![コンテナー正常性](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>VM に対する Azure Monitor
Azure Monitor VM Insights は、別のプロセスや、その他のリソースおよび外部プロセスと相互接続された依存関係まで含め、Windows および Linux VM のパフォーマンスと正常性を分析することによって、Azure 仮想マシン (VM) の大規模な監視を行います。 このソリューションでは、オンプレミスまたは別のクラウド プロバイダーにホストされた VM に対するパフォーマンスの監視およびアプリケーションの依存関係もサポートされています。  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>監視ソリューション
Azure Monitor の[監視ソリューション](../monitoring/monitoring-solutions.md)は、特定のアプリケーションまたはサービスに関する洞察を提供するロジックを 1 つにまとめたものです。 [クエリ](../log-analytics/log-analytics-queries.md)による分析と[ビュー](../log-analytics/log-analytics-view-designer.md)による視覚化を使用して、その他の監視データとともに Log Analytics にデータを収集します。 Azure サービスおよび他のアプリケーションを監視するさまざまな監視ソリューションが [Microsoft とパートナーから提供](../monitoring/monitoring-solutions-inventory.md)されています。

![監視ソリューション](../monitoring/media/monitoring-solutions/overview.png)

## <a name="responding-to-critical-situations"></a>重大な状況への応答
効果的な監視ソリューションは、対話形式で監視データを分析できるだけでなく、収集したデータから危機的な状況を識別して事前に応答できる必要があります。 たとえば、問題の調査を担当する管理者にテキストまたはメールを送信することが考えられます。 または、自動化されたプロセスを起動して、エラー状態の修正を試みることも考えられます。


### <a name="alerts"></a>アラート
[Azure Monitor のアラート](../monitoring-and-diagnostics/monitoring-overview-alerts.md)は、重大な状態を事前に通知し、可能であれば修正のためのアクションを実行しようとします。 メトリックに基づくアラート ルールは、数値に基づくリアルタイムに近いアラートを提供します。一方、ログに基づくルールを使うと、複数のソースのデータにまたがる複雑なロジックを実現できます。

Azure Monitor のアラート ルールは、受信者の一意なセットと複数のルール間で共有できるアクションを含む[アクション グループ](../monitoring-and-diagnostics/monitoring-action-groups.md)を使用します。 アクション グループを使うと、Webhook を使用してアラートから外部アクションを起動したり、ITSM ツールと統合したりするなど、要件に基づいたアクションを実行することができます。

![アラート](media/overview/alerts.png)

### <a name="autoscale"></a>自動スケール
自動スケールを使用すると、適切な量のリソースを実行して、アプリケーションに対する負荷を処理することができます。 Azure Monitor が収集したメトリックを使用して、負荷の増加に対応するために自動的にリソースを追加するタイミングや、アイドル状態にあるリソースを削除してコストを節約するタイミングを判別するルールを作成することができます。 インスタンスの最小数および最大数と、リソースを増減するタイミングを示すロジックを指定します。

![自動スケール](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>監視データの視覚化
グラフや表などによる[視覚化](visualizations.md)は、監視データをまとめてさまざまな対象ユーザーに提示する際に利用できる効果的なツールです。 Azure Monitor は、監視データを視覚化する独自の機能を備えており、他の Azure サービスを活用してさまざまな対象ユーザーに公開します。

### <a name="dashboards"></a>ダッシュボード
[Azure ダッシュボード](../azure-portal/azure-portal-dashboards.md)を使用すると、メトリックとログを含むさまざまな種類のデータを組み合わせて [Azure portal](https://portal.azure.com) 内の 1 つのウィンドウに表示できます。 ダッシュボードは、他の Azure ユーザーと共有することもできます。 Azure Monitor のすべての要素は、任意のログ クエリやメトリック グラフの出力と合わせて、Azure ダッシュ ボードに追加できます。 たとえば、メトリックのグラフ、アクティビティ ログの表、Application Insights の使用状況グラフ、Log Analytics のクエリ結果を組み合わせて 1 つのダッシュボードを作成できます。

![ダッシュボード](media/overview/dashboard.png)

### <a name="views"></a>ビュー
[Azure Monitor のビュー](../log-analytics/log-analytics-view-designer.md)は、Log Analytics のログ データを視覚的に表現します。  それぞれのビューには単一のタイルが含まれており、ドリルダウンすることで重要なデータを集約したリストのほか、棒グラフや折れ線グラフなど、さまざまな視覚化要素を表示できます。  監視ソリューションには、特定の用途向けにデータを集計したビューが含まれているほか、Log Analytics のログ検索から得たデータを表示する独自のビューを作成することもできます。 ビューは、Azure Monitor での他の要素と同様に、Azure ダッシュ ボードに追加できます。

![Log Analytics ビュー](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com)は、さまざまなデータ ソースを対話操作で視覚化するビジネス分析サービスで、データを組織内外のユーザーと効果的に共有することができます。 [Azure Monitor からログ データを自動的にインポート](../log-analytics/log-analytics-powerbi.md)するように Power BI を構成すると、そのデータも追加で視覚化することができます。


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>データの統合とエクスポート
多くの場合、監視データを使用するカスタム ソリューションを構築するために、Azure Monitor と他のシステムを統合したいという要件があります。 このような統合を実現するには、他の Azure サービスと Azure Monitor を連携させます。

### <a name="event-hub"></a>イベント ハブ
ストリーミング プラットフォームおよびイベント取り込みサービスである [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs) は、任意のリアルタイム分析プロバイダーやバッチ処理/ストレージ アダプターを使用して、データの変換と格納を行います。 Event Hubs を使用すると、パートナー SIEM および監視ツールに対して [Azure Monitor からログ データをストリーミング](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)できます。

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

### <a name="logic-apps"></a>Logic Apps
[Logic Apps](https://azure.microsoft.com/services/logic-apps) は、さまざまなシステムやサービスと統合されたワークフローを使用して、タスクやビジネス プロセスを自動化できるサービスです。 Azure Monitor でメトリックとログの読み取りおよび書き込みを行うアクティビティが利用できるので、他のさまざまなシステムと統合されたワークフローを構築することができます。

![ロジック アプリ](../log-analytics/media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="api"></a>API
Azure Monitor のメトリックとログの読み取りや書き込み、生成されたアラートのアクセスには、複数の API が利用できます。 アラートの構成や取得も可能です。 したがって、Azure Monitor と統合されたカスタム ソリューションを構築するために、ほぼあらゆることを行うことができます。

## <a name="next-steps"></a>次の手順
各項目の詳細情報

* [Azure Monitor](https://azure.microsoft.com/services/monitor/)。コアな監視のメトリックとアラートの使用を開始します。
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) (App Service Web アプリの問題を診断する場合)。
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/)。収集した監視データとログを分析する場合は、こちらをご覧ください。



