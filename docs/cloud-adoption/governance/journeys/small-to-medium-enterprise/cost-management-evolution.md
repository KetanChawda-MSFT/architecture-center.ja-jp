---
title: 'CAF: 中小企業 – コスト管理の進化  '
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 中小企業の拡大 – コスト管理の進化
author: BrianBlanchard
ms.openlocfilehash: ca070bfca3efbf9548faa8cf28a1adffefd4a33a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901286"
---
# <a name="small-to-medium-enterprise-cost-management-evolution"></a>中小企業: コスト管理の進化

この記事では、ガバナンス MVP にコスト制御を追加することで物語を展開します。

## <a name="evolution-of-the-narrative"></a>物語の展開

導入は、ガバナンス MVP で定義されているコストの許容範囲指標を以上に拡大しました。 これは "ディザスター リカバリー" 用データ センターからの移行に対応しているため、良いことです。 今回は支出の増加が、クラウド ガバナンス チームが時間をかける理由となっています。

### <a name="evolution-of-the-current-state"></a>現在の状態の展開

この物語の前のフェーズでは、IT 部門がディザスター リカバリー用データ センターの 100% を廃止しました。 アプリケーション開発チームと BI チームは、運用トラフィックに対する準備を終えていました。

その後、ガバナンスに影響を与える以下のような変更がありました。

- 移行チームが、運用環境のデータ センターから VM の移行を開始しました。
- アプリケーション開発チームが CI/CD パイプライン経由で運用環境のアプリケーションをクラウドに積極的にプッシュしてます。 それらのアプリケーションは、ユーザーの要求によって事後対的にスケールできます。
- IT 部門内のビジネス インテリジェンス チームは、クラウドに多数の予測分析ツールを提供してきました。 クラウドで集計されるデータの量は増加し続けています。
- こうした成長のすべてが、約束されたビジネス上の成果を支えています。 ただし、コストが急増し始めました。 推定経費は予想より早く増加しています。 CFO は、コストを管理するための改善されたアプローチを必要としています。

### <a name="evolution-of-the-future-state"></a>将来の状態の展開

コストの監視およびレポートが、クラウド ソリューションに追加されます。 現在も IT 部門がコスト情報センターとしての役割を果たしています。 これは、クラウド サービスの支払いが、引き続き IT 調達に分類されることを意味します。 しかしレポートでは、クラウド コストを費やしている機能に直接運用経費を結び付ける必要があります。 このモデルは、クラウド会計に対する "真相暴露" モデルと呼ばれています。

現在や将来の状態が変わると、新しいポリシー ステートメントを必要とする新しいリスクにさらされることになります。

## <a name="evolution-of-tangible-risks"></a>具体的なリスクの展開

**予算管理**: セルフ サービス機能には、新しいプラットフォームでは過度の予期しないコストが生じるリスクがつきものです。 コストを監視し、継続的なコスト リスクを軽減するためのガバナンス プロセスを実施し、計画した予算との継続的な整合を確保する必要があります。

このビジネス リスクは、いくつかの技術的リスクへと拡大する可能性があります。

- 実際のコストが計画を超過する可能性があります。
- ビジネス条件は変化します。 そうしたときには、ビジネス機能が予想より多くのクラウド サービスを消費する必要がある場合が存在し、それが異常な支出につながります。 こうした余分な支出は、計画に必要な調整ではなく、古くて役に立たないものと見なされるリスクがあります。
- システムがオーバープロビジョニングされて、余分なコストの支出となる可能性があります。

## <a name="evolution-of-the-policy-statements"></a>ポリシー ステートメントの展開

ポリシーに対する以下の変更は、新しいリスクを軽減して実施を導く助けとなります。

1. すべてのクラウド コストは、クラウド ガバナンス チームが毎週、計画を基準にして監視する必要があります。 クラウド コストと計画の間の差異に関するレポートは、毎月、IT リーダーと財務部門で共有する必要があります。 クラウド コストと計画のすべての更新は、毎月、IT リーダーと財務部門で確認する必要があります。
2. すべてのコストは、責任の所在を明らかにするため、特定の職務に割り当てる必要があります。
3. クラウド資産は、最適化の機会を探して継続的に監視する必要があります。
4. クラウド ガバナンス ツールでは、資産のサイズ変更オプションを、承認済みの構成リストに限定する必要があります。 このツールにより、すべての資産がコスト監視ソリューションによって発見可能で、追跡されるようにする必要があります。
5. デプロイの計画時には、運用ワークロードのホスティングに関連付けられている必須のクラウド リソースがあれば、それを文書化する必要があります。 このドキュメントは、予算の精度を高め、より高価なオプションの使用を防ぐ追加の自動化を準備するのに役立ちます。 このプロセス中には、予約インスタンスやライセンス コストの削減など、クラウド プロバイダーが提供するさまざまな割引ツールについて検討する必要があります。
6. すべてのアプリケーション所有者は、クラウド コストをより適切に管理するために、ワークロードの最適化実習のトレーニングに参加する必要があります。

## <a name="evolution-of-the-best-practices"></a>ベスト プラクティスの展開

記事のこのセクションでは、ガバナンス MVP の設計を進化させて、新しい Azure ポリシーと Azure Cost Management の実装を含めます。 これら 2 つの設計変更が一体となって、新しい企業ポリシー ステートメントが満たされます。

1. Azure Cost Management を実装します
    1. サブスクリプション パターンとリソース整合性の規範に合致する適切なアクセス スコープを確立します。 以前の記事で定義したガバナンス MVP と整合させることを前提にすると、このためには、高レベルのレポート作成を実行するクラウド ガバナンスチームのために、**登録アカウントのスコープ**へのアクセスが必要です。 ガバナンスの外部にあるその他のチームには、**リソース グループのスコープ**へのアクセスが必要な場合があります。
    2. Azure Cost Management で予算を編成します。
    3. 初期の推奨事項を確認して対応します。 レポート作成をサポートするための繰り返しのプロセスを用意します。
    4. 初期用と繰り返し用の両方について、Azure Cost Management のレポートを構成して実行します。
2. Azure Policy を更新します
    1. タグ付け、管理グループ、サブスクリプション、およびリソース グループの値を監査して、差異がないか特定します。
    2. SKU サイズ オプションを確定して、デプロイ計画ドキュメントに記載されている SKU にデプロイを制限します。

## <a name="conclusion"></a>まとめ

ガバナンス MVP にこれらのプロセスと変更を加えることで、コスト ガバナンスに関連するリスクの多くを軽減する助けとなります。 同時に、コスト管理に必要な可視性、説明責任、および最適化が実現します。

## <a name="next-steps"></a>次の手順

クラウドの導入が進み続けてより高いビジネス価値が提供されるのにつれて、リスクとクラウド ガバナンスのニーズも高度化します。 これを体験している架空の企業にとっての次の手順は、このガバナンス投資を利用して複数のクラウドを管理することです。

> [!div class="nextstepaction"]
> [マルチクラウドの展開](./multi-cloud-evolution.md)
