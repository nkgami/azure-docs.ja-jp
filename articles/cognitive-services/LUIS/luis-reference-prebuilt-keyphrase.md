---
title: LUIS 作成済みエンティティ keyPhrase リファレンス - Azure | Microsoft Docs
titleSuffix: Azure
description: この記事には、Language Understanding (LUIS) での keyPhrase 作成済みエンティティについての情報が含まれます。
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/09/2018
ms.author: diberry
ms.openlocfilehash: 4133b7c7c3fabbe92a3208c567d7b4c6c2c27283
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434400"
---
# <a name="keyphrase-entity"></a>keyPhrase エンティティ
keyPhrase は、発話からさまざまなキー フレーズを抽出します。 keyPhrase が含まれている発話例をアプリケーションに追加する必要はありません。 keyPhrase エンティティは、[テキスト分析](../text-analytics/overview.md)フィーチャーの一環として[多くのカルチャ](luis-language-support.md#languages-supported)でサポートされています。 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>作成済み keyPhrase エンティティの解決
次の例では、**builtin.keyPhrase** エンティティの解決を示します。

```JSON
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>次の手順

[percentage](luis-reference-prebuilt-percentage.md)、[number](luis-reference-prebuilt-number.md)、[age](luis-reference-prebuilt-age.md) エンティティについて学習します。
