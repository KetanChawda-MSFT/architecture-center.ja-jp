---
title: "データ ストアの選択条件"
description: "Azure コンピューティング オプションの概要"
author: MikeWasson
ms.openlocfilehash: 7fb75cd334438c5b985fa04ad8afe3236f2391f8
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="criteria-for-choosing-a-data-store"></a><span data-ttu-id="2c2a6-103">データ ストアの選択条件</span><span class="sxs-lookup"><span data-stu-id="2c2a6-103">Criteria for choosing a data store</span></span>

<span data-ttu-id="2c2a6-104">Azure では、多数のデータ ストレージ ソリューションをサポートしており、各ソリューションではさまざまな機能を提供しています。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-104">Azure supports many types of data storage solutions, each providing different features and capabilities.</span></span> <span data-ttu-id="2c2a6-105">この記事では、データ ストアを評価するときに使用する比較条件について説明します。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-105">This article describes the comparison criteria you should use when evaluating a data store.</span></span> <span data-ttu-id="2c2a6-106">目標は、どの種類のデータ ストレージがソリューションの要件に見合うかを判断する際に役立つことです。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-106">The goal is to help you determine which data storage types can meet your solution's requirements.</span></span>

## <a name="general-considerations"></a><span data-ttu-id="2c2a6-107">一般的な考慮事項</span><span class="sxs-lookup"><span data-stu-id="2c2a6-107">General Considerations</span></span>

<span data-ttu-id="2c2a6-108">比較を開始するために、データのニーズに関する以下の情報をできるだけ収集してください。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-108">To start your comparison, gather as much of the following information as you can about your data needs.</span></span> <span data-ttu-id="2c2a6-109">この情報は、どの種類のデータ ストレージがニーズに見合うかを判断するうえで役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-109">This information will help you to determine which data storage types will meet your needs.</span></span>

### <a name="functional-requirements"></a><span data-ttu-id="2c2a6-110">機能の要件</span><span class="sxs-lookup"><span data-stu-id="2c2a6-110">Functional requirements</span></span>

- <span data-ttu-id="2c2a6-111">**データ形式**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-111">**Data format**.</span></span> <span data-ttu-id="2c2a6-112">どのような種類のデータを格納する予定ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-112">What type of data are you intending to store?</span></span> <span data-ttu-id="2c2a6-113">一般的な種類として、トランザクション データ、JSON オブジェクト、利用統計情報、検索インデックス、またはフラット ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-113">Common types include transactional data, JSON objects, telemetry, search indexes, or flat files.</span></span>
- <span data-ttu-id="2c2a6-114">**データ サイズ**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-114">**Data size**.</span></span> <span data-ttu-id="2c2a6-115">格納する必要があるデータの大きさは、どのくらいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-115">How large are the entities you need to store?</span></span> <span data-ttu-id="2c2a6-116">これらのエンティティは単一のドキュメントとして維持する必要がありますか、それとも複数のドキュメント、テーブル、コレクションなどに分割できますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-116">Will these entities need to be maintained as a single document, or can they be split across multiple documents, tables, collections, and so forth?</span></span>
- <span data-ttu-id="2c2a6-117">**スケールと構造**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-117">**Scale and structure**.</span></span> <span data-ttu-id="2c2a6-118">必要なストレージの総容量は、どのくらいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-118">What is the overall amount of storage capacity you need?</span></span> <span data-ttu-id="2c2a6-119">データをパーティション分割する予定ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-119">Do you anticipate partitioning your data?</span></span> 
- <span data-ttu-id="2c2a6-120">**データのリレーションシップ**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-120">**Data relationships**.</span></span> <span data-ttu-id="2c2a6-121">データでは、一対多または多対多のリレーションシップをサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-121">Will your data need to support one-to-many or many-to-many relationships?</span></span> <span data-ttu-id="2c2a6-122">リレーションシップ自体はデータの重要な部分ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-122">Are relationships themselves an important part of the data?</span></span> <span data-ttu-id="2c2a6-123">同じデータセット内、または外部のデータセットのデータを結合あるいは連結する必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-123">Will you need to join or otherwise combine data from within the same dataset, or from external datasets?</span></span> 
- <span data-ttu-id="2c2a6-124">**整合性モデル**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-124">**Consistency model**.</span></span> <span data-ttu-id="2c2a6-125">次の変更を加える前に 1 つのノードで行われた更新が他のノードに反映されていることが、どのくらい重要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-125">How important is it for updates made in one node to appear in other nodes, before further changes can be made?</span></span> <span data-ttu-id="2c2a6-126">結果として整合性があればよいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-126">Can you accept eventual consistency?</span></span> <span data-ttu-id="2c2a6-127">トランザクションの ACID 保証は必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-127">Do you need ACID guarantees for transactions?</span></span>
- <span data-ttu-id="2c2a6-128">**スキーマの柔軟性**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-128">**Schema flexibility**.</span></span> <span data-ttu-id="2c2a6-129">どの種類のスキーマをデータに適用しますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-129">What kind of schemas will you apply to your data?</span></span> <span data-ttu-id="2c2a6-130">固定スキーマ、書き込み時スキーマの手法、または読み取り時スキーマの手法を使用しますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-130">Will you use a fixed schema, a schema-on-write approach, or a schema-on-read approach?</span></span>
- <span data-ttu-id="2c2a6-131">**同時実行**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-131">**Concurrency**.</span></span> <span data-ttu-id="2c2a6-132">データを更新して同期するときに、どの種類の同時実行メカニズムを使用しますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-132">What kind of concurrency mechanism do you want to use when updating and synchronizing data?</span></span> <span data-ttu-id="2c2a6-133">アプリケーションは、潜在的に競合する可能性がある多数の更新を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-133">Will the application perform many updates that could potentially conflict.</span></span> <span data-ttu-id="2c2a6-134">実行する場合、レコード ロックとペシミスティック同時実行制御が必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-134">If so, you may requiring record locking and pessimistic concurrency control.</span></span> <span data-ttu-id="2c2a6-135">代わりに、オプティミスティック同時実行制御をサポートすることはできますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-135">Alternatively, can you support optimistic concurrency controls?</span></span> <span data-ttu-id="2c2a6-136">サポートできる場合、簡単なタイムスタンプに基づく同時実行制御で十分ですか、あるいは、追加機能として複数バージョンの同時実行制御が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-136">If so, is simple timestamp-based concurrency control enough, or do you need the added functionality of multi-version concurrency control?</span></span>
- <span data-ttu-id="2c2a6-137">**データの移動**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-137">**Data movement**.</span></span> <span data-ttu-id="2c2a6-138">ソリューションでは、ETL タスクを実行して他のストアやデータ ウェアハウスにデータを移動する必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-138">Will your solution need to perform ETL tasks to move data to other stores or data warehouses?</span></span>
- <span data-ttu-id="2c2a6-139">**データ ライフサイクル**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-139">**Data lifecycle**.</span></span> <span data-ttu-id="2c2a6-140">データは 1 回だけ書き込んで、複数回読み取りますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-140">Is the data write-once, read-many?</span></span> <span data-ttu-id="2c2a6-141">クールまたはコールド ストレージに移動できますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-141">Can it be moved into cool or cold storage?</span></span>
- <span data-ttu-id="2c2a6-142">**サポートされるその他の機能**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-142">**Other supported features**.</span></span> <span data-ttu-id="2c2a6-143">スキーマ検証、集計、インデックス作成、フルテキスト検索、MapReduce、その他のクエリ機能など、他の特定の機能が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-143">Do you need any other specific features, such as schema validation, aggregation, indexing, full-text search, MapReduce, or other query capabilities?</span></span>

### <a name="non-functional-requirements"></a><span data-ttu-id="2c2a6-144">機能以外の要件</span><span class="sxs-lookup"><span data-stu-id="2c2a6-144">Non-functional requirements</span></span>

- <span data-ttu-id="2c2a6-145">**パフォーマンスとスケーラビリティ**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-145">**Performance and scalability**.</span></span> <span data-ttu-id="2c2a6-146">どのようなデータ パフォーマンス要件がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-146">What are your data performance requirements?</span></span> <span data-ttu-id="2c2a6-147">データ取り込み率およびデータ処理率に関する特定の要件はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-147">Do you have specific requirements for data ingestion rates and data processing rates?</span></span> <span data-ttu-id="2c2a6-148">取り込まれた後のクエリ実行とデータ集計で許容できる応答時間はどのくらいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-148">What are the acceptable response times for querying and aggregation of data once ingested?</span></span> <span data-ttu-id="2c2a6-149">データ ストアはどのくらいの規模にスケールアップする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-149">How large will you need the data store to scale up?</span></span> <span data-ttu-id="2c2a6-150">ワークロードの負荷がより高いのは、読み取りまたは書き込みのどちらですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-150">Is your workload more read-heavy or write-heavy?</span></span>
- <span data-ttu-id="2c2a6-151">**信頼性**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-151">**Reliability**.</span></span> <span data-ttu-id="2c2a6-152">SLA 全体の何をサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-152">What overall SLA do you need to support?</span></span> <span data-ttu-id="2c2a6-153">データ コンシューマーに提供する必要があるフォールトトレランスのレベルは、どのくらいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-153">What level of fault-tolerance do you need to provide for data consumers?</span></span> <span data-ttu-id="2c2a6-154">どのような種類のバックアップおよび復元の機能が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-154">What kind of backup and restore capabilities do you need?</span></span> 
- <span data-ttu-id="2c2a6-155">**レプリケーション**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-155">**Replication**.</span></span> <span data-ttu-id="2c2a6-156">複数のレプリカまたはリージョンにデータを配布する必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-156">Will your data need to be distributed among multiple replicas or regions?</span></span> <span data-ttu-id="2c2a6-157">どのような種類のデータ レプリケーション機能が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-157">What kind of data replication capabilities do you require?</span></span> 
- <span data-ttu-id="2c2a6-158">**制限**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-158">**Limits**.</span></span> <span data-ttu-id="2c2a6-159">特定のデータ ストアの制限値で、スケール、接続数、およびスループットの要件に対応する予定ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-159">Will the limits of a particular data store support your requirements for scale, number of connections, and throughput?</span></span> 

### <a name="management-and-cost"></a><span data-ttu-id="2c2a6-160">管理とコスト</span><span class="sxs-lookup"><span data-stu-id="2c2a6-160">Management and cost</span></span>

- <span data-ttu-id="2c2a6-161">**管理されたサービス**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-161">**Managed service**.</span></span> <span data-ttu-id="2c2a6-162">IaaS でホストされるデータ ストアにしかない特定の機能が必要な場合以外は、可能であれば、管理されたデータ サービスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-162">When possible, use a managed data service, unless you require specific capabilities that can only be found in an IaaS-hosted data store.</span></span>
- <span data-ttu-id="2c2a6-163">**利用可能なリージョン**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-163">**Region availability**.</span></span> <span data-ttu-id="2c2a6-164">管理されたサービスの場合、すべての Azure リージョンでそのサービスを利用できますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-164">For managed services, is the service available in all Azure regions?</span></span> <span data-ttu-id="2c2a6-165">ソリューションは、特定の Azure リージョンでホストする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-165">Does your solution need to be hosted in certain Azure regions?</span></span>
- <span data-ttu-id="2c2a6-166">**移植性**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-166">**Portability**.</span></span> <span data-ttu-id="2c2a6-167">オンプレミス、外部のデータセンター、またはその他のクラウド ホスティング環境にデータを移行する必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-167">Will your data need to migrated to on-premises, external datacenters, or other cloud hosting environments?</span></span>
- <span data-ttu-id="2c2a6-168">**ライセンス**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-168">**Licensing**.</span></span> <span data-ttu-id="2c2a6-169">専用のライセンスまたは OSS ライセンスのどちらかで、種類の希望はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-169">Do you have a preference of a proprietary versus OSS license type?</span></span> <span data-ttu-id="2c2a6-170">使用できるライセンスの種類について、他の外部的な制約はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-170">Are there any other external restrictions on what type of license you can use?</span></span>
- <span data-ttu-id="2c2a6-171">**総コスト**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-171">**Overall cost**.</span></span> <span data-ttu-id="2c2a6-172">ソリューション内でサービスを使用する総コストは、どのくらいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-172">What is the overall cost of using the service within your solution?</span></span> <span data-ttu-id="2c2a6-173">稼働時間とスループットの要件をサポートするために、どのくらいの数のインスタンスを実行する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-173">How many instances will need to run, to support your uptime and throughput requirements?</span></span> <span data-ttu-id="2c2a6-174">この計算では、運用コストを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-174">Consider operations costs in this calculation.</span></span> <span data-ttu-id="2c2a6-175">管理されたサービスが好まれる 1 つの理由に、運用コストの削減があります。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-175">One reason to prefer managed services is the reduced operational cost.</span></span>
- <span data-ttu-id="2c2a6-176">**コスト効率**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-176">**Cost effectiveness**.</span></span> <span data-ttu-id="2c2a6-177">より高いコスト効率でデータを格納するために、データをパーティション分割できますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-177">Can you partition your data, to store it more cost effectively?</span></span> <span data-ttu-id="2c2a6-178">たとえば、高価なリレーショナル データベースからオブジェクト ストアへ大規模なオブジェクトを移動できますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-178">For example, can you move large objects out of an expensive relational database into an object store?</span></span>

### <a name="security"></a><span data-ttu-id="2c2a6-179">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-179">Security</span></span>

- <span data-ttu-id="2c2a6-180">**セキュリティ**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-180">**Security**.</span></span> <span data-ttu-id="2c2a6-181">どの種類の暗号化が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-181">What type of encryption do you require?</span></span> <span data-ttu-id="2c2a6-182">保存時に暗号化は必要ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-182">Do you need encryption at rest?</span></span> <span data-ttu-id="2c2a6-183">データへの接続に使用する認証メカニズムは何ですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-183">What authentication mechanism do you want to use to connect to your data?</span></span>
- <span data-ttu-id="2c2a6-184">**監査**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-184">**Auditing**.</span></span> <span data-ttu-id="2c2a6-185">どの種類の監査ログを生成する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-185">What kind of audit log do you need to generate?</span></span>
- <span data-ttu-id="2c2a6-186">**ネットワーク要件**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-186">**Networking requirements**.</span></span> <span data-ttu-id="2c2a6-187">他のネットワーク リソースからのデータへのアクセスを制限したり、あるいは管理したりする必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-187">Do you need to restrict or otherwise manage access to your data from other network resources?</span></span> <span data-ttu-id="2c2a6-188">データは、Azure 環境の内部からだけアクセスできればよいですか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-188">Does data need to be accessible only from inside the Azure environment?</span></span> <span data-ttu-id="2c2a6-189">データは特定の IP アドレスやサブネットからアクセスできる必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-189">Does the data need to be accessible from specific IP addresses or subnets?</span></span> <span data-ttu-id="2c2a6-190">オンプレミスまたは外部の他のデータセンターでホストされているアプリケーションやサービスから、データにアクセスする必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-190">Does it need to be accessible from applications or services hosted on-premises or in other external datacenters?</span></span>

### <a name="devops"></a><span data-ttu-id="2c2a6-191">DevOps</span><span class="sxs-lookup"><span data-stu-id="2c2a6-191">DevOps</span></span>

- <span data-ttu-id="2c2a6-192">**スキル セット**。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-192">**Skill set**.</span></span> <span data-ttu-id="2c2a6-193">チームが使い慣れている特定のプログラミング言語、オペレーティング システム、その他のテクノロジはありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-193">Are there particular programming languages, operating systems, or other technology that your team is particularly adept at using?</span></span> <span data-ttu-id="2c2a6-194">チームで使用することが難しいテクノロジは、他にありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-194">Are there others that would be difficult for your team to work with?</span></span>
- <span data-ttu-id="2c2a6-195">**クライアント**。お使いの開発言語にふさわしいクライアント サポートはありますか。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-195">**Clients** Is there good client support for your development languages?</span></span>

<span data-ttu-id="2c2a6-196">以降のセクションでは、ワークロード プロファイル、データ型、およびサンプルの使用例の観点から、さまざまなデータ ストア モデルを比較します。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-196">The following sections compare various data store models in terms of workload profile, data types, and example use cases.</span></span>

## <a name="relational-database-management-systems-rdbms"></a><span data-ttu-id="2c2a6-197">リレーショナル データベース管理システム (RDBMS)</span><span class="sxs-lookup"><span data-stu-id="2c2a6-197">Relational database management systems (RDBMS)</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-198">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-198">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-199">新しいレコードの作成と既存データの更新の両方が、定期的に発生する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-199">Both the creation of new records and updates to existing data happen regularly.</span></span></li>
            <li><span data-ttu-id="2c2a6-200">1 つのトランザクションで複数の操作を完了する必要がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-200">Multiple operations have to be completed in a single transaction.</span></span></li>
            <li><span data-ttu-id="2c2a6-201">参照タブを実行する集計関数が必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-201">Requires aggregation functions to perform cross-tabulation.</span></span></li>
            <li><span data-ttu-id="2c2a6-202">レポート作成ツールによる強固な統合が必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-202">Strong integration with reporting tools is required.</span></span></li>
            <li><span data-ttu-id="2c2a6-203">データベースの制約を使用して、リレーションシップが強制的に適用される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-203">Relationships are enforced using database constraints.</span></span></li>
            <li><span data-ttu-id="2c2a6-204">クエリのパフォーマンスを最適化するために、インデックスが使用される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-204">Indexes are used to optimize query performance.</span></span></li>
            <li><span data-ttu-id="2c2a6-205">データの特定のサブセットへのアクセスを許可する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-205">Allows access to specific subsets of data.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-206">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-206">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-207">データは高度に正規化される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-207">Data is highly normalized.</span></span></li>
            <li><span data-ttu-id="2c2a6-208">データベース スキーマが必要であり、強制的に適用される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-208">Database schemas are required and enforced.</span></span></li>
            <li><span data-ttu-id="2c2a6-209">データベース内のデータ エンティティ間の多対多リレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-209">Many-to-many relationships between data entities in the database.</span></span></li>
            <li><span data-ttu-id="2c2a6-210">制約はスキーマで定義され、データベース内の任意のデータに適用される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-210">Constraints are defined in the schema and imposed on any data in the database.</span></span></li>
            <li><span data-ttu-id="2c2a6-211">データには、高度な整合性が必要である。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-211">Data requires high integrity.</span></span> <span data-ttu-id="2c2a6-212">インデックスおよびリレーションシップは、正確に維持される必要がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-212">Indexes and relationships need to be maintained accurately.</span></span></li>
            <li><span data-ttu-id="2c2a6-213">データには、強固な一貫性が必要である。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-213">Data requires strong consistency.</span></span> <span data-ttu-id="2c2a6-214">全データがすべてのユーザーおよびプロセスと 100% 整合性があることを保証した方法で、トランザクションが処理される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-214">Transactions operate in a way that ensures all data are 100% consistent for all users and processes.</span></span></li>
            <li><span data-ttu-id="2c2a6-215">個々のデータ エントリのサイズは、小規模から中規模であることが望まれている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-215">Size of individual data entries is intended to be small to medium-sized.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-216">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-216">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-217">基幹業務 (人材管理、顧客関係管理、エンタープライズ リソース プランニング)</span><span class="sxs-lookup"><span data-stu-id="2c2a6-217">Line of business  (human capital management, customer relationship management, enterprise resource planning)</span></span></li>
            <li><span data-ttu-id="2c2a6-218">在庫管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-218">Inventory management</span></span></li>
            <li><span data-ttu-id="2c2a6-219">レポート データベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-219">Reporting database</span></span></li>
            <li><span data-ttu-id="2c2a6-220">会計</span><span class="sxs-lookup"><span data-stu-id="2c2a6-220">Accounting</span></span></li>
            <li><span data-ttu-id="2c2a6-221">アセット管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-221">Asset management</span></span></li>
            <li><span data-ttu-id="2c2a6-222">ファンド管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-222">Fund management</span></span></li>
            <li><span data-ttu-id="2c2a6-223">注文管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-223">Order management</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="document-databases"></a><span data-ttu-id="2c2a6-224">ドキュメント データベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-224">Document databases</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-225">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-225">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-226">汎用的な用途。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-226">General purpose.</span></span></li>
            <li><span data-ttu-id="2c2a6-227">挿入や更新の操作は共通である。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-227">Insert and update operations are common.</span></span> <span data-ttu-id="2c2a6-228">新しいレコードの作成と既存データの更新の両方が、定期的に発生します。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-228">Both the creation of new records and updates to existing data happen regularly.</span></span></li>
            <li><span data-ttu-id="2c2a6-229">非オブジェクト リレーショナル インピー ダンスは適合しない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-229">No object-relational impedance mismatch.</span></span> <span data-ttu-id="2c2a6-230">ドキュメントには、アプリケーション コードで使用されるオブジェクト構造のほうがより適合します。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-230">Documents can better match the object structures used in application code.</span></span></li>
            <li><span data-ttu-id="2c2a6-231">オプティミスティック同時実行制御が、より一般的に使用される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-231">Optimistic concurrency is more commonly used.</span></span></li>
            <li><span data-ttu-id="2c2a6-232">データは、アプリケーションを使用して変更され処理される必要がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-232">Data must be modified and processed by consuming application.</span></span></li>
            <li><span data-ttu-id="2c2a6-233">データは、複数のフィールドにインデックスを必要とする。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-233">Data requires index on multiple fields.</span></span></li>
            <li><span data-ttu-id="2c2a6-234">個々のドキュメントが取得され、単一のブロックとして書き込まれる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-234">Individual documents are retrieved and written as a single block.</span></span></li>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-235">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-235">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-236">非正規化された方法で、データを管理できる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-236">Data can be managed in de-normalized way.</span></span></li>
            <li><span data-ttu-id="2c2a6-237">個々のドキュメント データのサイズは比較的小さい。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-237">Size of individual document data is relatively small.</span></span></li>
            <li><span data-ttu-id="2c2a6-238">各ドキュメントの種類で、独自のスキーマを使用できる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-238">Each document type can use its own schema.</span></span></li>
            <li><span data-ttu-id="2c2a6-239">ドキュメントには、省略可能なフィールドを含めることができる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-239">Documents can include optional fields.</span></span></li>
            <li><span data-ttu-id="2c2a6-240">ドキュメント データは半構造化されている。つまり、各フィールドのデータは、厳密には定義されていません。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-240">Document data is semi-structured, meaning that data types of each field are not strictly defined.</span></span></li>
            <li><span data-ttu-id="2c2a6-241">データ集計がサポートされている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-241">Data aggregation is supported.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-242">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-242">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-243">製品カタログ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-243">Product catalog</span></span></li>
            <li><span data-ttu-id="2c2a6-244">ユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="2c2a6-244">User accounts</span></span></li>
            <li><span data-ttu-id="2c2a6-245">部品表</span><span class="sxs-lookup"><span data-stu-id="2c2a6-245">Bill of materials</span></span></li>
            <li><span data-ttu-id="2c2a6-246">パーソナル化</span><span class="sxs-lookup"><span data-stu-id="2c2a6-246">Personalization</span></span></li>
            <li><span data-ttu-id="2c2a6-247">コンテンツ管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-247">Content management</span></span></li>
            <li><span data-ttu-id="2c2a6-248">操作データ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-248">Operations data</span></span></li>
            <li><span data-ttu-id="2c2a6-249">在庫管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-249">Inventory management</span></span></li>
            <li><span data-ttu-id="2c2a6-250">トランザクション履歴データ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-250">Transaction history data</span></span></li>
            <li><span data-ttu-id="2c2a6-251">その他の NoSQL ストアの具体化されたビュー。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-251">Materialized view of other NoSQL stores.</span></span> <span data-ttu-id="2c2a6-252">ファイルや BLOB のインデックスを置換する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-252">Replaces file/BLOB indexing.</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="keyvalue-stores"></a><span data-ttu-id="2c2a6-253">キーと値のペア</span><span class="sxs-lookup"><span data-stu-id="2c2a6-253">Key/value stores</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-254">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-254">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-255">データは識別され、単一の ID キーを使用して、ディクショナリのようにアクセスされる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-255">Data is identified and accessed using a single ID key, like a dictionary.</span></span></li>
            <li><span data-ttu-id="2c2a6-256">極めて高い拡張性。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-256">Massively scalable.</span></span></li>
            <li><span data-ttu-id="2c2a6-257">結合、ロック、統合は必要ない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-257">No joins, lock, or unions are required.</span></span></li>
            <li><span data-ttu-id="2c2a6-258">集計メカニズムは使用されない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-258">No aggregation mechanisms are used.</span></span></li>
            <li><span data-ttu-id="2c2a6-259">一般に、セカンダリ インデックスは使用されない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-259">Secondary indexes are generally not used.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-260">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-260">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-261">データ サイズが大きくなる傾向がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-261">Data size tends to be large.</span></span></li>
            <li><span data-ttu-id="2c2a6-262">各キーは、アンマネージ データの BLOB である単一の値に関連付けられる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-262">Each key is associated with a single value, which is an unmanaged data BLOB.</span></span></li>
            <li><span data-ttu-id="2c2a6-263">スキーマの適用はない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-263">There is no schema enforcement.</span></span></li>
            <li><span data-ttu-id="2c2a6-264">エンティティ間にリレーションシップはない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-264">No relationships between entities.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-265">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-265">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-266">データ キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-266">Data caching</span></span></li>
            <li><span data-ttu-id="2c2a6-267">セッションの管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-267">Session management</span></span></li>
            <li><span data-ttu-id="2c2a6-268">ユーザーの基本設定とプロファイルの管理</span><span class="sxs-lookup"><span data-stu-id="2c2a6-268">User preference and profile management</span></span></li>
            <li><span data-ttu-id="2c2a6-269">製品推奨と広告提供</span><span class="sxs-lookup"><span data-stu-id="2c2a6-269">Product recommendation and ad serving</span></span></li>
            <li><span data-ttu-id="2c2a6-270">ディクショナリ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-270">Dictionaries</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="graph-databases"></a><span data-ttu-id="2c2a6-271">グラフ データベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-271">Graph databases</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-272">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-272">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-273">データ項目間のリレーションシップは非常に複雑であり、データ項目間に多数のホップが関連している。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-273">The relationships between data items are very complex, involving many hops between related data items.</span></span></li>
            <li><span data-ttu-id="2c2a6-274">データ項目間のリレーションシップは動的であり、時間と共に変化する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-274">The relationship between data items are dynamic and change over time.</span></span></li>
            <li><span data-ttu-id="2c2a6-275">オブジェクト間のリレーションシップは最上位の扱いになり、外部キーと走査のための結合は必要ない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-275">Relationships between objects are first-class citizens, without requiring foreign-keys and joins to traverse.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-276">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-276">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-277">データは、ノードとリレーションシップで構成される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-277">Data is comprised of nodes and relationships.</span></span></li>
            <li><span data-ttu-id="2c2a6-278">ノードは、テーブル行または JSON ドキュメントに似ている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-278">Nodes are similar to table rows or JSON documents.</span></span></li>
            <li><span data-ttu-id="2c2a6-279">リレーションシップはノードと同様に重要であり、クエリ言語で直接公開される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-279">Relationships are just as important as nodes, and are exposed directly in the query language.</span></span></li>
            <li><span data-ttu-id="2c2a6-280">複数の電話番号の保持者など、複合オブジェクトは、走査可能なリレーションシップと組み合わせて、個々のより小さなノードに分割される傾向がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-280">Composite objects, such as a person with multiple phone numbers, tend to be broken into separate, smaller nodes, combined with traversable relationships</span></span> </li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-281">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-281">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-282">組織図</span><span class="sxs-lookup"><span data-stu-id="2c2a6-282">Organization charts</span></span></li>
            <li><span data-ttu-id="2c2a6-283">ソーシャル グラフ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-283">Social graphs</span></span></li>
            <li><span data-ttu-id="2c2a6-284">不正行為の検出</span><span class="sxs-lookup"><span data-stu-id="2c2a6-284">Fraud detection</span></span></li>
            <li><span data-ttu-id="2c2a6-285">分析</span><span class="sxs-lookup"><span data-stu-id="2c2a6-285">Analytics</span></span></li>
            <li><span data-ttu-id="2c2a6-286">推奨エンジン</span><span class="sxs-lookup"><span data-stu-id="2c2a6-286">Recommendation engines</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="column-family-databases"></a><span data-ttu-id="2c2a6-287">列ファミリのデータベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-287">Column-family databases</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-288">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-288">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-289">ほとんどの列ファミリ データベースでは、極めて高速で書き込み操作を実行する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-289">Most column-family databases perform write operations extremely quickly.</span></span></li>
            <li><span data-ttu-id="2c2a6-290">更新および削除の操作は、ほとんど行われない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-290">Update and delete operations are rare.</span></span></li>
            <li><span data-ttu-id="2c2a6-291">高いスループットと低い待機時間でのアクセスを提供するように設計されている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-291">Designed to provide high throughput and low-latency access.</span></span></li>
            <li><span data-ttu-id="2c2a6-292">より大規模なレコード内にある特定のフィールド セットへの簡単なクエリ アクセスをサポートしている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-292">Supports easy query access to a particular set of fields within a much larger record.</span></span></li>
            <li><span data-ttu-id="2c2a6-293">極めて高い拡張性。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-293">Massively scalable.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-294">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-294">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-295">データは、キー列と 1 つまたは複数の列ファミリで構成されたテーブルに格納される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-295">Data is stored in tables consisting of a key column and one or more column families.</span></span></li>
            <li><span data-ttu-id="2c2a6-296">特定の列は、個々の行別に変更できる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-296">Specific columns can vary by individual rows.</span></span></li>
            <li><span data-ttu-id="2c2a6-297">個々のセルには get および put コマンド経由でアクセスできる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-297">Individual cells are accessed via get and put commands</span></span></li>
            <li><span data-ttu-id="2c2a6-298">スキャン コマンドを使って、複数の行が返される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-298">Multiple rows are returned using a scan command.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-299">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-299">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-300">Recommendations</span><span class="sxs-lookup"><span data-stu-id="2c2a6-300">Recommendations</span></span></li>
            <li><span data-ttu-id="2c2a6-301">パーソナル化</span><span class="sxs-lookup"><span data-stu-id="2c2a6-301">Personalization</span></span></li>
            <li><span data-ttu-id="2c2a6-302">センサー データ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-302">Sensor data</span></span></li>
            <li><span data-ttu-id="2c2a6-303">テレメトリ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-303">Telemetry</span></span></li>
            <li><span data-ttu-id="2c2a6-304">メッセージング</span><span class="sxs-lookup"><span data-stu-id="2c2a6-304">Messaging</span></span></li>
            <li><span data-ttu-id="2c2a6-305">ソーシャル メディア分析</span><span class="sxs-lookup"><span data-stu-id="2c2a6-305">Social media analytics</span></span></li>
            <li><span data-ttu-id="2c2a6-306">Web 分析</span><span class="sxs-lookup"><span data-stu-id="2c2a6-306">Web analytics</span></span></li>
            <li><span data-ttu-id="2c2a6-307">アクティビティの監視</span><span class="sxs-lookup"><span data-stu-id="2c2a6-307">Activity monitoring</span></span></li>
            <li><span data-ttu-id="2c2a6-308">天気およびその他のタイム シリーズ データ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-308">Weather and other time-series data</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="search-engine-databases"></a><span data-ttu-id="2c2a6-309">検索エンジンのデータベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-309">Search engine databases</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-310">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-310">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-311">複数のソースおよびサービスからのデータにインデックスを作成する。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-311">Indexing data from multiple sources and services.</span></span></li>
            <li><span data-ttu-id="2c2a6-312">クエリはアドホックであり、複雑になる場合がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-312">Queries are ad-hoc and can be complex.</span></span></li>
            <li><span data-ttu-id="2c2a6-313">集計が必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-313">Requires aggregation.</span></span></li>
            <li><span data-ttu-id="2c2a6-314">フルテキスト検索が必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-314">Full text search is required.</span></span></li>
            <li><span data-ttu-id="2c2a6-315">アドホックのセルフ サービス クエリが必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-315">Ad hoc self-service query is required.</span></span></li>
            <li><span data-ttu-id="2c2a6-316">すべてのフィールドでインデックスによるデータ分析が必要になる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-316">Data analysis with index on all fields is required.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-317">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-317">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-318">半構造化または非構造化</span><span class="sxs-lookup"><span data-stu-id="2c2a6-318">Semi-structured or unstructured</span></span></li>
            <li><span data-ttu-id="2c2a6-319">テキスト</span><span class="sxs-lookup"><span data-stu-id="2c2a6-319">Text</span></span></li>
            <li><span data-ttu-id="2c2a6-320">構造化データへの参照を備えたテキスト</span><span class="sxs-lookup"><span data-stu-id="2c2a6-320">Text with reference to structured data</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-321">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-321">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-322">製品カタログ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-322">Product catalogs</span></span></li>
            <li><span data-ttu-id="2c2a6-323">サイトの検索</span><span class="sxs-lookup"><span data-stu-id="2c2a6-323">Site search</span></span></li>
            <li><span data-ttu-id="2c2a6-324">ログの記録</span><span class="sxs-lookup"><span data-stu-id="2c2a6-324">Logging</span></span></li>
            <li><span data-ttu-id="2c2a6-325">分析</span><span class="sxs-lookup"><span data-stu-id="2c2a6-325">Analytics</span></span></li>
            <li><span data-ttu-id="2c2a6-326">ショッピング サイト</span><span class="sxs-lookup"><span data-stu-id="2c2a6-326">Shopping sites</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="data-warehouse"></a><span data-ttu-id="2c2a6-327">データ ウェアハウス</span><span class="sxs-lookup"><span data-stu-id="2c2a6-327">Data warehouse</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-328">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-328">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-329">データ分析</span><span class="sxs-lookup"><span data-stu-id="2c2a6-329">Data analytics</span></span></li>
            <li><span data-ttu-id="2c2a6-330">企業の BI</span><span class="sxs-lookup"><span data-stu-id="2c2a6-330">Enterprise BI</span></span>   </li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-331">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-331">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-332">複数のソースからの履歴データ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-332">Historical data from multiple sources.</span></span></li>
            <li><span data-ttu-id="2c2a6-333">通常は、"star" または "snowflake" スキーマで非正規化され、ファクト テーブルおよびディメンション テーブルで構成されている。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-333">Usually denormalized in a "star" or "snowflake" schema, consisting of fact and dimension tables.</span></span></li>
            <li><span data-ttu-id="2c2a6-334">通常は、スケジュールに基づいて新しいデータと共に読み込まれる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-334">Usually loaded with new data on a scheduled basis.</span></span></li>
            <li><span data-ttu-id="2c2a6-335">多くの場合、ディメンション テーブルにはエンティティの複数の履歴バージョンが含まれ、*"緩やかに変化するディメンション"* として参照される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-335">Dimension tables often include multiple historic versions of an entity, referred to as a *slowly changing dimension*.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-336">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-336">**Examples**</span></span></td>
    <td><span data-ttu-id="2c2a6-337">分析モデル、レポート、およびダッシュボードのデータを提供するエンタープライズ データ ウェアハウス。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-337">An enterprise data warehouse that provides data for analytical models, reports, and dashboards.</span></span>
    </td>
</tr>
</table>


## <a name="time-series-databases"></a><span data-ttu-id="2c2a6-338">タイム シリーズ データベース</span><span class="sxs-lookup"><span data-stu-id="2c2a6-338">Time series databases</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-339">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-339">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-340">操作の大部分 (95 99%) は書き込みである。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-340">An overwhelmingly proportion of operations (95-99%) are writes.</span></span></li>
            <li><span data-ttu-id="2c2a6-341">レコードは通常、時系列で追加される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-341">Records are generally appended sequentially in time order.</span></span></li>
            <li><span data-ttu-id="2c2a6-342">更新はとんど行われない。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-342">Updates are rare.</span></span></li>
            <li><span data-ttu-id="2c2a6-343">削除は一括で、連続したブロックまたはレコードに対して行われる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-343">Deletes occur in bulk, and are made to contiguous blocks or records.</span></span></li>
            <li><span data-ttu-id="2c2a6-344">読み取り要求は、使用可能なメモリより大規模になる場合がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-344">Read requests can be larger than available memory.</span></span></li>
            <li><span data-ttu-id="2c2a6-345">複数の読み取りが同時に発生することが頻繁にある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-345">It's common for multiple reads to occur simultaneously.</span></span></li>
            <li><span data-ttu-id="2c2a6-346">データは、時系列の昇順または降順のどちらかで、読み込まれる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-346">Data is read sequentially in either ascending or descending time order.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-347">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-347">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-348">主キーおよび並べ替えのメカニズムとして使用されるタイムスタンプ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-348">A time stamp that is used as the primary key and sorting mechanism.</span></span></li>
            <li><span data-ttu-id="2c2a6-349">エントリまたはエントリに示された記述内容からの測定値。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-349">Measurements from the entry or descriptions of what the entry represents.</span></span></li>
            <li><span data-ttu-id="2c2a6-350">種類、配信元などエントリに関する情報の詳細を定義しているタグ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-350">Tags that define additional information about the type, origin, and other information about the entry.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-351">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-351">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-352">監視およびイベント テレメトリ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-352">Monitoring and event telemetry.</span></span></li>
            <li><span data-ttu-id="2c2a6-353">センサーまたはその他の IoT データ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-353">Sensor or other IoT data.</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="object-storage"></a><span data-ttu-id="2c2a6-354">オブジェクトのストレージ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-354">Object storage</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-355">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-355">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-356">キーによって識別される。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-356">Identified by key.</span></span></li>
            <li><span data-ttu-id="2c2a6-357">オブジェクトは、パブリックまたはプライベートでアクセスできる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-357">Objects may be publicly or privately accessible.</span></span></li>
            <li><span data-ttu-id="2c2a6-358">コンテンツは通常、スプレッドシート、イメージ、ビデオ ファイルなどのアセットである。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-358">Content is typically an asset such as a spreadsheet, image, or video file.</span></span></li>
            <li><span data-ttu-id="2c2a6-359">コンテンツは持続性 (永続性) があり、任意のアプリケーション層または仮想マシンの外部に存在する必要がある。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-359">Content must be durable (persistent), and external to any application tier or virtual machine.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-360">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-360">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-361">データ サイズが大きい。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-361">Data size is large.</span></span></li>
            <li><span data-ttu-id="2c2a6-362">Blob データ。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-362">Blob data.</span></span></li>
            <li><span data-ttu-id="2c2a6-363">値は非透過的である。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-363">Value is opaque.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-364">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-364">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-365">イメージ、ビデオ、office ドキュメント、PDF</span><span class="sxs-lookup"><span data-stu-id="2c2a6-365">Images, videos, office documents, PDFs</span></span></li>
            <li><span data-ttu-id="2c2a6-366">CSS、スクリプト、CSV</span><span class="sxs-lookup"><span data-stu-id="2c2a6-366">CSS, Scripts, CSV</span></span></li>
            <li><span data-ttu-id="2c2a6-367">静的 HTML、JSON</span><span class="sxs-lookup"><span data-stu-id="2c2a6-367">Static HTML, JSON</span></span></li>
            <li><span data-ttu-id="2c2a6-368">ログおよび監査ファイル</span><span class="sxs-lookup"><span data-stu-id="2c2a6-368">Log and audit files</span></span></li>
            <li><span data-ttu-id="2c2a6-369">データベースのバックアップ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-369">Database backups</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="shared-files"></a><span data-ttu-id="2c2a6-370">共有ファイル</span><span class="sxs-lookup"><span data-stu-id="2c2a6-370">Shared files</span></span>

<table>
<tr><td><span data-ttu-id="2c2a6-371">**ワークロード**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-371">**Workload**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-372">ファイル システムと対話する既存アプリからの移行。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-372">Migration from existing apps that interact with the file system.</span></span></li>
            <li><span data-ttu-id="2c2a6-373">SMB インターフェイスが必要である。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-373">Requires SMB interface.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-374">**データの種類**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-374">**Data type**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-375">フォルダーの階層セット内のファイル。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-375">Files in a hierarchical set of folders.</span></span></li>
            <li><span data-ttu-id="2c2a6-376">標準の I/O ライブラリを使ってアクセスできる。</span><span class="sxs-lookup"><span data-stu-id="2c2a6-376">Accessible with standard I/O libraries.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="2c2a6-377">**例**</span><span class="sxs-lookup"><span data-stu-id="2c2a6-377">**Examples**</span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="2c2a6-378">レガシ ファイル</span><span class="sxs-lookup"><span data-stu-id="2c2a6-378">Legacy files</span></span></li>
            <li><span data-ttu-id="2c2a6-379">多数の VM またはアプリ インスタンスからアクセス可能な共有コンテンツ</span><span class="sxs-lookup"><span data-stu-id="2c2a6-379">Shared content accessible among a number of VMs or app instances</span></span></li>
        </ul>
    </td>
</tr>
</table>