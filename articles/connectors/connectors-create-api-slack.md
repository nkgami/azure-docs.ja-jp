---
title: Azure Logic Apps から Slack に接続する | Microsoft Docs
description: Azure Logic Apps を使用して、Slack アカウントのファイルの監視、チャネル、グループ、およびメッセージの管理を行うタスクとワークフローを自動化します
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 7af2db528866d687064e854e00e43e81d2601b2b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042327"
---
# <a name="monitor-and-manage-slack-with-azure-logic-apps"></a>Azure Logic Apps を使用して Slack の監視と管理を行う

Azure Logic Apps と Slack コネクターを使用して、Slack ファイルの管理と Slack チャネル、メッセージ、グループの管理を行う自動化されたタスクとワークフローを作成できます。他にも、次のようなアクションを実行できます。

* 新しいファイルの作成を監視します。
* チャネルの作成、一覧表示、および結合を行います。 
* メッセージを投稿します。
* グループの作成と、応答不可の設定を行います。

Slack アカウントから応答を取得し、その出力を他のアクションが使用できるようにするトリガーを使用できます。 Slack アカウントでタスクを実行するアクションを使用できます。 他のアクションに Slack アクションからの出力を使用させることもできます。 たとえば、新しいファイルが作成されたときに、Office 365 Outlook コネクタを使用して電子メールを送信できます。 ロジック アプリを初めて使用する場合は、「[Azure Logic Apps とは](../logic-apps/logic-apps-overview.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。 Azure サブスクリプションがない場合は、<a href="https://azure.microsoft.com/free/" target="_blank">無料の Azure アカウントにサインアップ</a>してください。 

* [Slack](https://slack.com/) アカウントとユーザー資格情報。

  接続を作成してお使いの Slack アカウントにアクセスしてよいという承認が、この資格情報によってロジック アプリに与えられます。

* [ロジック アプリの作成方法](../logic-apps/quickstart-create-first-logic-app-workflow.md)に関する基本的な知識

* Slack アカウントにアクセスするロジック アプリ。 Slack トリガーで開始するには、[空のロジック アプリを作成](../logic-apps/quickstart-create-first-logic-app-workflow.md)します。 Slack アクションを使用するには、Slack トリガーや **繰り返し**トリガーなどの別のトリガーでロジック アプリを起動します。

## <a name="connect-to-slack"></a>Slack に接続する

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com) にサインインし、ロジック アプリ デザイナーでロジック アプリを開きます (まだ開いていない場合)。

1. 空のロジック アプリの場合: 検索ボックスに、フィルターとして「Slack」と入力します。 トリガーの一覧で、目的のトリガーを選択します。 

   または

   既存のロジック アプリの場合: アクションを追加する最後のステップの下で、**[新しいステップ]** を選択します。 
   検索ボックスに、フィルターとして「Slack」と入力します。 
   アクションの一覧で、目的のアクションを選択します。

   ステップの間にアクションを追加するには、ステップ間の矢印の上にポインターを移動します。 
   表示されるプラス記号 (**+**) を選択し、**[アクションの追加]**  を選択します。

1. Slack へのサインインを求められたら、Slack ワークスペースにサインインします。 

   ![Slack ワークスペースにサインイン](./media/connectors-create-api-slack/slack-sign-in-workspace.png)

1. ロジック アプリへのアクセスを承認します。

   ![Slack へのアクセスを承認](./media/connectors-create-api-slack/slack-authorize-access.png)

1. 選択したトリガーまたはアクションで必要な詳細を指定します。 ロジック アプリのワークフローの構築を続行する場合は、さらにアクションを追加します。

## <a name="connector-reference"></a>コネクタのレファレンス

コネクタの OpenAPI (以前の Swagger) の説明に記載されているトリガー、アクション、および制限に関する技術的な詳細については、コネクタの[リファレンス ページ](/connectors/slack/)を参照してください。

## <a name="get-support"></a>サポートを受ける

* 質問がある場合は、[Azure Logic Apps フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)にアクセスしてください。
* 機能のアイデアについて投稿や投票を行うには、[Logic Apps のユーザー フィードバック サイト](http://aka.ms/logicapps-wish)にアクセスしてください。

## <a name="next-steps"></a>次の手順

* 他の[Logic Apps コネクタ](../connectors/apis-list.md)を確認します。
