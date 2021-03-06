---
title: 回復性に優れた Azure 用アプリケーションの設計
description: 高可用性とディザスター リカバリーのために Azure で回復性に優れたアプリケーションを構築する方法。
author: MikeWasson
ms.date: 12/18/2018
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.custom: resiliency
ms.openlocfilehash: 7fd0e1bd42266b5e5718be4519352d99b58c0584
ms.sourcegitcommit: 644c2692a80e89648a80ea249fd17a3b17dc0818
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2019
ms.locfileid: "55987157"
---
# <a name="designing-resilient-applications-for-azure"></a>回復性に優れた Azure 用アプリケーションの設計

分散システムでは、障害が発生します。 ハードウェアの障害が発生することがあります。 ネットワークに一時的な障害が起きることがあります。 まれに、サービス全体またはリージョン全体で中断が発生する可能性がありますが、こうしたことについても計画しておく必要があります。

クラウドで信頼性の高いアプリケーションの構築は、エンタープライズ設定で信頼性の高いアプリケーションの構築とは異なります。 従来は、スケールアップのためにハイエンドなハードウェアを購入していましたが、クラウド環境では、スケールアップではなくスケールアウトする必要があります。 クラウド環境は、汎用的なハードウェアを使用することで低コストを維持できます。 目標は、障害がまったく発生しないように努力することではなく、システム内での障害の影響を最小限に抑えることです。

この記事では、Microsoft Azure で回復性に優れたアプリケーションを構築する方法の概要について説明します。 まず、*回復性*という用語の定義と関連する概念から説明します。 次に、設計、実装からデプロイ、運用までというアプリケーションの有効期間全体に体系的な方法を使用することで、回復性を実現するプロセスについて説明します。

<!-- markdownlint-disable MD026 -->

## <a name="what-is-resiliency"></a>回復性とは

<!-- markdownlint-enable MD026 -->

**回復性**とは、障害から回復して動作を続行する、システムの能力です。 障害を*回避*することではなく、ダウンタイムまたはデータの損失を回避するように障害に*対応*することです。 回復性の目的は、障害後にアプリケーションを完全に機能している状態に戻すことです。

回復性には、高可用性とディザスター リカバリーという 2 つの重要な側面があります。

- **高可用性** (HA) は、大幅なダウンタイムなしに、正常な状態で実行を継続するアプリケーションの能力です。 "正常な状態" とは、アプリケーションが応答し、ユーザーがアプリケーションに接続して対話できるという意味です。  
- **ディザスター リカバリー** (DR) は、まれに発生する大きなインシデント、すなわち地域全体に影響するサービス中断のように、一時的なものではない大規模な障害から回復する能力です。 ディザスター リカバリーには、データ バックアップやアーカイブの他に、バックアップからのデータベースの復元などの手動操作も含まれる場合があります。

HA と DR のとらえ方として、障害の影響が HA 設計で対応できる限度を超えたときに DR が始まると考えることができます。  

回復性を設計するときは、ご自分の可用性要件を理解する必要があります。 どの程度のダウンタイムを許容できるのでしょうか。 これは、ある程度はコストによって決まります。 起こり得るダウンタイムによって貴社のビジネスが被るコストはいくらになるでしょうか。 アプリケーションの可用性を高めることにいくら投資したいですか。 また、何をもってアプリケーションが使用可能であるとするかについても定義する必要があります。 たとえば、ユーザーが注文を送信しても、通常の時間枠内にシステムで処理できない場合、アプリケーションは "停止" 状態でしょうか。 また、特定の種類の障害が発生する可能性と、リスク軽減戦略のコスト効果が高いかどうかについても考慮します。

もう 1 つの一般的な用語に**ビジネス継続性** (BC) があります。BC とは、自然災害やサービスの停止などの悪条件の発生時と発生後に、重要なビジネス機能を実行できることです。 BC は、物理的な設備、人、コミュニティ、運輸、IT を含め、業務全体が対象です。 この記事ではクラウド アプリケーションを中心に扱いますが、回復性の計画は BC 要件全体のコンテキストで行う必要があります。

**データ バックアップ**は、DR の重要な部分です。 アプリケーションのステートレスなコンポーネントが失敗した場合、そのコンポーネントはいつでも再展開できます。 ただし、データが失われると、システムは安定した状態に戻ることはできません。 リージョン全体の障害に備えて、理想的には異なるリージョンにデータをバックアップする必要があります。

バックアップは、*データ レプリケーション*とは異なります。 データ レプリケーションでは、システムが迅速にレプリカにフェールオーバーできるように、ほぼリアルタイムでデータがコピーされます。 多くのデータベース システムがレプリケーションをサポートしています。たとえば、SQL Server では、SQL Server Always On 可用性グループがサポートされます。 データ レプリケーションでは、データのレプリカを確実かつ常に待機させることで、障害からの復旧時間を短縮できます。 しかし、データ レプリケーションで人的ミスを防ぐことはできません。 人的ミスによりデータが破損すると、その破損したデータはレプリカにコピーされます。 このため、DR 戦略には引き続き長期的なバックアップを含める必要があります。

## <a name="process-to-achieve-resiliency"></a>回復性を実現するプロセス

回復性はアドオンではありません。 システムの設計に追加し、運用方法に組み込む必要があります。 一般的なモデルは次のとおりです。

1. **定義**: ビジネスのニーズに基づいて可用性の要件を定義します。
2. **設計**: 回復性を考慮してアプリケーションを設計します。 実証済みの手法に従ったアーキテクチャから始めて、そのアーキテクチャで考えられる障害点を特定します。
3. **実装**: 障害を検出し、回復する戦略を実装します。
4. **テスト**: 障害をシミュレートし、強制的なフェールオーバーをトリガーして実装をテストします。
5. **デプロイ**: 信頼性が高い反復可能なプロセスを使用して、アプリケーションを運用環境にデプロイします。
6. **監視**: アプリケーションを監視して障害を検出します。 システムを監視することで、アプリケーションの正常性を評価し、必要に応じてインシデントに対応できます。
7. **対応**: 手動の介入が必要な障害が発生した場合に対応します。

以降、これらの各手順について詳しく説明します。

## <a name="define-your-availability-requirements"></a>可用性の要件を定義する

回復性の計画は、ビジネス要件から始まります。 ビジネス要件という点で回復性について考える場合の方法をいくつか挙げます。

### <a name="decompose-by-workload"></a>ワークロードごとに分解する

多くのクラウド ソリューションは、複数のアプリケーション ワークロードで構成されます。 このコンテキストで "ワークロード" という用語は、ビジネス ロジックとデータ ストレージ要件の点で、他のタスクとは論理的に区別できる個別の機能またはコンピューティング タスクを意味します。 たとえば、eコマース アプリには次のようなワークロードがあります。

- 製品カタログの閲覧と検索。
- 注文の作成と追跡。
- 推奨事項の表示。

これらのワークロードでは、可用性、スケーラビリティ、データの一貫性、およびディザスター リカバリーの要件が異なっている可能性があります。 コストとリスクのバランスを取るという観点から行う必要がある、ビジネス上の意思決定があります。

また、使用パターンについても考慮します。 システムが使用可能である必要がある特定の重要な期間はありますか。 たとえば、税金申告書サービスが申告期限の直前に停止してはならない、大規模なスポーツ イベント中にビデオ ストリーミング サービスは稼働している必要がある、などです。 重要な期間中は、複数のリージョンにまたがる冗長的なデプロイにして、1 つのリージョンで障害が発生してもアプリケーションがフェールオーバーできるようにすることができます。 ただし、複数リージョンのデプロイはコストが高くなる可能性があるため、重要な期間以外は、1 つのリージョンでアプリケーションを実行することができます。 場合によっては、課金対象となる使用量に達していないコンピューティング リソースには課金しない従量課金制を採用している最新のサーバーレス手法を利用することで、追加費用を軽減できます。

### <a name="rto-and-rpo"></a>RTO と RPO

考慮すべき 2 つの重要なメトリックとして、ディザスター リカバリーに関連している目標復旧時間と目標復旧時点があります。

- **目標復旧時間** (RTO) は、インシデントが発生した後に、アプリケーションに許容する使用不能状態の最大時間です。 RTO が 90 分の場合、災害発生から 90 分以内にアプリケーションを実行状態に復元できる必要があります。 RTO を非常に低い値にする場合は、リージョン全体の停止を防ぐために、アクティブ/パッシブ構成の中でスタンバイ状態で継続的に実行される第 2 のリージョンのデプロイを維持する必要があります。 場合によっては、アクティブ/アクティブ構成のデプロイでも、低めの RTO を実現できます。

- **回復ポイントの目標** (RPO) は、災害発生時に許容できるデータ損失の最大期間です。 たとえば、単一のデータベースにデータを格納し、他のデータベースへのレプリケーションなしで、毎時のバックアップを実行する場合、最大 1 時間のデータを失う可能性があります。

RTO と RPO はシステムの機能要件ではありません。これらはビジネス要件によって決定する必要があります。 これらの値を得るためにリスク評価を実施し、ダウンタイムまたはデータ損失のコストを明確に理解しておくことをお勧めします。

### <a name="mttr-and-mtbf"></a>MTTR と MTBF

可用性を測定するためのその他の一般的な 2 つのメジャーとして、平均復旧時間 (MTTR) と平均故障間隔 (MTBF) があります。 通常、これらのメジャーは、サービス プロバイダーがクラウド サービスに冗長性を追加する場所や顧客に提供する SLA を決定するために内部的に使用します。

**平均復旧時間** (MTTR) とは、障害発生後にコンポーネントを復旧するためにかかる平均時間です。 MTTR は、コンポーネントに関する経験的事実です。 各コンポーネントの MTTR に基づいて、アプリケーション全体の MTTR を見積もることができます。 MTTR 値が低い複数のコンポーネントでアプリケーションを構築すると、総合的な MTTR が低いアプリケーション、つまり障害からすばやく回復するアプリケーションを実現できます。

**平均故障間隔**(MTBF) とは、次の障害が発生するまでに合理的に予期されるコンポーネントの稼働時間です。 このメトリックは、サービスが利用不可になる頻度を計算するために役立ちます。 信頼性の低いコンポーネントでは MTBF が低いため、そのコンポーネントに対する SLA の数値は低くなります。 ただし、MTBF の低さは、コンポーネントの複数のインスタンスをデプロイし、それらの間でフェールオーバーを実装することで軽減できます。

> [!NOTE]
> 高可用性セットアップ内のコンポーネントの MTTR 値のいずれかがシステムの RTO を超えている場合、システムの障害によって容認できないビジネス上の中断が発生します。 定義されている RTO 内でシステムを復旧させることができなくなります。

### <a name="slas"></a>SLA

Azure の[サービス レベル アグリーメント][sla] (SLA) では、稼働時間と接続性に関する Microsoft のコミットメントが示されています。 特定のサービスの SLA が 99.9% の場合は、そのサービスを 99.9% の確率で使用可能だと予想できることを意味します。

> [!NOTE]
> また、Azure SLA には、SLA が満たされない場合にサービス クレジットを取得するための規定と、各サービスの "可用性" の詳細な定義も含まれています。 SLA のこの側面は、強制ポリシーとして機能します。

お客様のソリューションの各ワークロードには、独自の目標 SLA を定義することをお勧めします。 SLA があると、アーキテクチャがビジネス要件を満たしているかどうかを評価できるようになります。 依存関係マッピングの操作を実行して、Active Directory または支払プロバイダーや電子メール メッセージ サービスのようなサード パーティのサービスなど、内部および外部の依存関係を特定します。 特に、単一障害点になったり、イベントの間にボトルネックを生じさせたりする可能性がある外部の依存関係には、注意を払ってください。 たとえば、ワークロードのアップタイム要件が 99.99% で、SLA が 99.9% のサービスに依存している場合、そのサービスをシステムの単一障害点にしてはなりません。 救済策として、サービスで障害が発生した場合に備えてフォールバック パスを用意するか、そのサービスの障害から復旧する他の措置を実行する方法があります。

次の表は、さまざまな SLA レベルで考えられる累積的なダウンタイムの一覧です。

| SLA | 週あたりのダウンタイム | 月あたりのダウンタイム | 年あたりのダウンタイム |
| --- | --- | --- | --- |
| 99% |1.68 時間 |7.2 時間 |3.65 日 |
| 99.9% |10.1 分 |43.2 分 |8.76 時間 |
| 99.95% |5 分 |21.6 分 |4.38 時間 |
| 99.99% |1.01 分 |4.32 分 |52.56 分 |
| 99.999% |6 秒 |25.9 秒 |5.26 分 |

当然ながら高可用性が望ましく、他の項目は同じくらいです。 ただし、99.99...% の 9 の桁数を増やすにつれて、そのレベルの可用性を実現するためのコストと複雑さは増大します。 99.99% のアップタイムは、月あたりの合計ダウンタイムの約 5 分に換算されます。 5 つの 9 (99.999%) を実現するために、複雑さとコストが増大する価値はありますか。 その答えはビジネス要件によって変わります。

SLA を定義する場合の他の考慮事項を次に示します。

- 4 つの 9 (99.99%) を実現するには、手動操作による障害からの復旧には依存できません。 アプリケーションには自己診断機能と自己復旧機能が必要です。
- 4 つの 9 を超えると、SLA を満たすことができるほど迅速に障害を検出することは困難です。
- SLA を測定する時間枠について考慮します。 この時間枠を短くするほど、許容度は厳格になります。 時間単位または日単位のアップタイムという観点で SLA を定義することはおそらく意味がありません。
- MTBF と MTTR の測定値を考慮します。 SLA が低ければ低いほど、サービスがダウンする可能性は低くなり、サービスをより短時間で回復させる必要があります。

### <a name="composite-slas"></a>複合 SLA

Azure SQL Database に書き込む App Service Web アプリについて検討します。 この記事の執筆時点で、このような Azure サービスには次の SLA があります。

- App Service Web Apps = 99.95%
- SQL Database = 99.99%

![複合 SLA](./images/sla1.png)

このアプリケーションで予想される最大ダウンタイムはどのくらいですか。 いずれかのサービスで障害が発生すると、アプリケーション全体で障害が発生します。 一般的に、各サービスで障害が発生する可能性は独立しているため、このアプリケーションの複合 SLA は 99.95% &times; 99.99% = 99.94% です。 これは個々の SLA よりも低い値です。複数のサービスに依存するアプリケーションは潜在的な障害点が多くなるため、これは驚くことではありません。

一方、複合 SLA を改善するには、独立したフォールバック パスを作成する方法があります。 たとえば、SQL Database が使用できない場合は、後で処理できるようにトランザクションをキューに格納します。

![複合 SLA](./images/sla2.png)

この設計の場合、データベースに接続できない場合でも、アプリケーションは使用可能な状態です。 ただし、データベースとキューの両方で同時に障害が発生すると、アプリケーションは使用できなくなります。 同時に障害が発生する時間の予測される割合は 0.0001 &times; 0.001 なので、この組み合わせのパスに関する複合 SLA は次のとおりです。  

- データベース OR キュー = 1.0 &minus; (0.0001 &times; 0.001) = 99.99999%

合計複合 SLA は次のとおりです。

- Web アプリ AND (データベース OR キュー) = 99.95% &times; 99.99999% = 99.95% 以下

ただし、この方法にはトレードオフがあります。 アプリケーション ロジックは複雑になり、キューのコストがかかり、データの一貫性に関する問題も考慮が必要になる可能性があります。

**複数リージョン デプロイの SLA**。 もう 1 つの HA 手法は、アプリケーションを複数のリージョンにデプロイし、1 つのリージョンのアプリケーションで障害が発生した場合にフェールオーバーするように Azure Traffic Manager を使用する方法です。 マルチリージョンのデプロイの場合、複合 SLA は次のように計算されます。

1 つのリージョンにデプロイされるアプリケーションの複合 SLA を *N* とし、アプリケーションがデプロイされるリージョンの数を *R* とします。 すべてのリージョンでアプリケーションの障害が発生する可能性は ((1 &minus; N) ^ R) と予測されます。

たとえば、1 つのリージョンの SLA が 99.95% の場合は次のようになります。

- 2 つのリージョンの複合 SLA = (1 &minus; (0.9995 ^ 2)) = 99.999975%
- 4 つのリージョンの複合 SLA = (1 &minus; (0.9995 ^ 4)) = 99.999999%

[Traffic Manager の SLA][tm-sla] を考慮する必要があります。 この記事の執筆時点で Traffic Manager の SLA は 99.99% です。

また、アクティブ/パッシブ構成ではフェールオーバーは瞬時に完了しないので、フェールオーバー時にはある程度のダウンタイムが発生する可能性があります。 [Traffic Manager エンドポイントの監視とフェールオーバー][tm-failover]に関するページを参照してください。

計算された SLA 値は便利なベースラインですが、可用性のすべてを把握できる訳ではありません。 重要ではないパスで障害が発生した場合、アプリケーションの機能を適切に低下できることがよくあります。 書籍のカタログを表示するアプリケーションを例に説明します。 そのアプリケーションが表紙のサムネイル画像を取得できない場合は、プレース ホルダー イメージを表示することができます。 この場合、画像を取得できないと、アプリケーションの稼働時間は減りませんが、ユーザー エクスペリエンスには影響が出ます。  

## <a name="design-for-resiliency"></a>回復性の設計

設計フェーズで、障害モードの分析 (FMA) を実行することをお勧めします。 FMA の目標は、潜在的な障害点を特定し、アプリケーションがその障害に対して対応する方法を定義することです。

- アプリケーションはこのような障害をどのように検出するでしょうか。
- アプリケーションはこのような障害にどのように対応するでしょうか。
- このような障害のログ記録と監視はどのように行うでしょうか。

FMA プロセスの詳細と、Azure 固有の推奨事項については、[Azure 回復性ガイダンスの「障害モードの分析][fma]」を参照してください。

### <a name="example-of-identifying-failure-modes-and-detection-strategy"></a>障害モードと検出戦略の特定例

**障害点:** 外部 Web サービス/API の呼び出し。

| 障害モード | 検出戦略 |
| --- | --- |
| サービスを使用できない |HTTP 5xx |
| Throttling |HTTP 429 (要求が多すぎます) |
| Authentication |HTTP 401 (権限がありません) |
| 遅い応答 |要求のタイムアウト |

### <a name="redundancy-and-designing-for-failure"></a>障害のための冗長性と設計

障害が及ぼす影響の範囲はさまざまです。 たとえば、ディスク障害などの一部のハードウェア障害が、1 台のホスト コンピューターに影響を及ぼすことがあります。 障害が発生したネットワーク スイッチが、サーバー ラック全体に影響する場合もあります。 データセンターの停電など、データセンター全体を中断させる障害はまれです。 ほとんど発生しませんが、リージョン全体が利用できなくなることもあります。

アプリケーションの回復性を実現するための 1 つの手段として、冗長性があります。 ただし、この冗長性は、アプリケーションの設計時に計画しなければなりません。 また、必要な冗長性のレベルはビジネス要件によって異なります。リージョン障害に備えるために、すべてのアプリケーションにリージョン間での冗長性が必要だとは限りません。 一般的に、冗長性と信頼性を高めると、コストと複雑さが増すというトレードオフがあります。  

Azure には、個別の VM からリージョン全体まで、あらゆる障害レベルでアプリケーションの冗長性を確保するための機能が複数用意されています。

![Azure の回復性の機能](./images/redundancy.svg)

**単一の VM**。 Azure では、単一の VM に対しては[アップタイム SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) が用意されています  (VM では、すべてのオペレーティング システム ディスクとデータ ディスク用に Premium Storage が使用されている必要があります)。2 つ以上の VM を実行すると高い SLA を得られますが、ワークロードによっては単一の VM で十分な信頼性が得られる場合もあります。 ただし、運用環境のワークロードの場合は、2 台以上の VM を使用して冗長性を確保することをお勧めします。

**可用性セット**。 ディスクやネットワーク スイッチの障害など、ローカライズされたハードウェアの障害から保護するには、可用性セットに 2 つ以上の VM をデプロイします。 可用性セットは、電源とネットワーク スイッチを共有する 2 つ以上の "*障害ドメイン*" で構成されます。 可用性セットの VM は複数の障害ドメインに分散されるため、ある障害ドメインがハードウェア障害の影響を受けた場合は、ネットワーク トラフィックを他の障害ドメインの VM にルーティングできます。 可用性セットの詳細については、「[Azure での Windows 仮想マシンの可用性の管理](/azure/virtual-machines/windows/manage-availability)」を参照してください。

**可用性ゾーン**。  可用性ゾーンとは、Azure リージョンの物理的に独立したゾーンのことです。 可用性ゾーンはそれぞれ異なる電源、ネットワーク、および冷却装置を持ちます。 可用性ゾーンに VM をデプロイすると、データセンター全体の障害からアプリケーションを保護するのに役立ちます。 Availability Zones は、すべてのリージョンでサポートされているわけではありません。 サポートされているリージョンとサービスの一覧については、「[Azure の可用性ゾーンの概要](/azure/availability-zones/az-overview)」を参照してください。

デプロイの中で Availability Zones を使用する予定がある場合は、アプリケーションのアーキテクチャとコード ベースでこの構成をサポートできることを最初に確認してください。 市販のソフトウェアをデプロイする場合は、ソフトウェア ベンダーに問い合わせて、運用環境にデプロイする前に十分にテストしてください。 アプリケーションでは、状態の管理と、構成されたゾーン内で障害が発生した場合のデータの消失防止が可能である必要があります。 コード ベース内でハード コーディングされたインフラストラクチャ コンポーネントが指定されていない、エラスティックな分散されたインフラストラクチャ上での実行がサポートされる必要があります。

**Azure Site Recovery**。  ビジネス継続性とディザスター リカバリーのニーズに応じて、Azure 仮想マシンを別の Azure リージョンにレプリケートします。 コンプライアンスのニーズを満たしていることを確認する定期的な DR ドリルを実施できます。 ソース リージョンで障害が発生した場合、アプリケーションを回復できるように、選択したリージョンに指定した設定で VM がレプリケートされます。 詳細については、[ASR を使用した Azure VM のレプリケート][site-recovery]に関する記事を参照してください。 ここで、お使いのソリューションの RTO と RPO の数値を考慮し、テストの実行時に復旧時間と復旧ポイントがご自身のニーズに適していることを確認します。

**ペアになっているリージョン**。 リージョンの障害からアプリケーションを保護するために、複数のリージョンにアプリケーションをデプロイし、Azure Traffic Manager を使用してインターネット トラフィックを異なるリージョンに分散できます。 各 Azure リージョンは別のリージョンとペアになります。 これが[リージョン ペア](/azure/best-practices-availability-paired-regions)になります。 ブラジル南部を除き、リージョン ペアは、税および法の執行を目的としたデータ常駐要件を満たすために同じ地理的場所に配置されます。

複数リージョンのアプリケーションを設計する場合、リージョン間のネットワーク待機時間は、リージョン内の待機時間よりも長くなることを考慮してください。 たとえば、データベースをレプリケートしてフェールオーバーを有効にするときは、リージョン間の非同期データ レプリケーションではなく、リージョン内で同期データ レプリケーションを使用します。

ペアになったリージョンを選ぶ場合は、両方のリージョンで Azure サービスが要件になっていることを確認してください。 リージョンごとのサービスの一覧については、「[リージョン別の利用可能な製品](https://azure.microsoft.com/global-infrastructure/services/)」をご覧ください。 RPO/RTO が短い場合には特に、ディザスター リカバリーのために適切なデプロイ トポロジを選択することも重要です。 フェールオーバー リージョンにおいて、お使いのワークロードをサポートするだけの十分な容量が確実に保持できるように、アクティブ/パッシブ (完全なレプリカ) トポロジまたはアクティブ/アクティブ トポロジのどちらかを選択します。 これらのデプロイ トポロジは、セカンダリ リージョン内のリソースが事前にプロビジョニングされることから複雑さとコストが増す場合があり、アイドル状態になる可能性があることを念頭に置いてください。 詳しくは、[ディザスター リカバリーのデプロイ トポロジ][deployment-topologies]に関するページをご覧ください。

| &nbsp; | 可用性セット | 可用性ゾーン | Azure Site Recovery/ペアのリージョン |
|--------|------------------|-------------------|---------------|
| 障害の範囲 | ラック | データセンター | リージョン |
| 要求のルーティング | Load Balancer | クロスゾーン ロード バランサー | Traffic Manager |
| ネットワーク待ち時間 | 非常に低い | 低 | 中～高 |
| 仮想ネットワーク  | VNet | VNet | リージョン間 VNet ピアリング |

## <a name="implement-resiliency-strategies"></a>回復性戦略を実装する

ここでは、一般的な回復性戦略の調査について説明します。 ほとんどの項目は特定のテクノロジに限定されていません。 このセクションの説明では、各手法の背後にある一般的な考えをまとめ、詳細なドキュメントのリンクを紹介しています。

**一時的な障害を再試行する**。 一時的な障害は、ネットワーク接続の一時的な喪失、データベース接続の欠落、またはサービスがビジー状態のときのタイムアウトが原因で発生します。 多くの場合、一時的な障害は、要求を再試行するだけで解決できます。 多くの Azure サービスでは、クライアント SDK が呼び出し元には認識されない方法の自動再試行を実装しています。詳細については、[サービス固有の再試行ガイダンス][retry-service-specific guidance]に関するページを参照してください。

再試行ごとに、合計待機時間が追加されます。 また、失敗した要求が多すぎると、保留中の要求がキューに蓄積されるため、ボトルネックの原因になる可能性があります。 これらのブロックされた要求が重要なシステム リソース (メモリ、しきい値、データベース接続など) を保持することで、障害が連鎖する可能性があります。 この問題を回避するために、再試行間隔を長くして、失敗した要求の合計数を制限します。

![再試行の図](./images/retry.png)

**インスタンス間で負荷分散する**。 スケーラビリティのために、インスタンス数を増やしてクラウド アプリケーションをスケールアウトすることをお勧めします。 この方法では、正常ではないインスタンスをローテーションから除去できるため、回復性も改善されます。 例: 

- 複数の VM をロード バランサーの内側に配置します。 ロード バランサーはトラフィックをすべての VM に分散します。 「[Run load-balanced VMs for scalability and availability][ra-multi-vm]」(スケーラビリティと可用性のために負荷分散された VM を実行する) を参照してください。
- Azure App Service アプリを複数インスタンスにスケールアウトします。 App Service は自動的にインスタンス全体に負荷を分散します。 「[Basic web application][ra-basic-web]」(基本的な Web アプリケーション) を参照してください。
- [Azure Traffic Manager][tm] を使用して、一連のエンドポイント全体でトラフィックを分散します。

**データをレプリケートする**。 データのレプリケートは、データ ストアの一時的ではない障害を処理する場合の一般的な戦略です。 Azure Storage、Azure SQL Database、Cosmos DB、Apache Cassandra など、多くのストレージ テクノロジにはレプリケーション機能が組み込まれています。 読み取りパスと書き込みパスの両方を考慮することが重要です。 戦略テクノロジに応じて、複数の書き込み可能なレプリカ、または単一の書き込み可能なレプリカと複数の読み取り専用レプリカを用意する必要があります。

可用性を最大限にするために、レプリカを複数のリージョンに配置する方法もあります。 ただし、データをレプリケートすると待機時間が長くなります。 通常、複数リージョンをまたがるレプリケートは非同期に行われます。これは最終的な整合性モデルであり、レプリカで障害が発生した場合にはデータ損失が発生する可能性があります。

[Azure Site Recovery][site-recovery] を使用して、異なる Azure リージョン間で Azure 仮想マシンをレプリケートします。 Site Recovery では、ターゲット リージョンに、データが継続的にレプリケートされます。 プライマリ サイトで障害が発生した場合は、セカンダリ ロケーションにフェールオーバーします。

**潔く機能を減らす**。 サービスで障害が発生し、フェールオーバー パスがない場合、許容範囲のユーザー エクスペリエンスを提供した状態で、アプリケーションの機能を適切に低下させることができます。 例: 

- 作業項目を後で処理できるようにキューに格納します。
- 予測値を返します。
- ローカルにキャッシュされたデータを使用します。
- ユーザーにエラー メッセージを表示します (この選択肢は、アプリケーションが要求への応答を停止するよりも優れています)。

**高負荷のユーザーを調整する**。 少数のユーザーが過剰な負荷を発生させることがあります。 その結果、他のユーザーに影響が及び、アプリケーションの全体的な可用性が低くなる可能性があります。

単一のクライアントが大量の要求を行った場合、一定期間、アプリケーションはクライアントを調整することができます。 この調整期間、(詳細な調整戦略に基づいて) アプリケーションはそのクライアントからの要求の一部またはすべてを拒否します。 調整のしきい値は、ユーザーのサービス レベルによって変わる可能性があります。

調整は、クライアントが悪意を持って実行している場合に限定されません。サービス クォータを超えたというだけの理由で行われます。 場合によっては、コンシューマーが常にクォータを超えているか、動作が正しくない可能性があります。 この場合は、さらにユーザーをブロックすることがあります。 通常、このブロックを行うには、API キーまたは IP アドレス範囲をブロックします。 詳細については、「[Throttling pattern][throttling-pattern]」(調整パターン) を参照してください。

**サーキット ブレーカーを使用する**。 [サーキット ブレーカー][circuit-breaker-pattern] パターンを使用すると、失敗する可能性のある操作がアプリケーションで繰り返し再試行されるのを回避できます。 サーキット ブレーカーはサービスの呼び出しをラップし、最近発生したエラーの数を追跡します。 エラーの数がしきい値を超えると、サーキット ブレーカーはサービスを呼び出さずに、エラー コードを返し始めます。 これにより、サービスが復旧するための時間が確保されます。

**負荷平準化を使用してトラフィックの急増をなくす**。
アプリケーションではトラフィックの急増が起きることがあり、これがバックエンドのサービスに大きな負荷をかけることがあります。 バックエンド サービスが要求にすばやく応答できない場合、要求がキューに格納されることや (バックアップ)、サービスによってアプリケーションが調整されることがあります。 この状況を回避するために、バッファーとしてキューを使用することができます。 新しい作業項目がある場合、バックエンド サービスを即時に呼び出すのではなく、アプリケーションで作業項目をキューに格納して非同期に実行します。 キューは、負荷のピークを平準化するバッファーとして機能します。 詳細については、[Queue-Based Load Leveling パターン][load-leveling-pattern]に関するページを参照してください。

**重要なリソースを分離する**。 1 つのサブシステムで複数の障害が連鎖し、アプリケーションの他の部分で障害が発生する可能性があります。 これが起きるのは、障害によってスレッドやソケットなどのリソースが適切なタイミングで解放されない場合で、リソースの消費につながります。

これを回避するには、システムを分離グループにパーティション分割して、1 つのパーティションでの障害がシステム全体をダウンさせないようにします。 この手法は、[バルクヘッド パターン][bulkhead-pattern]と呼ばれることもあります。

次に例を示します。

- データベースをパーティション分割し (テナント別など)、各パーティションに Web サーバー インスタンスの別プールを割り当てます。  
- 個別のスレッド プールを使用して、異なるサービスの呼び出しを分離します。 こうすることで、いずれかのサービスで障害が発生した場合の障害の連鎖を回避できます。 例については、Netflix の [Hystrix ライブラリ][hystrix]を参照してください。
- [コンテナー][containers]を使用して、特定のサブシステムに使用できるリソースを制限します。

![バルクヘッド パターンの図](./images/bulkhead.png)

**補正トランザクションを適用する**。 [補正トランザクション][compensating-transaction-pattern]は、別の完了したトランザクションの影響を元に戻すトランザクションです。 分散システムでは、厳密なトランザクションの一貫性を実現することが非常に困難な可能性があります。 補正トランザクションは、各手順で元に戻すことができる一連の小規模な個別トランザクションを使用することで、一貫性を実現する方法です。

たとえば、旅行を予約する場合、ユーザーは車、ホテルの客室、航空便を予約する可能性があります。 これらいずれかの手順に失敗した場合、操作全体が失敗します。 操作全体に単一の分散トランザクションを使用するのではなく、各手順に補正トランザクションを定義することができます。 たとえば、車の予約を元に戻すには、予約を取り消します。 操作全体を完了するために、コーディネーターは各手順を実行します。 いずれかの手順が失敗すると、コーディネーターは補正トランザクションを適用し、完了したすべての手順を元に戻します。

## <a name="test-for-resiliency"></a>回復性のテスト

一般的に、アプリケーション機能のテストと同じ方法 (単体テストの実行など) で回復性をテストすることはできません。 代わりに、断続的にのみ発生する障害条件下で、エンドツーエンドのワークロードが動作する方法をテストする必要があります。

テストは反復的なプロセスです。 アプリケーションをテストし、結果を測定し、結果のすべてのエラーを分析して対応し、プロセスを繰り返します。

**フォールト挿入テスト**。 実際のエラーをトリガーするまたはシミュレートすることによって、システムの障害への回復性をテストすることができます。 テストされる一般的な障害シナリオを次に示します。

- VM インスタンスのシャットダウン。
- プロセスのクラッシュ。
- 証明書の期限切れ。
- アクセス キーの変更。
- ドメイン コントローラー上の DNS サービスのシャットダウン。
- RAM やスレッド数など、使用可能なシステム リソースの制限。
- ディスクのマウント解除。
- VM の再デプロイ。

回復時間を測定し、ビジネス要件を満たしていることを確認します。 障害モードの組み合わせもテストします。 障害が連鎖しないこと、分離された方法で処理されることを確認します。

これが、設計フェーズで潜在的な障害点を分析することが重要なもう 1 つの理由です。 この分析結果をテスト計画に反映させることをお勧めします。

**ロード テスト**。 ロード テストは、過負荷状態またはサービスの調整が行われているバックエンド データベースなど、負荷がかかった状態でのみ発生する障害を特定するために重要です。 運用データまたは運用データに可能な限り近い総合的なデータを使用して、ピーク負荷の場合をテストします。 目標は、実際の条件下でアプリケーションがどのように動作するかを確認することです。

**ディザスター リカバリーの訓練**。 優れたディザスター リカバリー プランを策定しただけでは不十分です。 問題が発生したときに復旧計画が適切に機能することを確認するために、定期的にテストを行う必要があります。 Azure 仮想マシンについては、[Azure Site Recovery][site-recovery] を使用することで、運用アプリケーションや進行中のレプリケーションに影響を与えることなく、レプリケーションおよび [DR 訓練を実施][site-recovery-test-failover]できます。

## <a name="deploy-using-reliable-processes"></a>信頼性の高いプロセスを使用してデプロイする

アプリケーションを運用環境にデプロイした場合、更新プログラムがエラーの原因になることがあります。 最悪の場合には、更新プログラムの不良でダウンタイムが発生することがあります。 この問題を回避するために、デプロイ プロセスを予測可能で反復的にする必要があります。 デプロイには、Azure リソースのプロビジョニング、アプリケーション コードのデプロイ、構成設定の適用が含まれます。 1 つの更新プログラムで、この 3 つすべて、または一部が行われることがあります。

手動のデプロイはエラーが起きやすいという点が重要です。 そのため、必要に応じて実行可能で、何かが失敗した場合に再実行できる自動的なべき等プロセスを用意することをお勧めします。

* Azure リソースのプロビジョニングを自動化するために、[Terraform][terraform]、[Ansible][ansible]、[Chef][chef]、[Puppet][puppet]、[PowerShell][powershell]、[CLI][cli]、または [Azure Resource Manager テンプレート][template-deployment]を使用できます。
* [Azure Automation Desired State Configuration][dsc] (DSC) を使用して VM を構成します。 Linux VM では、[Cloud-init][cloud-init] を使用できます。
* [Azure DevOps Services][azure-devops-services] または [Jenkins][jenkins] を使用して、アプリケーションのデプロイを自動化できます。

*コードとしてのインフラストラクチャ*と*イミュータブル インフラストラクチャ*という回復性のデプロイに関連する 2 つの概念があります。

- **コードとしてのインフラストラクチャ**は、コードを使用してインフラストラクチャのプロビジョニングと構成を行う手法です。 コードとしてのインフラストラクチャには、宣言型の方法、命令型の方法 (またはその両方の組み合わせ) を使用することができます。 宣言型の方法の例として、Resource Manager テンプレートがあります。 PowerShell スクリプトは、命令型の方法の一例です。
- **イミュータブル インフラストラクチャ**は、運用環境にデプロイした後にインフラストラクチャを変更すべきではないという原則です。 そうしないと、場当たりの変更が適用され、変更内容を正確に把握しづらくなり、システムについて論理的に判断しづらくなる状態に陥る可能性があります。

もう 1 つの問題は、アプリケーションの更新プログラムを展開する方法です。 ブルーグリーン デプロイまたはカナリア リリースなどの手法を採用して高度に制御された方法で更新プログラムをプッシュし、不適切なデプロイの考えられる影響を最小限に抑えることをお勧めします。

- [ブルーグリーン デプロイ][blue-green]は、ライブ アプリケーションとは別の運用環境に更新プログラムをデプロイする手法です。 デプロイの検証が完了したら、トラフィック ルーティングを更新されたバージョンに切り替えます。 たとえば、Azure App Service Web Apps では、[ステージング スロット][staging-slots]を利用してこれを実現できます。
- [カナリア リリース][canary-release]は、ブルーグリーン デプロイと似ています。 すべてのトラフィックを更新されたバージョンに切り替えるのではなく、トラフィックの一部を新しいデプロイにルーティングすることで更新プログラムを少数のユーザーに展開します。 問題が発生した場合は、以前のデプロイに戻します。 問題が発生しない場合は、より多くのトラフィックを新しいバージョンにルーティングします。トラフィックの 100% になるまでこの手順を実行します。

どの方法を採用する場合でも、新しいバージョンが機能しなかった場合に最後の正常なデプロイに戻すことができるようにします。 また、データベースの変更や依存サービスに対するその他の変更をロールバックするための戦略を適切に配置します。 エラーが発生した場合は、アプリケーション ログによって必ずエラーの原因になったバージョンを特定できるようにします。

## <a name="monitor-to-detect-failures"></a>監視によって障害を検出する
回復性のためには、監視が非常に重要です。 何かが失敗した場合、失敗したことを把握し、障害の原因を分析する必要があります。 

大規模な分散システムの監視は、大きな課題です。 数十単位の VM で実行されるアプリケーションを例にして説明します。&mdash; 各 VM 1 つずつにログを記録し、ログ ファイルを確認し、問題を解決することは実用的ではありません。 さらに、VM インスタンスの数はおそらく静的ではありません。アプリケーションのスケールインとスケールアウトによって VM 数は増減し、インスタンスが失敗して再プロビジョニングが必要になることがあります。 さらに、一般的なクラウド アプリケーションは複数のデータ ストア (App Storage、SQL Database、Cosmos DB、Redis Cache) を使用することや、単一のユーザー アクションが複数のサブシステムに広がることがあります。 

監視プロセスは、いくつかの異なる段階があるパイプラインと捉えることができます。

![複合 SLA](./images/monitoring.png)

* **インストルメンテーション**。 監視対象となる生データは、[アプリケーション ログ](/azure/application-insights/app-insights-overview?toc=/azure/azure-monitor/toc.json)、[オペレーティング システムのパフォーマンス メトリック](/azure/azure-monitor/platform/agents-overview)、[Azure リソース](/azure/monitoring-and-diagnostics/monitoring-supported-metrics?toc=/azure/azure-monitor/toc.json)、[Azure サブスクリプション](/azure/service-health/service-health-overview)、および [Azure テナント](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics) を含む、さまざまなソースに由来します。 ほとんどの Azure サービスでは、問題の原因を分析して特定するために構成可能な[メトリック](/azure/azure-monitor/platform/data-collection)を公開しています。
* **収集と保存**。 生インストルメンテーション データは多様な場所に多様な形式で保持される可能性があります (たとえば、アプリケーション トレース ログ、IIS ログ、パフォーマンス カウンターなど)。 これらのさまざまなソースは収集され、統合されて、Application Insights、Azure Monitor メトリック、Service Health、ストレージ アカウント、および Log Analytics などの信頼できるデータ ストアに配置されます。
* **分析と診断**。 データは、これらの異なるデータ ストア内で統合された後、問題を解決するために分析され、アプリケーションの正常性に関する全体的な見解を提示できます。 一般的には、[Kusto クエリ](/azure/log-analytics/log-analytics-queries)を使用して、Application Insights および Log Analytics 内のデータを検索できます。 Azure Advisor では、[回復性](/azure/advisor/advisor-high-availability-recommendations)および[最適化](/azure/advisor/advisor-performance-recommendations)に重点を置いて推奨事項を提示します。 
* **視覚化とアラート**。 この段階では、オペレーターが問題や傾向にすばやく気づくことができる方法で利用統計情報が表示されます。 たとえば、ダッシュボードや電子メールのアラートがあります。 [Azure ダッシュボード](/azure/azure-portal/azure-portal-dashboards)を使用すると、Application Insights、Log Analytics、Azure Monitor メトリックおよびサービスの正常性を元にして、単一ウィンドウでの監視グラフのグラス ビューを構築できます。 [Azure Monitor アラート](/azure/monitoring-and-diagnostics/monitoring-overview-alerts?toc=/azure/azure-monitor/toc.json)を使用すると、サービスの正常性およびリソースの正常性に関するアラートを作成できます。

監視は障害の検出と同じではありません。 たとえば、アプリケーションが一時的なエラーを検出して再試行し、ダウンタイムが発生しないことがあります。 それでも再試行操作はログに記録されるので、エラー率を監視して、アプリケーション正常性の全体像を把握することができます。

アプリケーション ログは、診断データの重要なソースです。 アプリケーションのログ記録には、次のようなベスト プラクティスがあります。

- 運用環境でログを記録します。 そうしないと、最も必要な場合に洞察できません。
- サービスの境界でイベントのログを記録します。 フローがサービス境界をまたがる関連付け ID を含めます。 トランザクション フローが複数のサービスを経由し、いずれかが失敗した場合、関連付け ID でトランザクションが失敗した理由を特定できます。
- セマンティック ログ (構造化ログとも呼ばれます) を使用します。 構造化されていないログの場合、ログ データの使用と分析の自動化が困難です。こうした自動化はクラウド規模で必要になります。
- 非同期のログ記録を使用します。 そうしないと、ログ アプリケーションによって、ログ記録イベントの書き込みを待機中に要求がブロックされ、要求がバックアップされ、アプリケーションで障害が発生する原因になる可能性があります。
- アプリケーションのログ記録は、監査と同じではありません。 監査は、コンプライアンスまたは法規制上の理由で行われることがあります。 そのため、監査レコードは完全である必要があります。トランザクションの処理中に削除が発生することは許容されません。 アプリケーションに監査が必要な場合、診断ログとは別に維持する必要があります。

監視と診断の詳細については、「[Monitoring and diagnostics guidance][monitoring-guidance]」(監査と診断のガイダンス) を参照してください。

## <a name="respond-to-failures"></a>障害に対応する
これまでのセクションでは、高可用性に重要な自動的な復旧戦略を中心に説明してきましたが、 手動操作が必要な場合もあります。

* **[Alerts]** (アラート)。 アプリケーションで、積極的な介入が必要な可能性がある警告の兆候を監視します。 たとえば、SQL Database または Cosmos DB がアプリケーションを常に調整している場合は、データベース容量の増加や、クエリの最適化が必要な可能性があります。 この例では、アプリケーションが調整エラーを透過的に処理している場合でも、利用統計情報でアラートが報告されるので、対応することができます。 サービス制限とクォータのしきい値に対して、Azure リソース メトリックと診断ログにアラートを構成することが推奨されています。 診断ログに比べてメトリックは低待機時間になるため、メトリックにアラートを設定することをお勧めします。 さらに、Azure では、[リソースの正常性](https://docs.microsoft.com/en-us/azure/service-health/resource-health-checks-resource-types)を使用して、Azure サービスの調整を診断できるすぐに使える正常性ステータスを提供できます。    
* **フェールオーバーする**。 アプリケーションのディザスター リカバリー戦略を構成します。 適切な戦略は、お使いの SLA に応じて変わります。 詳しいシナリオについては、アクティブ/パッシブ実装で十分に確認できます。 詳しくは、[ディザスター リカバリーのデプロイ トポロジ](./disaster-recovery-azure-applications.md#deployment-topologies-for-disaster-recovery)に関するページをご覧ください。 ほとんどの Azure サービスでは、手動または自動フェールオーバーのどちらかに対応しています。 たとえば、IaaS アプリケーションでは、Web 層および論理層に [Azure Site Recovery](/azure/site-recovery/azure-to-azure-architecture) を、データベース層に [SQL AlwaysOn 可用性グループ](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr)を使用します。 [Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview) では、リージョン間での自動フェールオーバーを提供しています。
* **運用準備状況のテスト**。 セカンダリ リージョンへのフェールオーバーと、プライマリ リージョンへのフェールバックの両方について、運用準備状況のテストを実行します。 多くの Azure サービスでは、ディザスター リカバリー訓練のために手動フェールオーバーまたはテスト フェールオーバーをサポートしています。 また、サービスをシャットダウンまたは削除することで、停止をシミュレートできます。
* **データ整合性チェック**。 データ ストアで障害が発生した場合、特にデータがレプリケートされた場合には、ストアを再び使用可能な状態にするときにデータに整合性がある必要があります。 リージョン間でのレプリケーションを提供する Azure サービスの場合、障害で想定されるデータ損失を把握するために RTO と RPO を確認します。 リージョン間でのフェールオーバーが手動で開始できるか、または Microsoft によって開始されるかを把握するには、Azure サービスの SLA を確認します。 一部のサービスでは、フェールオーバーを実行するタイミングを Microsoft が判断します。 Microsoft では、プライマリ リージョンにあるデータの復旧を優先し、プライマリ リージョンにあるデータが復旧不能であると判断した場合にのみ、セカンダリ リージョンにフェールオーバーする場合があります。 たとえば、[Geo 冗長ストレージ](/azure/storage/common/storage-redundancy-grs)や [Key Vault](/azure/key-vault/key-vault-disaster-recovery-guidance) では、このモデルに従います。
* **バックアップからの復元**。 一部のシナリオでは、バックアップからの復元は、同じリージョン内のみで可能です。 [Azure VMs Backup](/azure/backup/backup-azure-vms-first-look-arm) は、これに該当します。 他のAzure サービスでは、[Redis Cache の geo レプリカ](/azure/redis-cache/cache-how-to-geo-replication)など、geo レプリケートされたバックアップを提供しています。 バックアップの目的は、誤削除やデータの破損から保護し、アプリケーションを前の時点の動作可能なバージョンに復元することです。 そのため、バックアップは、場合によってはディザスター リカバリー ソリューションとしての機能できますが、逆は必ずしも当てはまりません。ディザスター リカバリーでは、誤削除やデータの破損から保護されません。  

ディザスター リカバリー計画を文書化し、テストします。 アプリケーションの障害によるビジネスの影響を評価します。 できるだけ多くのプロセスを自動化し、手動の手順を文書化します。たとえば、バックアップからの手動のフェールオーバーやデータの復元などです。 ディザスター リカバリー プロセスは定期的にテストし、計画の検証と改善を行います。 アプリケーションによって使用される Azure サービスのアラートを設定します。

## <a name="summary"></a>まとめ
この記事では、クラウド固有の一部の課題を特に取り上げながら、包括的な観点から回復性について説明しました。 たとえば、クラウド コンピューティングの分散の特性、汎用的なハードウェアの使用、一時的なネットワーク障害の存在などです。

この記事の主な点を次に示します。

- 回復性によって可用性が高くなり、障害からの平均復旧時間が短くなります。
- クラウドで回復性を実現するには、従来のオンプレミス ソリューションのさまざまな手法が必要です。
- 偶発的に回復性が実現することはありません。 初期段階から設計し、構築する必要があります。
- 回復性は、計画、コーディングから運用まで、アプリケーションのライフサイクルのあらゆる部分に関係します。
- テストと監視をお勧めします。

<!-- links -->

[blue-green]: https://martinfowler.com/bliki/BlueGreenDeployment.html
[canary-release]: https://martinfowler.com/bliki/CanaryRelease.html
[circuit-breaker-pattern]: ../patterns/circuit-breaker.md
[compensating-transaction-pattern]: ../patterns/compensating-transaction.md
[containers]: https://en.wikipedia.org/wiki/Operating-system-level_virtualization
[dsc]: /azure/automation/automation-dsc-overview
[contingency-planning-guide]: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-34r1.pdf
[fma]: ./failure-mode-analysis.md
[hystrix]: https://medium.com/netflix-techblog/introducing-hystrix-for-resilience-engineering-13531c1ab362
[jmeter]: https://jmeter.apache.org/
[load-leveling-pattern]: ../patterns/queue-based-load-leveling.md
[monitoring-guidance]: ../best-practices/monitoring.md
[ra-basic-web]: ../reference-architectures/app-service-web-app/basic-web-app.md
[ra-multi-vm]: ../reference-architectures/virtual-machines-windows/multi-vm.md
[checklist]: ../checklist/resiliency.md
[retry-pattern]: ../patterns/retry.md
[retry-service-specific guidance]: ../best-practices/retry-service-specific.md
[sla]: https://azure.microsoft.com/support/legal/sla/
[throttling-pattern]: ../patterns/throttling.md
[tm]: https://azure.microsoft.com/services/traffic-manager/
[tm-failover]: /azure/traffic-manager/traffic-manager-monitoring
[tm-sla]: https://azure.microsoft.com/support/legal/sla/traffic-manager
[site-recovery]: /azure/site-recovery/azure-to-azure-quickstart/
[site-recovery-test-failover]: /azure/site-recovery/azure-to-azure-tutorial-dr-drill/
[site-recovery-failover]: /azure/site-recovery/azure-to-azure-tutorial-failover-failback/
[deployment-topologies]: ./disaster-recovery-azure-applications.md#deployment-topologies-for-disaster-recovery
[bulkhead-pattern]: ../patterns/bulkhead.md
[terraform]: /azure/virtual-machines/windows/infrastructure-automation#terraform
[ansible]: /azure/virtual-machines/windows/infrastructure-automation#ansible
[chef]: /azure/virtual-machines/windows/infrastructure-automation#chef
[puppet]: /azure/virtual-machines/windows/infrastructure-automation#puppet
[template-deployment]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[cloud-init]: /azure/virtual-machines/windows/infrastructure-automation#cloud-init
[azure-devops-services]: /azure/virtual-machines/windows/infrastructure-automation#azure-devops-services
[jenkins]: /azure/virtual-machines/windows/infrastructure-automation#jenkins
[staging-slots]: /azure/app-service/deploy-staging-slots
[powershell]: /powershell/azure/overview
[cli]: /cli/azure
