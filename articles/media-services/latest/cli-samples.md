---
title: Azure CLI の例 - Azure Media Services | Microsoft Docs
description: Azure Media Services 用の Azure CLI の例
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 04/15/2018
ms.author: juliako
ms.openlocfilehash: acc92662aa5b727656a8eda368ba6d78a87d9ecd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640891"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure Media Services 用の Azure CLI の例

次の表に、Azure Media Services 用の Azure CLI の例へのリンクが含まれています。

|  |  |
|---|---|
|**アカウント**||
| [Media Services アカウントを作成する](./scripts/cli-create-account.md) | Azure Media Services アカウントを作成します。 プログラムでアカウントを管理する API にアクセスするために使用できるサービス プリンシパルも作成できます。 |
| [アカウントの資格情報をリセットする](./scripts/cli-reset-account-credentials.md)|アカウントの資格情報をリセットし、app.config settings 設定を元に戻します。|
|**資産**||
| [資産を作成する](./scripts/cli-create-asset.md)|コンテンツをアップロードする Media Services Asset を作成します。|
| [ファイルをアップロードする](./scripts/cli-upload-file-asset.md)|ローカル ファイルをストレージ コンテナーにアップロードします。|
| [資産を公開する](./scripts/cli-publish-asset.md)| ストリーミング ロケーターを作成し、ストリーミング URL を戻します。 |
| **トランスフォーム**と**ジョブ**||
| [トランスフォームを作成する](./scripts/cli-create-transform.md)|トランスフォームの作成方法を示します。 トランスフォームは、ビデオまたはオーディオ ファイルを処理するためのタスクの単純なワークフローを記述します (多くの場合、"レシピ" と呼ばれます)。<br/> 必要な名前と "レシピ" を持つトランスフォームが既に存在するかどうかを必ず確認する必要があります。 該当する場合は、それを再利用します。 |
| [ジョブを作成する](./scripts/cli-create-jobs.md)|HTTPS URL を使用する単純なエンコードされたトランスフォームにジョブを送信します。|
| [EventGrid を作成する](./scripts/cli-create-event-grid.md)|ジョブ状態変更用のアカウント レベルの Event Grid サブスクリプションを作成します。|

## <a name="see-also"></a>関連項目

[CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
