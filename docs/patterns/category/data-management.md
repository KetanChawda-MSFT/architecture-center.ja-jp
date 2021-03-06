---
title: データ管理のパターン
titleSuffix: Cloud Design Patterns
description: データ管理はクラウド アプリケーションの重要な要素であり、品質属性のほとんどに影響します。 通常、パフォーマンス、スケーラビリティ、または可用性の理由から、データは複数のサーバーにまたがってさまざまな場所でホストされます。これによって、広範な課題が生じることがあります。 たとえば、データの整合性を維持する必要があります。また、通常はさまざまな場所にあるデータを同期する必要があります。
keywords: 設計パターン
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 25571a431836656856ed3f299455dfdb94ae3477
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486983"
---
# <a name="data-management-patterns"></a>データ管理のパターン

[!INCLUDE [header](../../_includes/header.md)]

データ管理はクラウド アプリケーションの重要な要素であり、品質属性のほとんどに影響します。 通常、パフォーマンス、スケーラビリティ、または可用性の理由から、データは複数のサーバーにまたがってさまざまな場所でホストされます。これによって、広範な課題が生じることがあります。 たとえば、データの整合性を維持する必要があります。また、通常はさまざまな場所にあるデータを同期する必要があります。

|                        Pattern                         |                                                                  まとめ                                                                  |
|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|            [キャッシュ アサイド](../cache-aside.md)            |                                            オンデマンドでデータをデータ ストアからキャッシュに読み込みます                                             |
|                   [CQRS](../cqrs.md)                   |                    個別のインターフェイスを使用して、データを更新する操作から、データを読み取る操作を分離します。                     |
|         [イベント ソース](../event-sourcing.md)         |               追加専用のストアを使用して、ドメイン内のデータに実行されるアクションを記述する一連のイベントすべてを記録します。               |
|            [テーブルのインデックス作成](../index-table.md)            |                         クエリによって頻繁に参照されるデータ ストア内のフィールドにインデックスを作成します。                          |
|      [具体化されたビュー](../materialized-view.md)      | データの形式が必要なクエリ操作に適していない場合に、1 つ以上のデータ ストアのデータの事前設定されたビューを生成します。 |
|               [シャーディング](../sharding.md)               |                                    データ ストアを水平方向のパーティションまたはシャードのセットに分割します。                                     |
| [静的コンテンツ ホスティング](../static-content-hosting.md) |                   静的なコンテンツを、クライアントに直接配信できるクラウドベースのストレージ サービスにデプロイします。                    |
|              [バレット キー](../valet-key.md)              |                 特定のリソースまたはサービスへの限定的な直接アクセスをクライアントに提供する、トークンまたはキーを使用します。                 |
