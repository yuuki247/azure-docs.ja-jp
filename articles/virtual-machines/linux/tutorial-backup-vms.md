---
title: "Azure Linux VM のバックアップ | Microsoft Docs"
description: "Linux VM は、Azure Backup でバックアップすることによって保護します。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/05/2017
ms.author: cynthn
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 4dcfad63fdc610160bd47a3b900591fb06585005
ms.contentlocale: ja-jp
ms.lasthandoff: 06/01/2017

---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Azure での Linux 仮想マシンのバックアップ

データは、定期的にバックアップすることで保護することができます。 Azure Backup では、geo 冗長 Recovery コンテナーに保存される復旧ポイントが作成されます。 復旧ポイントから復元するときは、VM 全体を復元するか、特定のファイルだけを復元することができます。 この記事では、nginx を実行する Linux VM に単一のファイルを復元する方法について説明します。 まだ使用する VM がない場合は、[Linux のクイック スタート](quick-create-cli.md)を使用して作成してください。 このチュートリアルで学習する内容は次のとおりです。

> [!div class="checklist"]
> * VM のバックアップを作成する
> * 毎日のバックアップをスケジュールする
> * バックアップからファイルを復元する



## <a name="backup-overview"></a>Backup の概要

Azure Backup サービスによってバックアップが開始されると、バックアップ拡張機能がトリガーされて特定時点のスナップショットが作成されます。 Azure Backup サービスには Linux の _VMSnapshotLinux_ 拡張機能が使用されます。 拡張機能は、VM の初回バックアップ中、VM が実行されている場合にインストールされます。 VM が実行されていない場合、Backup サービスは基盤となるストレージのスナップショットを取ります (VM が停止している間はアプリケーション書き込みが行われないため)。

Azure Backup サービスがスナップショットを取ると、データはバックアップコンテナーに転送されます。 効率を最大に高めるために、前回のバックアップ以降に変更されたデータ ブロックが特定され、そのデータのみが転送されます。

データ転送が完了すると、スナップショットが削除され、回復ポイントが作成されます。


## <a name="create-a-backup"></a>バックアップの作成
Recovery Services コンテナーに対するバックアップを 1 日 1 回の単純なスケジュールで作成します。 

1. [Azure ポータル](https://portal.azure.com/)にサインインします。
2. 左側のメニューから **[仮想マシン]** を選択します。 
3. バックアップする VM を一覧から選択します。
4. その VM のブレードの **[設定]** セクションで **[バックアップ]** をクリックします。 **[バックアップの有効化]** ブレードが開きます。
5. **[Recovery Services コンテナー]** の **[新規作成]** をクリックして、新しいコンテナーの名前を入力します。 仮想マシンと同じリソース グループで、仮想マシンと同じ場所に新しいコンテナーが作成されます。
6. **[バックアップ ポリシー]** をクリックします。 この例では、既定値のまま **[OK]** をクリックします。
7. **[バックアップの有効化]** ブレードの **[バックアップの有効化]** をクリックします。 これで、既定のスケジュールで毎日のバックアップが作成されます。
10. 最初の復旧ポイントを作成するには、**[バックアップ]** ブレードの **[今すぐバックアップ]** をクリックします。
11. **[今すぐバックアップ]** ブレードでカレンダー アイコンをクリックし、カレンダー コントロールを使ってこの復旧ポイントを保持する最終日を選び、**[バックアップ]** をクリックします。
12. 対象の VM の **[バックアップ]** ブレードに、作成されている復旧ポイントの数が表示されます。

    ![[回復ポイント]](./media/tutorial-backup-vms/backup-complete.png)

初回バックアップには約 20 分かかります。 バックアップが完了したら、このチュートリアルの次のパートに進んでください。

## <a name="restore-a-file"></a>ファイルの復元

ファイルを誤って削除または変更した場合は、ファイルの回復機能を使ってバックアップ コンテナーからファイルを復元することができます。 ファイルの回復には、VM 上で動作するスクリプトが使用され、復旧ポイントがローカル ドライブとしてマウントされます。 これらのドライブは、12 時間マウントされた状態になります。その間に復旧ポイントからファイルをコピーし、VM に復元することができます。  

この例は、nginx の既定の Web ページ (/var/www/html/index.nginx-debian.html) を復元する方法を示しています。 この例に使用している VM の パブリック IP アドレスは *13.69.75.209* です。 VM の IP アドレスは次のようにして調べることができます。

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. ローカル コンピューターでブラウザーを開き、VM のパブリック IP アドレスを入力して nginx の既定の Web ページを表示します。

    ![nginx の既定の Web ページ](./media/tutorial-backup-vms/nginx-working.png)

1. VM に SSH で接続します。

    ```bash
    ssh 13.69.75.209
    ```
2. /var/www/html/index.nginx-debian.html を削除します。

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. ローカル コンピューターで、Ctrl キーを押しながら F5 キーを押してブラウザーを最新の情報に更新すると、nginx の既定のページが消えます。

    ![nginx の既定の Web ページ](./media/tutorial-backup-vms/nginx-broken.png)
    
1. ローカル コンピューターから [Azure Portal](https://portal.azure.com/) にサインインします。
6. 左側のメニューの **[仮想マシン]** を選択します。 
7. 一覧から VM を選択します。
8. その VM のブレードの **[設定]** セクションで **[バックアップ]** をクリックします。 **[バックアップ]** ブレードが開きます。 
9. ブレード上部のメニューから **[ファイルの回復 (プレビュー)]** を選択します。 **[ファイルの回復 (プレビュー)]** ブレードが表示されます。
10. **[ステップ 1: 回復ポイントを選択する]** で、ドロップダウンから復旧ポイントを選択します。
11. **[ステップ 2: ファイルを参照および回復するためのスクリプトをダウンロードする]** の **[実行可能ファイルのダウンロード]** ボタンをクリックします。 ダウンロードしたファイルをローカル コンピューターに保存します。
7. **[スクリプトのダウンロード]** をクリックして、スクリプト ファイルをローカルにダウンロードします。
8. Bash プロンプトを開いて以下のように入力します。*Linux_myVM_05-05-2017.sh* は実際にダウンロードしたスクリプトのパスとファイル名に、*azureuser* は VM のユーザー名に、*13.69.75.209* は VM のパブリック IP アドレスに置き換えてください。
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. ローカル コンピューターから VM への SSH 接続を開きます。

    ```bash
    ssh 13.69.75.209
    ```
    
10. VM で、スクリプト ファイルに実行権限を追加します。

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. 復旧ポイントをファイル システムとしてマウントするスクリプトを VM で実行します。

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. スクリプトからの出力によって、マウント ポイントのパスが得られます。 出力の例を次に示します。

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. VM 上の先ほどファイルを削除した場所に、マウント ポイントから nginx の既定の Web ページをコピーします。

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. ローカル コンピューターで、先ほど VM の IP アドレスに接続して nginx の既定のページを表示したブラウザー タブを開きます。 Ctrl キーを押しながら F5 キーを押して、ブラウザー ページを最新の情報に更新します。 今度は、既定のページが再び正常に表示されます。

    ![nginx の既定の Web ページ](./media/tutorial-backup-vms/nginx-working.png)

18. ローカル コンピューターで Azure Portal のブラウザー タブに移動し、**[ステップ 3: 回復後にディスクのマウントを解除する]** の **[ディスクのマウント解除]** ボタンをクリックします。 この手順を実行するのを忘れた場合、12 時間後にマウントポイントへの接続が自動的に解除されます。 12 時間が経過した後、新しいマウント ポイントを作成するには、新しいスクリプトをダウンロードする必要があります。


## <a name="next-steps"></a>次のステップ

このチュートリアルで学習した内容は次のとおりです。

> [!div class="checklist"]
> * VM のバックアップを作成する
> * 毎日のバックアップをスケジュールする
> * バックアップからファイルを復元する

次のチュートリアルに進み、仮想マシンの監視について学習してください。

> [!div class="nextstepaction"]
> [仮想マシンの監視](tutorial-monitoring.md)


