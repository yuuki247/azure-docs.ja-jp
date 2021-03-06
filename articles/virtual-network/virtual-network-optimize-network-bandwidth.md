---
title: "VM のネットワーク スループットの最適化 | Microsoft Docs"
description: "Azure 仮想マシンのネットワーク スループットを最適化する方法について説明します。"
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2017
ms.author: steveesp
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 1340048d5d518caff3397f671d0c75caaab4b5ac
ms.contentlocale: ja-jp
ms.lasthandoff: 07/01/2017


---

# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Azure 仮想マシンのネットワーク スループットの最適化

Azure 仮想マシン (VM) には既定のネットワーク設定がありますが、ネットワーク スループットを向上させるために最適化できます。 この記事では、Microsoft Azure の Windows VM および Linux VM (Ubuntu、CentOS、Red Hat などの主要ディストリビューションを含む) のネットワーク スループットを最適化する方法について説明します。

## <a name="windows-vm"></a>Windows VM

Receive Side Scaling (RSS) を使用する VM は、RSS を使用しない VM よりも高い最大スループットを実現できます。 Windows VM では、RSS が既定で無効になっている場合があります。 次の手順を実行して、RSS が有効かどうかを確認し、無効になっている場合は有効にします。

1. `Get-NetAdapterRss` PowerShell コマンドを入力して、ネットワーク アダプターに対して RSS が有効になっているかどうかを確認します。 `Get-NetAdapterRss` からの次の出力例では、RSS は有効になっていません。

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. 次のコマンドを入力して、RSS を有効にします。

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    上記のコマンドの出力はありません。 このコマンドにより、NIC 設定が変更され、約 1 分間接続が一時的に切断されます。 接続の切断中は、再接続中を示すダイアログ ボックスが表示されます。 通常、3 回目の試行後に接続が復元されます。
3. 再度 `Get-NetAdapterRss` コマンドを入力して、VM で RSS が有効になっていることを確認します。 成功した場合は、次のような出力が返されます。

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux VM

Azure Linux VM では、RSS は既定で常に有効になっています。 2017 年 1 月以降にリリースされた Linux カーネルには、Linux VM がより高いネットワーク スループットを実現できる新しいネットワーク最適化オプションが含まれています。

### <a name="ubuntu"></a>Ubuntu

最適化を行うには、最初に、サポートされている最新バージョンに更新します。2017 年 6 月時点で次のバージョンが該当します。
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
更新が完了したら、次のコマンドを入力して、最新のカーネルを入手します。

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

省略可能なコマンド:

`apt-get -y dist-upgrade`

### <a name="centos"></a>CentOS

最適化を行うには、最初に、サポートされている最新バージョンに更新します。2017 年 5 月時点で次のバージョンが該当します。
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
更新が完了したら、最新の Linux Integration Services (LIS) をインストールします。
スループットの最適化は、LIS の 4.2 以降に含まれています。 次のコマンドを入力して、LIS をインストールします。

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

最適化を行うには、最初に、サポートされている最新バージョンに更新します。2017 年 1 月時点で次のバージョンが該当します。
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017062722"
```
更新が完了したら、最新の Linux Integration Services (LIS) をインストールします。
スループットの最適化は、LIS の 4.2 以降に含まれています。 次のコマンドを実行して、LIS をダウンロードしてインストールします。

```bash
mkdir lis4.2.1
cd lis4.2.1
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1-1.tar.gz
tar xvzf lis-rpms-4.2.1-1.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Linux Integration Services Version 4.2 for Hyper-V の詳細については、[ダウンロード ページ](https://www.microsoft.com/download/details.aspx?id=55106)を参照してください。

## <a name="next-steps"></a>次のステップ
* これで、VM が最適化されました。使用中のシナリオについては、「[Azure VM の帯域幅とスループットのテスト](virtual-network-bandwidth-testing.md)」で結果を確認してください。
* 「[Azure Virtual Network についてよく寄せられる質問 (FAQ)](virtual-networks-faq.md)」を参照してください。

