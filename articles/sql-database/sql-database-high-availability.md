---
title: 高可用性 - Azure SQL Database サービス | Microsoft Docs
description: Microsoft Azure SQL Database サービスの高可用性機能および特色について説明します。
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 7a60d800ce76f8ff9a903cc068fa7bc87cd33f3f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700637"
---
# <a name="high-availability-and-azure-sql-database"></a>高可用性と Microsoft Azure SQL Database

Azure SQL Database は、稼働率 99.99% が保証されている、メンテナンスやダウンタイムの心配が不要な、サービスとしての高可用性データベース プラットフォームです。 これは、Azure クラウドでホストされる、完全に管理された SQL Server データベース エンジン プロセスです。SQL Server データベースは、ワークロードに影響を与えることなく、常にアップグレードされ、パッチが適用されます。 インスタンスにパッチを適用するか、またはインスタンスがフェールオーバーされるときにアプリで[再試行ロジックを使用する](sql-database-develop-overview.md#resiliency)と、通常はダウンタイムが認識されることはありません。 フェールオーバーの完了までの時間が 60 秒を超える場合は、サポート ケースを開いてください。 Azure SQL Database は、クリティカルな状況であっても迅速な復旧が可能であるため、データが常に使用可能であることが保証されます。

すべての Azure SQL Database は Azure プラットフォームによって完全に管理され、データ消失ゼロおよび高いデータ可用性 (%) を実現しています。 基になるハードウェア、ソフトウェア、ネットワークの障害リスクへの対応や、パッチの適用、バックアップ、レプリケーション、障害検出、バグ修正、フェールオーバー、データベースのアップグレードなど、各種メンテナンス タスクは、Azure によって自動的に処理されます。 SQL Server のエンジニアは、定評のあるベスト プラクティスを導入しており、すべてのメンテナンス操作は、データベース運用期間の 0.01% 未満で完了します。 このアーキテクチャは、コミットされたデータが消失せず、メンテナンス操作を実行してもワークロードに影響が及ばないように設計されています。 データベースのアップグレードやメンテナンスを行うときでも、ワークロードの停止が必要なメンテナンス期間やダウンタイムは発生しません。 Azure SQL Database には、高可用性が組み込まれています。そのため、データベースがソフトウェア アーキテクチャの単一障害点にならないことが保証されています。

Azure SQL Database は、インフラストラクチャに障害が発生した場合でも 99.99% の可用性を確保するために、クラウド環境に合わせて調整された SQL Server データベース エンジン アーキテクチャに基づいています。 Azure SQL Database には、2 つの高可用性アーキテクチャ モデルがあります (いずれも 99.99% の可用性を確保しています)。
- 計算とストレージの分離に基づく Standard/汎用モデル。 このアーキテクチャ モデルは、ストレージ層の高可用性と信頼性に依存していますが、保守作業中はパフォーマンスが低下する可能性があります。
- データベース エンジン プロセスのクラスターに基づく Premium/Business Critical モデル。 このアーキテクチャ モデルは、常に使用可能なデータベース エンジン ノードのクォーラムが存在し、保守作業中でもワークロードに対するパフォーマンスの影響が最小限であるというファクトに依存しています。

Azure では、基盤となるオペレーティング システム、ドライバー、および SQL Server Database エンジンに対して、アップグレードとパッチ適用が透過的に実行されます。そのため、エンド ユーザーのダウンタイムは最小限に抑えられます。 Azure SQL データベースは、最新バージョンの SQL Server データベース エンジンおよび Windows OS 上で実行されています。また、アップグレードが、ほぼ気づかれることなく、継続的に実行されています。

## <a name="standardgeneral-purpose-availability"></a>Standard/General Purpose 可用性

Standard 可用性は、99.99% の SLA を指しており、Standard/Basic/General Purpose の階層で適用されます。 このアーキテクチャ モデルの高可用性は、計算層とストレージ層の分離と、ストレージ層でのデータのレプリケーションによって実現されます。

次の図は、計算レイヤーとストレージ レイヤーが分離されている Standard アーキテクチャ モデルの 4 ノードを示しています。

![計算とストレージの分離](media/sql-database-managed-instance/general-purpose-service-tier.png)

Standard 可用性モデルには、次の 2 つのレイヤーがあります。

- ステートレス計算レイヤー。sqlserver.exe プロセスを実行しており、一時データとキャッシュ データのみが含まれています (プラン キャッシュ、バッファー プール、列のストア プールなど)。 このステートレス SQL Server ノードは、プロセスの初期化、ノードの正常性の制御、および他の場所へのフェールオーバーを必要に応じて実行する Azure Service Fabric によって操作されます。
- ステートフル データ レイヤー。データベース ファイル (.mdf/.ldf) が Azure Premium Storage に保存されています。 Azure Storage では、データベース ファイル内にあるレコードのデータが消失しないことが保証されています。 Azure Storage には、データの可用性と冗長性が組み込まれています。そのため、たとえ SQL Server プロセスがクラッシュしても、ログ ファイルのレコードやデータ ファイルのページはすべて維持されます。

データベース エンジンまたはオペレーティング システムがアップグレードされた場合、基となるインフラストラクチャの一部で障害が発生した場合、または Sql Server プロセスで重大な問題が検出された場合、Azure Service Fabric は、ステートレス SQL Server プロセスを別のステートレス計算ノードに移行します。 フェールオーバー時間を最小限に抑えるために、フェールオーバーが発生した場合に新しい計算サービスの実行を待機している一連のスペア ノードがあります。 Azure Storage レイヤーのデータは影響を受けず、データ/ログ ファイルは、新しく初期化された SQL Server プロセスにアタッチされます。 このプロセスは、99.99% の可用性を保証していますが、移行時間や、新しい SQL Server ノードの起動にコールド キャッシュを使用することが原因で、実行中の大きなワークロードに対しては、パフォーマンス上の影響が若干生じる場合があります。

## <a name="premiumbusiness-critical-availability"></a>Premium/Business Critical 可用性

Premium 可用性は、Azure SQL Database の Premium 階層で実現されます。Premium 可用性は、メンテナンス操作中であってもパフォーマンスへの影響が許されない集中的なワークロードのために設計されています。

Premium モデルでは、Azure SQL Database が計算およびストレージを単一のノード上に統合しています。 このアーキテクチャ モデルの高可用性は、4 ノードの [Always On 可用性グループ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) クラスターにデプロイされた計算 (SQL Server データベース エンジン プロセス) とストレージ (ローカルに接続された SSD) のレプリケーションによって実現されます。

![データベース エンジン ノードのクラスター](media/sql-database-managed-instance/business-critical-service-tier.png)

SQL Server データベース エンジン プロセスと、基礎となる mdf/ldf ファイルは、両方とも同じノード上に配置されます。このノードには、SSD ストレージがアタッチされているため、ワークロードに対する待ち時間が短くなります。 高可用性を実装するときは、[Always On 可用性グループ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)が使用されます。 すべてのデータベースは、データベース ノードのクラスターになっています。このクラスターの中で、1 つのプライマリ データベースは、顧客のワークロード用にアクセスすることができ、3 つのセカンダリ プロセスは、データのコピーが格納されています。 プライマリ ノードは、変更内容を絶えずセカンダリ ノードにプッシュしています。これは、何らかの原因でプライマリ ノードがクラッシュした場合でも、セカンダリ レプリカでデータを確実に使用できるようにするためです。 フェールオーバーは、SQL Server データベース エンジンで処理されます。つまり、あるセカンダリ レプリカがプライマリ ノードになると、クラスター内のノード数を十分に確保するために、新しいセカンダリ レプリカが作成されます。 ワークロードは、新しいプライマリ ノードに自動的にリダイレクトされます。

さらに、Business Critical クラスターには、プライマリ ワークロードのパフォーマンスに影響を及ぼさない読み取り専用のクエリ (レポートなど) を実行するために使用できる組み込みの読み取り専用ノードが用意されています。 

## <a name="zone-redundant-configuration-preview"></a>ゾーン冗長の構成 (プレビュー)

既定では、ローカル ストレージ構成のクォーラムセット レプリカが同じデータセンターに作成されます。 [Azure 可用性ゾーン](../availability-zones/az-overview.md)の導入により、同じリージョンのさまざまな可用性ゾーンに対するさまざまなレプリカをクォーラムセットに配置することができます。 単一障害点をなくすため、制御リングも複数のゾーンで 3 つのゲートウェイ リング (GW) として複製できます。 特定のゲートウェイ リングへのルーティングは [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) によって制御されます。 ゾーン冗長構成ではデータベースの冗長性は追加されないため、Premium または Business Critical (プレビュー) サービス レベルの可用性ゾーンを追加費用なしで利用できます。 ゾーン冗長データベースを選択することで、アプリケーション ロジックをまったく変更せずに、データセンターの壊滅的な障害を含む大規模な障害から Premium または Business Critical (プレビュー) データベースが回復できるようになります。 また、既存の Premium または Business Critical (プレビュー) データベース、あるいはプール (プレビュー) をゾーン冗長構成に変換することもできます。

ゾーン冗長クォーラムセットは、離れた距離に位置するさまざまなデータセンターにレプリカがあるため、ネットワーク待機時間が長くなるとコミット時間が延長され、一部の OLTP ワークロードのパフォーマンスに影響する可能性があります。 いつでもゾーン冗長設定を無効にして単一ゾーン構成に戻ることができます。 このプロセスはデータ操作のサイズであり、通常のサービス レベル目標 (SLO) 更新プログラムと似ています。 プロセスの最後に、データベースまたはプールがゾーン冗長リングから単一ゾーン リングに (または逆方向に) 移行されます。

> [!IMPORTANT]
> ゾーン冗長データベースとエラスティック プールは、現時点では Premium サービス レベルでのみサポートされています。 パブリック プレビューでは、バックアップおよび監査レコードは RA-GRS ストレージに格納され、ゾーン全体の障害時に自動的に利用できない可能性があります。 

ゾーン冗長による高可用性アーキテクチャを、次の図に示します。
 
![高可用性アーキテクチャのゾーン冗長](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>読み取りスケールアウト
前述したように、Premium および Business Critical (プレビュー) サービス階層では、単一ゾーン構成でもゾーン冗長構成でも高可用性を実現するために、クォーラム セットと Always On テクノロジが活用されています。 AlwaysON 可用性グループの利点の 1 つは、レプリカが常に、トランザクション一貫性の保たれた状態になることです。 レプリカがプライマリと同じパフォーマンス レベルを備えているため、アプリケーションでは、追加料金なしで読み取り専用ワークロードを提供する追加容量 (読み取りスケールアウト) を利用できます。 これにより、読み取り専用クエリは、メインの読み取り/書き込みワークロードから分離され、パフォーマンスに影響を及ぼすことはありません。 読み取りスケールアウト機能は、分析などの論理的に分離された読み取り専用ワークロードを含むアプリケーションを対象としています。そのため、プライマリに接続することなくこの追加容量を活用できます。 

特定のデータベースで読み取りスケールアウト機能を使用するには、データベースを作成するときに明示的に有効にするか、PowerShell を使用して [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) または [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) コマンドレットを呼び出すか、Azure Resource Manager REST API から[データベース - 作成または更新](/rest/api/sql/databases/createorupdate)メソッドを使用して構成を変更し、後から明示的に有効にする必要があります。

データベースの読み取りスケールアウトが有効になると、そのデータベースに接続するアプリケーションが、アプリケーションの接続文字列で構成されている `ApplicationIntent` プロパティに応じて、データベースの読み取り/書き込みレプリカまたは読み取り専用レプリカにリダイレクトされます。 `ApplicationIntent` プロパティの詳細については、「[アプリケーションの目的を指定する](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent)」を参照してください。 

読み取りスケールアウトが無効の場合またはサポートされていないサービス レベルで ReadScale プロパティを設定した場合、`ApplicationIntent` プロパティとは関係なく、すべての接続が読み取り/書き込みレプリカにリダイレクトされます。  

## <a name="conclusion"></a>まとめ
Azure SQL Database は、Azure プラットフォームと緊密に統合されており、障害の検出と復旧に Service Fabric を、データ保護に Azure Storage Blob を、フォールト トレランスを高めるために可用性ゾーンを活用します。 同時に、Azure SQL データベースは、レプリケーションとフェールオーバーのために、SQL Server 既成製品からの AlwaysOn 可用性グループ テクノロジをフル活用します。 これらのテクノロジを組み合わせることにより、アプリケーションは混合ストレージ モデルを最大限に活用して、高要件の SLA にも対応できます。 

## <a name="next-steps"></a>次の手順

- [Azure 可用性ゾーン](../availability-zones/az-overview.md)の詳細
- [Service Fabric](../service-fabric/service-fabric-overview.md) の詳細
- [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) の詳細 
