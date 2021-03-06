---
title: "VM に Trend Micro Deep Security をインストールする | Microsoft Docs"
description: "この記事では、Azure でクラシック デプロイ モデルを使用して作成された VM で Trend Micro のセキュリティをインストールして構成する方法について説明します。"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: e2ccb78da419b12a9845d720efb5b0229d4f455b
ms.contentlocale: ja-jp
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Windows VM に Trend Micro Deep Security をサービスとしてインストールし、構成する方法
> [!IMPORTANT]
> Azure には、リソースの作成と操作に関して、 [Resource Manager とクラシック](../../../resource-manager-deployment-model.md)の 2 種類のデプロイメント モデルがあります。 この記事では、クラシック デプロイ モデルの使用方法について説明します。 最新のデプロイでは、リソース マネージャー モデルを使用することをお勧めします。

この記事では、Windows Server を実行している既存の仮想マシン (VM) または新しい VM に Trend Micro Deep Security as a Service をインストールして構成する方法を説明します。 Deep Security as a Service には、ウイルス対策、ファイアウォール、侵入防止システム、変更監視が含まれています。

このクライアントは、VM エージェントを使用することにより、セキュリティ拡張機能としてインストールされます。 新しい仮想マシンには、Azure Portal によって VM エージェントが自動作成されるときに、Deep Security Agent をインストールします。

従来のポータル、Azure CLI、または PowerShell を使用して作成された既存の仮想マシンには、VM エージェントがありません。 VM エージェントがない既存の仮想マシンの場合は、最初に VM エージェントをダウンロードしてインストールする必要があります。 この記事では、この両方の場合について説明します。

Trend Micro のオンプレミスのソリューション用サブスクリプションが既にある場合は、それを使用して Azure の仮想マシンを保護できます。 サブスクリプションがない場合には、サインアップして試用サブスクリプションを利用できます。 このソリューションの詳細については、Trend Micro ブログの Deep Security 用の [Microsoft Azure VM エージェント拡張機能](http://go.microsoft.com/fwlink/p/?LinkId=403945)に関する投稿を参照してください。

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>新しい VM に Deep Security Agent をインストールする

[Azure ポータル](http://portal.azure.com) を使用すると、**Marketplace** のイメージを使用して仮想マシンを作成するときに、Trend Micro のセキュリティ拡張機能をインストールできます。 仮想マシンを 1 つだけ作成する場合には、この方法が Trend Micro の保護機能を追加する最も簡単な方法です。

この **Marketplace** のエントリを使用すると、仮想マシンの設定を支援するウィザードが起動します。 ウィザードの 3 番目のパネルの **[設定]** ブレードを使用して、Trend Micro のセキュリティ拡張機能をインストールします。  一般的な手順については、「[Windows を実行する仮想マシンを Azure ポータルで作成する](tutorial.md)」をご覧ください。

ウィザードの **[設定]** ブレードに移動して、次の手順を実行します。

1. **[拡張]** をクリックして、次のウィンドウで **[拡張機能の追加]** をクリックします。

   ![拡張機能の追加開始][1]

2. **[新しいリソース]** ウィンドウで **Deep Security Agent** を選択します。 Deep Security Agent ウィンドウで、**[作成]** をクリックします。

   ![Deep Security Agent の識別][2]

3. 拡張機能の**テナント ID**と**テナント アクティべーション パスワード**を入力します。 必要に応じて、**セキュリティ ポリシー識別子**を入力できます。 次に、**[OK]** をクリックして、クライアントを追加します。

   ![拡張機能の詳細提示][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>既存の VM に Deep Security Agent をインストールする
既存の VM にエージェントをインストールするには、次の条件が必要です。

* Azure PowerShell モジュール Version 0.8.2 以降がローカル コンピューターにインストールされていること。 インストールした Azure PowerShell のバージョンは、 **Get-Module azure | format-table version** コマンドを使用して確認できます。 最新バージョンの説明とダウンロード用リンクについては、「 [Azure PowerShell のインストールおよび構成方法](/powershell/azure/overview)」を参照してください。 `Add-AzureAccount`を使用して Azure サブスクリプションにログインします。
* VM エージェントがターゲットの仮想マシンにインストールされていること。

最初に、VM エージェントがインストールされていることを確認します。 クラウド サービス名と仮想マシン名を入力して、管理者レベルの Azure PowerShell のコマンド プロンプトで、次のコマンドを実行します。 引用符内のすべての文字 (< および > を含む) を置き換えます。

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

クラウド サービスや仮想マシンの名前がわからない場合は、**Get-AzureVM** を実行します。現在のサブスクリプションのすべての仮想マシンの情報が表示されます。

**write-host** コマンドによって **True** が返された場合は、VM エージェントがインストールされています。 **False**が返された場合は、Azure ブログの [VM エージェントと拡張機能に関する記事のパート 2](http://go.microsoft.com/fwlink/p/?LinkId=403947)で、手順とダウンロード用リンクをご確認ください。

VM エージェントがインストールされている場合は、次のコマンドを実行します。

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>次のステップ
エージェントがインストールされると、起動までに数分かかります。 起動後、仮想マシン上で Deep Security をアクティブにする必要があります。これは Deep Security Manager で管理できるようにするためです。 詳しくは、次の記事をご覧ください。

* このソリューションに関する Trend の記事「 [Instant-On Cloud Security for Microsoft Azure (Microsoft Azure 用の Instant-On クラウド セキュリティ)](http://go.microsoft.com/fwlink/?LinkId=404101)
* 仮想マシンを構成するための [Windows PowerShell サンプル スクリプト](http://go.microsoft.com/fwlink/?LinkId=404100)
* [説明](http://go.microsoft.com/fwlink/?LinkId=404099) 

## <a name="additional-resources"></a>その他のリソース
[Windows Server が実行されている仮想マシンにログオンする方法]

[Azure VM 拡張機能とその機能]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Windows Server が実行されている仮想マシンにログオンする方法]:connect-logon.md
[Azure VM 拡張機能とその機能]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

