---
title: "PowerShell を使用してクラウド サービス コンテナーを作成する | Microsoft Docs"
description: "この記事では、PowerShell を使用してクラウド サービス コンテナーを作成する方法について説明します。 コンテナーは、Web ロールと worker ロールをホストします。"
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: c024d8d47ab9b78399d8eb8b4df1f764f2877339
ms.lasthandoff: 04/27/2017


---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Azure PowerShell コマンドを使用して空のクラウド サービス コンテナーを作成する
この記事では、Azure PowerShell コマンドレットを使用して Cloud Services コンテナーを簡単に作成する方法について説明します。 次の手順に従ってください。

1. [Azure PowerShell のダウンロード](http://aka.ms/webpi-azps) ページから Microsoft Azure PowerShell コマンドレットをインストールします。
2. PowerShell コマンド プロンプトを開きます。
3. [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) を使用してサインインします。

   > [!NOTE]
   > Azure PowerShell コマンドレットをインストールして、Azure サブスクリプションに接続する手順の詳細については、「 [Azure PowerShell をインストールして構成する方法](/powershell/azure/overview)」を参照してください。
   >
   >
4. **New-AzureService** コマンドレットを使用し、空の Azure クラウド サービス コンテナーを作成します。

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. この例に従って、コマンドレットを呼び出します。

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Azure クラウド サービスの作成方法については、次のコマンドレットを実行します。

```
Get-help New-AzureService
```

### <a name="next-steps"></a>次のステップ
* クラウド サービス デプロイの管理については、[Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx)、[Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)、[Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) の各コマンドの説明を参照してください。 詳細については、 [クラウド サービスを構成する方法](cloud-services-how-to-configure.md) に関するページも参考になります。
* Azure にクラウド サービス プロジェクトを発行する方法については、[Azure でのクラウド サービスの継続的な配信](cloud-services-dotnet-continuous-delivery.md)に関するページの **PublishCloudService.ps1** コード サンプルを参照してください。

