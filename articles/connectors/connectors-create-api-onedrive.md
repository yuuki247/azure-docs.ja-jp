---
title: "ロジック アプリに OneDrive コネクタを追加する | Microsoft Docs"
description: "OneDrive コネクタと REST API パラメーターの概要"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 63bd33bf4e09b98aa53dcfec9fcc4a0109204952
ms.contentlocale: ja-jp
ms.lasthandoff: 05/26/2017


---
# <a name="get-started-with-the-onedrive-connector"></a>OneDrive コネクタの使用
OneDrive に接続して、ファイルのアップロード、取得、削除など、ファイルを管理します。 

OneDrive では、次の操作を実行できます。 

* OneDrive にファイルを保存してワークフローを構築するか、OneDrive 内の既存ファイルを更新できます。 
* OneDrive 内でファイルが作成または更新されたときに、トリガーを使用してワークフローを開始できます。
* ファイルの作成、ファイルの削除などのアクションを使用できます。 たとえば、添付ファイル付きの新しい Office 365 電子メールを受信したときに (トリガー)、OneDrive に新しいファイルを作成します (アクション)。

このトピックでは、ロジック アプリ内で OneDrive コネクタを使用する方法を説明し、トリガーとアクションの一覧を示します。

Logic Apps の詳細については、「[Logic Apps とは](../logic-apps/logic-apps-what-are-logic-apps.md)」と[ロジック アプリの作成](../logic-apps/logic-apps-create-a-logic-app.md)に関するページを参照してください。

## <a name="connect-to-onedrive"></a>OneDrive に接続する
ロジック アプリから任意のサービスにアクセスできるようにするには、まず、そのサービスへの "*接続*" を作成します。 接続により、ロジック アプリと別のサービスとの接続が実現します。 たとえば、OneDrive に接続するには、最初に OneDrive "*接続*" が必要です。 接続を作成するには、接続対象のサービスへのアクセスに通常使用する資格情報を入力します。 そのため、OneDrive の場合は、OneDrive アカウントの資格情報を入力して接続を作成します。

### <a name="create-the-connection"></a>接続の作成
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>トリガーを使用する
トリガーとは、ロジック アプリで定義されたワークフローの開始に使用できるイベントです。 トリガーは、指定された間隔と頻度でサービスを "ポーリング" します。 トリガーの詳細については[こちら](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts)を参照してください。

1. ロジック アプリで「onedrive」と入力して、トリガーの一覧を取得します。  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. **[When a file is modified (ファイルの変更時)]** を選択します。 接続が既に存在する場合は、[ピッカーの表示] ボタンを選択してフォルダーを選択します。
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    サインインを求められたら、サインインの詳細を入力して接続を作成します。 この手順については、このトピックの「[接続の作成](connectors-create-api-onedrive.md#create-the-connection)」を参照してください。 
   
   > [!NOTE]
   > この例では、選択したフォルダー内のファイルが更新されたときに、ロジック アプリが実行されます。 このトリガーの結果を確認するには、自身に電子メールを送信する別のアクションを追加してください。 たとえば、Office 365 Outlook の "*電子メールを送信する*" アクションを追加します。これにより、ファイルが更新されると電子メールが送信されます。 

3. **[編集]** を選択し、**[頻度]** と **[間隔]** の値を設定します。 たとえば、トリガーを使用して 15 分ごとにポーリングを実行するには、**[頻度]** を **[分]** に設定し、**[間隔]** を **15** に設定します。 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。 ロジック アプリが保存され、場合によっては、自動的に有効になります。

## <a name="use-an-action"></a>アクションを使用する
アクションとは、ロジック アプリで定義されたワークフローによって実行される操作です。 アクションの詳細については[こちら](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts)を参照してください。

1. プラス記号を選択します。 **[アクションの追加]**、**[条件の追加]**、**[More (その他)]** のいずれかのオプションという複数の選択肢があります。
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. **[アクションの追加]**を選択します。
3. テキスト ボックスに「onedrive」と入力して、使用可能なすべてのアクションの一覧を取得します。
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. この例では、**[OneDrive - Create file (OneDrive - ファイルを作成する)]** を選択します。 接続が既に存在する場合は、**[フォルダー パス]** で、ファイルを配置する場所を選択し、**[ファイル名]** に入力して、**目的のファイルのコンテンツ**を選択します。  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    接続情報の入力を求められたら、詳細を入力して接続を作成します。 これらのプロパティについては、このトピックの「[接続の作成](connectors-create-api-onedrive.md#create-the-connection)」を参照してください。 
   
   > [!NOTE]
   > この例では、OneDrive フォルダーに新しいファイルを作成します。 別のトリガーからの出力を使用して、OneDrive ファイルを作成できます。 たとえば、Office 365 Outlook の "*When a new email arrives (新しい電子メールが届いたとき)*" トリガーを追加します。 次に、OneDrive の "*Create file (ファイルを作成する)*" アクションを追加します。このアクションは、ForEach 内の Attachments フィールドと Content-Type フィールドを使用して OneDrive に新しいファイルを作成します。 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。 ロジック アプリが保存され、場合によっては、自動的に有効になります。


## <a name="connector-specific-details"></a>コネクタ固有の詳細

[コネクタの詳細](/connectors/onedriveconnector/)に関するページに、Swagger で定義されているトリガーとアクション、さらに制限が記載されています。

## <a name="more-connectors"></a>その他のコネクタ
[API リスト](apis-list.md)に戻ります。
