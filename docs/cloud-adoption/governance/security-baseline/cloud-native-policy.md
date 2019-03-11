---
title: 'CAF: クラウド ネイティブのセキュリティ ベースライン ポリシー'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: クラウド ネイティブのセキュリティ ベースライン ポリシー
author: BrianBlanchard
ms.openlocfilehash: fc161009f6ce7aa2b775fe6b3b53ff1e1d62a848
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901302"
---
# <a name="cloud-native-security-baseline-policy"></a><span data-ttu-id="ed927-103">クラウド ネイティブのセキュリティ ベースライン ポリシー</span><span class="sxs-lookup"><span data-stu-id="ed927-103">Cloud-Native Security Baseline policy</span></span>

<span data-ttu-id="ed927-104">[セキュリティ ベースライン](overview.md)は、[5 つあるクラウド ガバナンス規範](../governance-disciplines.md)のうちの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="ed927-104">[Security Baseline](overview.md) is one of the [five disciplines of Cloud Governance](../governance-disciplines.md).</span></span> <span data-ttu-id="ed927-105">この規範は、ネットワーク、デジタル資産、データの保護などを含む一般的なセキュリティ トピックに的を絞っています。[ポリシー レビュー ガイド](../policy-compliance/what-is-a-cloud-policy-review.md)の概要説明のように、CAF には 3 つのレベルの**サンプル ポリシー**が含まれています。5 つの規範それぞれに向けた、クラウド ネイティブ、エンタープライズ、およびクラウドの設計原則準拠レベルです。</span><span class="sxs-lookup"><span data-stu-id="ed927-105">This discipline focuses on general security topics including protection of the network, digital assets, data, etc. As outlined in the [policy review guide](../policy-compliance/what-is-a-cloud-policy-review.md), the CAF includes three levels of **sample policy**: Cloud-Native, Enterprise, and Cloud Design Principle Compliant for each of the five disciplines.</span></span> <span data-ttu-id="ed927-106">この記事では、セキュリティ ベースライン規範に向けたクラウド ネイティブのサンプル ポリシーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed927-106">This article discusses the Cloud-Native Sample Policy for the Security Baseline Discipline.</span></span>

> [!NOTE]
> <span data-ttu-id="ed927-107">Microsoft は、企業ポリシーや IT ポリシーを規定する立場にありません。</span><span class="sxs-lookup"><span data-stu-id="ed927-107">Microsoft is in no position to dictate corporate or IT policy.</span></span> <span data-ttu-id="ed927-108">この記事の目的は、お客様による社内ポリシー レビューの準備を助けることです。</span><span class="sxs-lookup"><span data-stu-id="ed927-108">This article is intended to help you prepare for an internal policy review.</span></span> <span data-ttu-id="ed927-109">このサンプル ポリシーは、使用を試みる前に、企業ポリシーを基準とした拡張、検証、テストが行われると想定されています。</span><span class="sxs-lookup"><span data-stu-id="ed927-109">It is assumed that this sample policy will be extended, validated, and tested against your corporate policy before attempting to use it.</span></span> <span data-ttu-id="ed927-110">このサンプル ポリシーの現状のままでの使用はお勧めできません。</span><span class="sxs-lookup"><span data-stu-id="ed927-110">Any use of this sample policy, as is, is discouraged.</span></span>

## <a name="policy-alignment"></a><span data-ttu-id="ed927-111">ポリシーの配置</span><span class="sxs-lookup"><span data-stu-id="ed927-111">Policy alignment</span></span>

<span data-ttu-id="ed927-112">このサンプル ポリシーは、クラウド ネイティブのシナリオを総合的に扱うものです。そのため、Azure によって提供されるツールとプラットフォームは、デプロイに関するビジネス上のリスクを軽減するうえで十分な機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="ed927-112">This sample policy synthesizes a Cloud Native scenario, meaning that the tools and platforms provided by Azure are sufficient to mitigate business risks regarding a deployment.</span></span> <span data-ttu-id="ed927-113">このシナリオでは、既定の Azure サービスの単純な構成で、十分な資産保護が提供されると想定しています。</span><span class="sxs-lookup"><span data-stu-id="ed927-113">In this scenario, it is assumed that a simple configuration of the default Azure services provides sufficient asset protection.</span></span>

## <a name="cloud-security-and-compliance"></a><span data-ttu-id="ed927-114">クラウドのセキュリティとコンプライアンス</span><span class="sxs-lookup"><span data-stu-id="ed927-114">Cloud security and compliance</span></span>

<span data-ttu-id="ed927-115">セキュリティは Azure のあらゆる側面に統合されていて、グローバルなセキュリティ インテリジェンス、高度な顧客用コントロール、安全で堅牢なインフラストラクチャに由来する、類のないセキュリティ上の利点が提供されます。</span><span class="sxs-lookup"><span data-stu-id="ed927-115">Security is integrated into every aspect of Azure, offering unique security advantages derived from global security intelligence, sophisticated customer-facing controls, and a secure, hardened infrastructure.</span></span> <span data-ttu-id="ed927-116">この強力な組み合わせにより、アプリケーションとデータの保護、コンプライアンスに関する取り組みの支援、あらゆる規模の組織に適したコスト効果の高いセキュリティの提供が可能になります。</span><span class="sxs-lookup"><span data-stu-id="ed927-116">This powerful combination helps protect your applications and data, support your compliance efforts, and provide cost-effective security for organizations of all sizes.</span></span> <span data-ttu-id="ed927-117">このアプローチは、どのようなセキュリティ ポリシーにもしっかりとした開始基盤となりますが、使用する予定のセキュリティ サービスに関して、同様に強力なセキュリティ プラクティスが不要になることはありません。</span><span class="sxs-lookup"><span data-stu-id="ed927-117">This approach creates a strong starting position for any security policy, but does not negate the need for equally strong security practices related to the security services being used.</span></span>

### <a name="built-in-security-controls"></a><span data-ttu-id="ed927-118">組み込みのセキュリティ制御</span><span class="sxs-lookup"><span data-stu-id="ed927-118">Built-in security controls</span></span>

<span data-ttu-id="ed927-119">セキュリティ制御が直感的でなく、個別に構成する必要がある場合には、強固なセキュリティ インフラストラクチャを維持することは困難です。</span><span class="sxs-lookup"><span data-stu-id="ed927-119">It’s hard to maintain a strong security infrastructure when security controls are not intuitive and need to be configured separately.</span></span> <span data-ttu-id="ed927-120">Azure には、さまざまなサービスにわたって、データとワークロードをすばやく保護し、ハイブリッド環境全体のリスクを管理するのに役立つ組み込みのセキュリティ制御が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed927-120">Azure includes built-in security controls across a variety of services that help you protect data and workloads quickly and manage risk across hybrid environments.</span></span> <span data-ttu-id="ed927-121">統合されるパートナー ソリューションでも、既存の保護をクラウドに容易に移行できます。</span><span class="sxs-lookup"><span data-stu-id="ed927-121">Integrated partner solutions also let you easily transition existing protections to the cloud.</span></span>

### <a name="cloud-native-identity-policies"></a><span data-ttu-id="ed927-122">クラウド ネイティブの ID ポリシー</span><span class="sxs-lookup"><span data-stu-id="ed927-122">Cloud Native identity policies</span></span>

<span data-ttu-id="ed927-123">ID は、セキュリティの新しい境界制御面となりつつあり、従来のネットワーク中心の観点から、その役割を引き継いでいます。</span><span class="sxs-lookup"><span data-stu-id="ed927-123">Identity is becoming the new boundary control plane for security, taking over that role from the traditional network-centric perspective.</span></span> <span data-ttu-id="ed927-124">ネットワーク境界はますます侵入しやすくなってきており、その境界防御は、Bring Your Own Device (BYOD) とクラウド アプリケーションが広まる前ほど有効ではなくなっています。</span><span class="sxs-lookup"><span data-stu-id="ed927-124">Network perimeters have become increasingly porous and that perimeter defense cannot be as effective as it was before the evolution of bring your own device (BYOD) and cloud applications.</span></span> <span data-ttu-id="ed927-125">Azure の ID 管理とアクセス制御では、すべてのアプリケーションに対するシームレスで安全なアクセスが実現されます。</span><span class="sxs-lookup"><span data-stu-id="ed927-125">Azure identity management and access control enable seamless, secure access to all your applications.</span></span>

<span data-ttu-id="ed927-126">クラウドとオンプレミスのディレクトリにわたる ID についてのサンプルのクラウド ネイティブ ポリシーには、以下のような要件が含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="ed927-126">A sample cloud native policy for identity across cloud and on-premises directories, could include requirements like the following:</span></span>

* <span data-ttu-id="ed927-127">ロールベースのアクセス制御 (RBAC)、多要素認証 (MFA)、およびシングル サインオン (SSO) によって承認されるリソースへのアクセス</span><span class="sxs-lookup"><span data-stu-id="ed927-127">Authorized access to resources with role-based access control (RBAC), multi-factor authentication (MFA), and single sign-on (SSO)</span></span>
* <span data-ttu-id="ed927-128">セキュリティ侵害の疑いのあるユーザー ID の迅速なリスク軽減</span><span class="sxs-lookup"><span data-stu-id="ed927-128">Quick mitigation of user identities suspected of compromise</span></span>
* <span data-ttu-id="ed927-129">ちょうどよいときに、タスクごとの単位で与えられる必要十分のアクセスによって、過剰な特権を持つ管理者資格情報の公開を制限</span><span class="sxs-lookup"><span data-stu-id="ed927-129">Just-in-time (JIT), just-enough access granted on a task-by-task basis to limit exposure of over-privileged admin credentials</span></span>
* <span data-ttu-id="ed927-130">Azure Active Directory を通じて、ユーザー ID とポリシーへのアクセスを複数の環境にわたって拡張</span><span class="sxs-lookup"><span data-stu-id="ed927-130">Extended user identity and access to policies across multiple environments through Azure Active Directory</span></span>

<span data-ttu-id="ed927-131">セキュリティ ベースラインのコンテキストで [ID ベースライン](../identity-baseline/overview.md)を理解することは大切ですが、[クラウド ガバナンスの 5 つの規範](../overview.md)では、[ID ベースライン](../identity-baseline/overview.md)は、セキュリティ ベースラインとは別の独自の規範であると見なしています。</span><span class="sxs-lookup"><span data-stu-id="ed927-131">While it is important to understand [Identity Baseline](../identity-baseline/overview.md) in the context of Security Baseline, the [Five Disciplines of Cloud Governance](../overview.md) calls out [Identity Baseline](../identity-baseline/overview.md) as its own discipline, separate from Security Baseline.</span></span>

### <a name="network-access-policies"></a><span data-ttu-id="ed927-132">ネットワーク アクセス ポリシー</span><span class="sxs-lookup"><span data-stu-id="ed927-132">Network access policies</span></span>

<span data-ttu-id="ed927-133">ネットワーク制御には、仮想ネットワーク、負荷分散、DNS、ゲートウェイなどのネットワーク要素の構成、管理、セキュリティ保護が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ed927-133">Network control includes the configuration, management, and securing of network elements such as virtual networking, load balancing, DNS, and gateways.</span></span> <span data-ttu-id="ed927-134">こうした制御が、サービスが通信と相互作用を行う手段を提供しています。</span><span class="sxs-lookup"><span data-stu-id="ed927-134">The controls provide a means for services to communicate and interoperate.</span></span> <span data-ttu-id="ed927-135">Azure には、アプリケーションとサービスの接続要件をサポートする、堅牢かつセキュリティで保護されたネットワーク インフラストラクチャが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed927-135">Azure includes a robust and secure networking infrastructure to support your application and service connectivity requirements.</span></span> <span data-ttu-id="ed927-136">ネットワーク接続は、Azure に配置されているリソース間、オンプレミスのリソースと Azure でホストされているリソース間、インターネットと Azure 間で可能です。</span><span class="sxs-lookup"><span data-stu-id="ed927-136">Network connectivity is possible between resources located in Azure, between on-premises and Azure hosted resources, and to and from the internet and Azure.</span></span>

<span data-ttu-id="ed927-137">ネットワーク制御についてのクラウド ネイティブ ポリシーには、以下のような要件が含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="ed927-137">A cloud native policy for network controls may include requirements like the following:</span></span>

* <span data-ttu-id="ed927-138">クラウド ネイティブ ポリシーでは (Azure では技術的に可能であるものの) オンプレミス リソースへのハイブリッド接続が許可されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="ed927-138">Hybrid connections to on-premises resources (While technically possible in Azure), might not be allowed in a Cloud Native policy.</span></span> <span data-ttu-id="ed927-139">ハイブリッド接続の必要性が明らかであれば、より堅牢なエンタープライズ セキュリティ ポリシーのサンプルを参照する方が適切です。</span><span class="sxs-lookup"><span data-stu-id="ed927-139">Should a hybrid connection prove necessary, a more robust Enterprise Security Policy sample would be a more relevant reference.</span></span>
* <span data-ttu-id="ed927-140">ユーザーは仮想ネットワークとネットワーク セキュリティ グループを使用して、セキュリティで保護された接続を Azure に対して、また、Azure 内で確立できます。</span><span class="sxs-lookup"><span data-stu-id="ed927-140">Users can establish secure connections to and within Azure using virtual networks and network security groups.</span></span>
* <span data-ttu-id="ed927-141">ネイティブの Windows Azure Firewall は、ポートへのアクセスを制限することで、悪意のあるネットワーク トラフィックからホストを保護します。</span><span class="sxs-lookup"><span data-stu-id="ed927-141">Native Windows Azure Firewall protects hosts from malicious network traffic by limited port access.</span></span> <span data-ttu-id="ed927-142">このポリシーの良い例は、RDP - TCP/UDP のポート 3389 経由で直接 VM に向かうトラフィックをブロックする (または有効にしない) 要件です。</span><span class="sxs-lookup"><span data-stu-id="ed927-142">A good example of this policy would be the requirement to block (or not enable) traffic directly to a VM over RDP - TCP/UDP port 3389.</span></span>
* <span data-ttu-id="ed927-143">Web アプリケーション ファイアウォールや Azure DDoS Protection などのサービスは、アプリケーションを保護し、Azure で実行される仮想マシンの可用性を確保します。</span><span class="sxs-lookup"><span data-stu-id="ed927-143">Services like Web Application Firewall and Azure DDoS Protection safeguard applications and ensure availability for virtual machines running in Azure.</span></span> <span data-ttu-id="ed927-144">これらの機能は、無効にしたり、誤用したりしないでください。</span><span class="sxs-lookup"><span data-stu-id="ed927-144">These features should not be disabled or misused.</span></span>

### <a name="data-protection"></a><span data-ttu-id="ed927-145">データ保護</span><span class="sxs-lookup"><span data-stu-id="ed927-145">Data protection</span></span>

<span data-ttu-id="ed927-146">クラウドでのデータ保護で重要なことの 1 つは、データが発生する可能性があると考えられる状態を把握し、それらの各状態で、どのような制御を適用できるかを把握しておくことです。</span><span class="sxs-lookup"><span data-stu-id="ed927-146">One of the keys to data protection in the cloud is accounting for the possible states in which your data may occur, and what controls are available for each state.</span></span> <span data-ttu-id="ed927-147">目的が Azure のデータ セキュリティと暗号化のベスト プラクティスの場合、推奨事項は以下のデータの状態に焦点が当てられています。</span><span class="sxs-lookup"><span data-stu-id="ed927-147">For the purpose of Azure data security and encryption best practices, recommendations focus on the following data states:</span></span>

* <span data-ttu-id="ed927-148">データの暗号化制御は、仮想マシンからストレージおよび SQL Database へのサービスに組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="ed927-148">Data encryption controls are built into services from virtual machines to storage and SQL Database.</span></span>
* <span data-ttu-id="ed927-149">クラウドと顧客の間でデータが移動するときには、業界標準の暗号化プロトコルを使用してデータを保護できます。</span><span class="sxs-lookup"><span data-stu-id="ed927-149">As data moves between clouds and customers, it can be protected using industry-standard encryption protocols.</span></span>
* <span data-ttu-id="ed927-150">ユーザーは、Azure Key Vault の利用によって、クラウド アプリやサービスが使用する暗号化キーなどのシークレットを保護および管理することができます。</span><span class="sxs-lookup"><span data-stu-id="ed927-150">Azure Key Vault enables users to safeguard and control cryptographic keys and other secrets used by cloud apps and services.</span></span>
* <span data-ttu-id="ed927-151">Azure Information Protection は、アプリ内の機密データを分類、ラベル付け、および保護するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="ed927-151">Azure Information Protection will help classify, label, and protect your sensitive data in apps.</span></span>

<span data-ttu-id="ed927-152">これらの機能は Azure に組み込まれていますが、上記の各機能を利用するには構成が必要で、コストが増える可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="ed927-152">While these features are built into Azure, each of the above requires configuration and could increase costs.</span></span> <span data-ttu-id="ed927-153">それぞれのクラウド ネイティブ機能は、[データ分類戦略](../policy-compliance/what-is-data-classification.md)と整合させておくことを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed927-153">Alignment of each Cloud Native feature with a [data classification strategy](../policy-compliance/what-is-data-classification.md) is highly suggested.</span></span>

### <a name="security-monitoring"></a><span data-ttu-id="ed927-154">セキュリティの監視</span><span class="sxs-lookup"><span data-stu-id="ed927-154">Security monitoring</span></span>

<span data-ttu-id="ed927-155">セキュリティの監視は、組織の標準やベスト プラクティスを満たしていないシステムを特定するためにリソースを監査する、事前対応型の戦略です。</span><span class="sxs-lookup"><span data-stu-id="ed927-155">Security monitoring is a proactive strategy that audits your resources to identify systems that do not meet organizational standards or best practices.</span></span> <span data-ttu-id="ed927-156">Azure Security Center は、ハイブリッド クラウド ワークロードの全体にわたり、統合されたセキュリティ ベースラインと高度な脅威保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="ed927-156">Azure Security Center provides unified Security Baseline and advanced threat protection across hybrid cloud workloads.</span></span> <span data-ttu-id="ed927-157">Security Center で、ワークロード全体にセキュリティ ポリシーを適用し、脅威への露出を減らし、攻撃の検出と対応を行うことができます。可能なことには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ed927-157">With Security Center, you can apply security policies across your workloads, limit your exposure to threats, and detect and respond to attacks, including:</span></span>

* <span data-ttu-id="ed927-158">オンプレミスとクラウドのすべてのワークロードにわたる、Azure Security Center によるセキュリティの統合表示</span><span class="sxs-lookup"><span data-stu-id="ed927-158">Unified view of security across all on-premises and cloud workloads with Azure Security Center</span></span>
* <span data-ttu-id="ed927-159">継続的な監視とセキュリティ評価によるコンプライアンスの確保とすべての脆弱性の修復</span><span class="sxs-lookup"><span data-stu-id="ed927-159">Continuous monitoring and security assessments to ensure compliance and remediate any vulnerabilities</span></span>
* <span data-ttu-id="ed927-160">対話型のツールと状況に応じた脅威インテリジェンスによる調査の効率化</span><span class="sxs-lookup"><span data-stu-id="ed927-160">Interactive tools and contextual threat intelligence for streamlined investigation</span></span>
* <span data-ttu-id="ed927-161">広範囲にわたるログ記録と、既存のセキュリティ情報との統合</span><span class="sxs-lookup"><span data-stu-id="ed927-161">Extensive logging and integration with existing security information</span></span>

### <a name="extending-cloud-native-policies"></a><span data-ttu-id="ed927-162">クラウド ネイティブのポリシーの拡張</span><span class="sxs-lookup"><span data-stu-id="ed927-162">Extending Cloud Native policies</span></span>

<span data-ttu-id="ed927-163">クラウドを使用すると、セキュリティ負担の一部を軽減できます。</span><span class="sxs-lookup"><span data-stu-id="ed927-163">Using the cloud can reduce some of the security burden.</span></span> <span data-ttu-id="ed927-164">Microsoft は Azure のデータ センターに物理的なセキュリティを提供し、DDoS 攻撃などのインフラストラクチャの脅威からクラウド プラットフォームを保護することに貢献しています。</span><span class="sxs-lookup"><span data-stu-id="ed927-164">Microsoft provides physical security for Azure datacenters and helps protect the cloud platform against infrastructure threats such as a DDoS attack.</span></span> <span data-ttu-id="ed927-165">Microsoft では何千人ものサイバー セキュリティ専門家が毎日セキュリティ業務に取り組んでいるため、サイバー攻撃を軽減するためのリソースは膨大です。</span><span class="sxs-lookup"><span data-stu-id="ed927-165">Given that Microsoft has thousands of cybersecurity specialists working on security every day, the resources to mitigate cyberattacks are considerable.</span></span> <span data-ttu-id="ed927-166">実際のところ、組織はかつて、クラウドがセキュリティで保護されているかどうかを心配していましたが、Microsoft などのベンダーが人材と特殊なインフラストラクチャに投資しているレベルを考慮すると、クラウドは実際には、ほとんどのオンプレミス データ センターよりも安全であると、多くの組織に理解されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="ed927-166">In fact, while organizations were once worried about whether the cloud was secure, most now understand that given the level of investment that vendors such as Microsoft make in people and specialized infrastructure, the cloud is actually more secure than most on-premises datacenters.</span></span>

<span data-ttu-id="ed927-167">このようにクラウド ネイティブ セキュリティ ベースラインへの投資が行われてるとは言え、任意のセキュリティ ベースライン ポリシーで、既定のクラウド ネイティブ ポリシーを拡張することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed927-167">Even with this investment in Cloud Native Security Baseline, it is suggested that any Security Baseline policy extend the default Cloud Native policies.</span></span> <span data-ttu-id="ed927-168">以下に、クラウド ネイティブ環境であっても検討する必要のある拡張ポリシーの例を示します。</span><span class="sxs-lookup"><span data-stu-id="ed927-168">The following are examples of extended policies that should be considered, even in a Cloud Native environment:</span></span>

* <span data-ttu-id="ed927-169">**VM をセキュリティで保護します**。</span><span class="sxs-lookup"><span data-stu-id="ed927-169">**Secure VMs**.</span></span> <span data-ttu-id="ed927-170">セキュリティは、どの組織でも最優先事項である必要があり、それを効果的に実施するには、いくつかのことが必要です。</span><span class="sxs-lookup"><span data-stu-id="ed927-170">Security should be every organization's top priority, and doing it effectively requires several things.</span></span> <span data-ttu-id="ed927-171">セキュリティの状態を評価し、セキュリティ上の脅威からの保護を提供してから、発生している脅威の迅速な検出と対応を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed927-171">You must assess your security state, protect against security threats, and then detect and respond rapidly to threats that occur.</span></span>
* <span data-ttu-id="ed927-172">**VM の内容を保護します**。</span><span class="sxs-lookup"><span data-stu-id="ed927-172">**Protect VM contents**.</span></span> <span data-ttu-id="ed927-173">定期的な自動バックアップの設定は、ユーザー エラーからの保護のために欠かせません。</span><span class="sxs-lookup"><span data-stu-id="ed927-173">Setting up regular automated backups is essential to protect against user errors.</span></span> <span data-ttu-id="ed927-174">ただしこれは十分ではありません。バックアップがサイバー攻撃から保護されていて、必要なときにそれを利用できるようにしておくことも必要です。</span><span class="sxs-lookup"><span data-stu-id="ed927-174">This isn’t enough, though; you must also make sure that your backups are safe from cyberattacks and are available when you need them.</span></span>
* <span data-ttu-id="ed927-175">**VM とアプリケーションを監視します**。</span><span class="sxs-lookup"><span data-stu-id="ed927-175">**Monitor VMs and applications**.</span></span> <span data-ttu-id="ed927-176">このパターンには、VM の正常性に関する分析情報の取得、それらの間の相互作用の理解、これらの VM で実行されるアプリケーションを監視する方法の確立など、いくつかのタスクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ed927-176">This pattern encompasses several tasks, including getting insight into the health of your VMs, understanding interactions among them, and establishing ways to monitor the applications these VMs run.</span></span> <span data-ttu-id="ed927-177">これらのタスクはすべて、アプリケーションの運用を 24 時間体制で続けるうえで不可欠です。</span><span class="sxs-lookup"><span data-stu-id="ed927-177">All of these tasks are essential in keeping your applications running around the clock.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed927-178">次の手順</span><span class="sxs-lookup"><span data-stu-id="ed927-178">Next steps</span></span>

<span data-ttu-id="ed927-179">ここでクラウド ネイティブ ソリューションのセキュリティ ベースライン ポリシーのサンプルを確認したので、[ポリシー レビュー ガイド](../policy-compliance/what-is-a-cloud-policy-review.md)に戻り、このサンプルに基づいてクラウド導入用の独自のポリシーの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="ed927-179">Now that you've reviewed the sample Security Baseline policy for Cloud Native solutions, return to the [policy review guide](../policy-compliance/what-is-a-cloud-policy-review.md) to start building on this sample to create your own policies for cloud adoption.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed927-180">ポリシー レビュー ガイドを使用して独自のポリシーを作成する</span><span class="sxs-lookup"><span data-stu-id="ed927-180">Build your own policies using the Policy Review Guide</span></span>](../policy-compliance/what-is-a-cloud-policy-review.md)