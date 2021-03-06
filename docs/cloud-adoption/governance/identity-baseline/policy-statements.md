---
title: CAF:ID ベースラインのサンプル ポリシー ステートメント
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: ID ベースラインのサンプル ポリシー ステートメント
author: BrianBlanchard
ms.openlocfilehash: 5fad9265b9c048ee502c7e084ddd03faa0ad3e23
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902033"
---
# <a name="identity-baseline-sample-policy-statements"></a>ID ベースラインのサンプル ポリシー ステートメント

個々のクラウド ポリシー ステートメントは、リスクの評価プロセス時に識別された特定のリスクに対処するためのガイドラインとなります。 これらのステートメントでは、リスクとそれらのリスクに対処する計画の簡潔な概要を提供する必要があります。 各ステートメント定義には、以下の情報を含める必要があります。

- 技術的なリスク - このポリシーで対処されるリスクの概要。
- ポリシー ステートメント - ポリシー要件の端的な概要説明。
- 設計オプション - 実践的な推奨事項、仕様、または IT チームおよび開発者がポリシーの実装時に使用できるその他のガイダンス。

次のサンプル ポリシー ステートメントは、ID に関連する複数の一般的なビジネス リスクに対処し、組織のニーズに対応するポリシー ステートメントのドラフトを作成するときに参照する例として提供されます。 これらのサンプルは、規制的であることを意図しておらず、特定のリスクに対処するために複数のポリシー オプションが存在する可能性があります。 ビジネス チームおよび IT チームと密接に連携して、リスクの一意のセットに最適なポリシー ソリューションを識別します。

## <a name="lack-of-access-controls"></a>アクセス制御の欠如

**技術的なリスク**:不十分な、またはアドホックのアクセス制御設定により、機密リソースまたはミッション クリティカルなリソースへの未承認アクセスのリスクが生じる可能性があります。

**ポリシー ステートメント**:クラウドにデプロイされるすべての資産は、現在のガバナンス ポリシーによって承認された ID とロールを使用して制御する必要があります。

**使用可能な設計オプション**:[Azure Active Directory の条件付きアクセス](/azure/active-directory/conditional-access/overview)は、Azure の既定のアクセス制御メカニズムです。

## <a name="overprovisioned-access"></a>オーバープロビジョニングのアクセス

**技術的なリスク**:ユーザーおよびグループがその責任範囲を超えてリソースを制御した結果、無許可の変更が行われ、障害またはセキュリティの脆弱性につながる可能性があります。

**ポリシー ステートメント**:次のポリシーが実装されます:

- ミッション クリティカルなアプリケーションまたは保護対象データに関わるあらゆるリソースに、最小特権アクセス モデルが適用されます。
- アクセス許可の昇格は例外とするべきであり、クラウド ガバナンス チームはそのような例外をすべて記録する必要があります。 例外は定期的に監査されます。

**使用可能な設計オプション**:[知る必要性](https://wikipedia.org/wiki/Need_to_know)と[最小特権セキュリティ](https://wikipedia.org/wiki/Principle_of_least_privilege)の各原則に基づいてアクセスを制限するロールベースのアクセス制御 (RBAC) 戦略を、[Azure Identity Management のベスト プラクティス](/azure/security/azure-security-identity-management-best-practices)を参考にして実装します。

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a>オンプレミスとクラウドの間で共有される管理アカウントの欠如

**技術的なリスク**:オンプレミスの Active Directory にアカウントを持つ IT 管理スタッフが、クラウド リソースへの十分なアクセス権限を持たないことにより、運用またはセキュリティの問題を効率的に解決できない場合があります。

**ポリシー ステートメント**:オンプレミスの Active Directory インフラストラクチャ内にあり、昇格された特権を持つすべてのグループを、承認済みの RBAC ロールにマップする必要があります。

**使用可能な設計オプション**:クラウド ベースの Azure Active Directory とオンプレミスの Active Directory の間でハイブリッド ID ソリューションを実装し、必要なオンプレミス グループを、各自の作業を行うために必要な RBAC ロールに追加します。

## <a name="weak-authentication-mechanisms"></a>弱い認証メカニズム

**技術的なリスク**:基本的なユーザー/パスワードの組み合わせなど、安全性が不十分なユーザー認証方法を使用した ID 管理システムは、パスワードの侵害またはハッキングにつながる可能性があり、セキュリティで保護されたクラウド システムへの未承認アクセスの大きなリスクをもたらします。

**ポリシー ステートメント**:すべてのアカウントは、セキュリティで保護されたリソースにログインする際、多要素認証 (MFA) 方式を使用する必要があります。

**使用可能な設計オプション**:Azure Active Directory の場合、ユーザー承認プロセスの一部として [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks) を実装します。

## <a name="isolated-identity-providers"></a>孤立した ID プロバイダー

**技術的なリスク**:互換性のない ID プロバイダーにより、顧客またはその他のビジネス パートナーとリソースまたはサービスを共有できなくなる可能性があります。

**ポリシー ステートメント**:カスタマー認証が必要なアプリケーションをデプロイする場合、内部ユーザー用のプライマリ ID プロバイダーと互換性のある承認済み ID プロバイダーを使用する必要があります。

**使用可能な設計オプション**:社内の ID プロバイダーと顧客の ID プロバイダーの間に [Azure Active Directory とのフェデレーション](/azure/active-directory/hybrid/whatis-fed)を実装します。

## <a name="identity-reviews"></a>ID レビュー

**技術的なリスク**:時間の経過と共に、ビジネスの変化、新しいクラウド デプロイの追加、またはその他のセキュリティの懸念事項により、セキュリティで保護されたリソースへの未承認アクセスのリスクが高まる可能性があります。

**ポリシー ステートメント**:クラウド ガバナンスのプロセスには、クラウド資産の構成によって阻止する必要がある悪意のあるアクターや使用パターンを識別するために、ID 管理チームによる四半期ごとのレビューを含める必要があります。

**使用可能な設計オプション**:ガバナンスのチーム メンバーと、ID サービスの管理を担当する IT スタッフの両方を含む四半期ごとのセキュリティ レビュー会議を設定します。 既存のセキュリティ データとメトリックのレビューを行って、現在の ID 管理のポリシーとツールのギャップを確かめ、新しいリスクを軽減するようにポリシーを更新します。

## <a name="next-steps"></a>次の手順

手始めに、この記事で説明されているサンプルを使用して、クラウドの導入計画に合致する特定のビジネス リスクに対処するポリシーを作成します。

ID ベースラインに関連する独自のカスタム ポリシー ステートメントを作成するには、[ID ベースライン テンプレート](template.md)をダウンロードします。

この規範の導入を促進するには、ご使用の環境に最も合う[アクションにつながるガバナンス体験](../journeys/overview.md)を選択します。 その後、設計を変更して、特定の企業ポリシーの決定を組み込みます。

> [!div class="nextstepaction"]
> [アクションにつながるガバナンス体験](../journeys/overview.md)