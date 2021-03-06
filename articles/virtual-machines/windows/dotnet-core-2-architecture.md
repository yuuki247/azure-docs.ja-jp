---
title: "Azure Resource Manager テンプレートを使った Windows コンピューティング リソースのデプロイ | Microsoft Docs"
description: "Azure Virtual Machines DotNet Core チュートリアル"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b026fe81-1bc1-4899-ac32-886091671498
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 6f4b947d767deea3b1a7cb870f659d718fa8ab10
ms.contentlocale: ja-jp
ms.lasthandoff: 03/31/2017


---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a>Windows VM での Azure Resource Manager テンプレートを使用したアプリケーション アーキテクチャ

Azure Resource Manager デプロイを開発する場合は、コンピューティング要件を Azure リソースとサービスにマップする必要があります。 アプリケーションが複数の http エンドポイント、1 つのデータベース、1 つのデータ キャッシュ サービスで構成されている場合は、これらの各コンポーネントをホストする Azure リソースを合理化する必要があります。 たとえば、サンプルのミュージック ストア アプリケーションには、仮想マシンでホストされる Web アプリケーションと Azure SQL Database でホストされる SQL データベースが含まれています。 

このドキュメントでは、サンプルの Azure Resource Manager テンプレートでミュージック ストアのコンピューティング リソースがどのように構成されているかについて説明します。 すべての依存関係と固有の構成に焦点を当てます。 最善の結果を得るために、ソリューションのインスタンスを Azure サブスクリプションに事前にデプロイし、Azure Resource Manager テンプレートを手元に用意して取り組んでください。 完全なテンプレートは、こちら ([Windows のミュージック ストア デプロイ](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)) にあります。

## <a name="virtual-machine"></a>仮想マシン
ミュージック ストア アプリケーションには、ユーザーが音楽を閲覧して購入できる Web アプリケーションが含まれています。 Web アプリケーションをホストできる Azure サービスは複数あります。この例では、Virtual Machines を使います。 サンプルのミュージック ストア テンプレートを使うと、仮想マシンがデプロイされ、Web サーバーがインストールされて、ミュージック ストア Web サイトがインストールおよび構成されます。 この記事では、仮想マシンのデプロイについてのみ説明します。 Web サーバーとアプリケーションの構成については、後の記事で説明します。

仮想マシンをテンプレートに追加するには、Visual Studio の新しいリソースの追加ウィザードを使うか、有効な JSON をデプロイ テンプレートに挿入します。 仮想マシンをデプロイするには、いくつかの関連リソースも必要です。 Visual Studio を使ってテンプレートを作成すると、これらのリソースは自動的に作成されます。 テンプレートを手動で作成する場合は、これらのリソースを挿入して構成する必要があります。

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [仮想マシンの JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285)) をご覧ください。

```json
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

デプロイすると、Azure Portal で仮想マシンのプロパティを確認できます。

![仮想マシン](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a>ストレージ アカウント
ストレージ アカウントには、さまざまなストレージ オプションと機能があります。 Azure Virtual Machines に関しては、ストレージ アカウントは仮想マシンの仮想ハード ドライブと追加のデータ ディスクを保持します。 ミュージック ストア サンプルには、デプロイ内の各仮想マシンの仮想ハード ドライブを保持するストレージ アカウントが 1 つ含まれています。 

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [ストレージ アカウント](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98)) をご覧ください。

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

ストレージ アカウントは、Resource Manager テンプレートの仮想マシンの宣言内で仮想マシンと関連付けられます。 

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [仮想マシンとストレージ アカウントの関連付け](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321)) をご覧ください。

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

デプロイすると、Azure Portal でストレージ アカウントを確認できます。

![ストレージ アカウント](./media/dotnet-core-2-architecture/storacct-win.png)

ストレージ アカウントの BLOB コンテナーをクリックすると、テンプレートを使ってデプロイされた各仮想マシンの仮想ハード ドライブ ファイルを確認できます。

![Virtual Hard Drives](./media/dotnet-core-2-architecture/vhd-win.png)

Azure Storage について詳しくは、 [Azure Storage のドキュメント](https://azure.microsoft.com/documentation/services/storage/)に関するページをご覧ください。

## <a name="virtual-network"></a>仮想ネットワーク
仮想マシンに、他の仮想マシンや Azure リソースと通信する機能など、内部ネットワークが必要な場合は、Azure Virtual Network が必要です。  仮想ネットワークによって、仮想マシンがインターネット経由でアクセスできるようになることはありません。 パブリック接続には、パブリック IP アドレスが必要です。これについては、このシリーズの後半で説明します。

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [仮想ネットワークとサブネット](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126)) をご覧ください。

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Azure Portal では、仮想ネットワークは次の図のように表示されます。 テンプレートを使ってデプロイしたすべての仮想マシンが仮想ネットワークに接続されていることに注意してください。

![仮想ネットワーク](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a>ネットワーク インターフェイス
 ネットワーク インターフェイスは、仮想マシンを仮想ネットワーク (具体的には、仮想ネットワーク内に定義されているサブネット) に接続します。 

 Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [ネットワーク インターフェイス](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156)) をご覧ください。

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

各仮想マシン リソースには、ネットワーク プロファイルが含まれています。 このプロファイルでは、ネットワーク インターフェイスは仮想マシンに関連付けられています。  

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ( [仮想マシンのネットワーク プロファイル](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330)) をご覧ください。

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Azure Portal では、ネットワーク インターフェイスは次の図のように表示されます。 内部 IP アドレスと仮想マシンの関連付けは、ネットワーク インターフェイス リソースで確認できます。

![ネットワーク インターフェイス](./media/dotnet-core-2-architecture/nic-win.png)

Azure Virtual Network について詳しくは、 [Azure Virtual Network のドキュメント](https://azure.microsoft.com/documentation/services/virtual-network/)に関するページをご覧ください。

## <a name="azure-sql-database"></a>Azure SQL Database
ミュージック ストア Web サイトをホストする仮想マシンのほかに、Azure SQL データベースもミュージック ストア データベースをホストするためにデプロイされます。 ここで Azure SQL Database を使う利点は、仮想マシンの 2 番目のセットが不要で、サービスにスケールと可用性が組み込まれていることです。

Azure SQL データベースを追加するには、Visual Studio の新しいリソースの追加ウィザードを使うか、有効な JSON をテンプレートに挿入します。 SQL Server リソースには、SQL インスタンスの管理者権限が付与されているユーザーの名前とパスワードが含まれています。 また、SQL ファイアウォール リソースが追加されています。 既定では、Azure でホストされるアプリケーションは SQL インスタンスに接続できます。 SQL Server Management Studio などの外部アプリケーションが SQL インスタンスに接続できるようにするには、ファイアウォールを構成する必要があります。 ミュージック ストア デモでは、既定の構成で問題ありません。 

Resource Manager テンプレート内の JSON サンプルを確認するには、こちらのリンク ([Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379)) をご覧ください。

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Azure Portal に表示された SQL Server と MusicStore データベース。

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

Azure SQL Database のデプロイについて詳しくは、 [Azure SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)に関するページをご覧ください。

## <a name="next-step"></a>次のステップ
<hr>

[手順 2 - Azure Resource Manager テンプレートでのアクセスとセキュリティ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


