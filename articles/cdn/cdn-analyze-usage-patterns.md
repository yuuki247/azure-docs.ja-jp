---
title: "Azure CDN の使用パターンを分析する | Microsoft Docs"
description: "次のレポートを使用して、CDN の使用パターンを表示できます: 帯域幅、転送されたデータ、ヒット数、キャッシュの状態、キャッシュ ヒット率、転送された IPV4/IPV6 データ。"
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: aadbe872dd3384c8d337b432fb3be69422ca322b
ms.contentlocale: ja-jp
ms.lasthandoff: 07/06/2017


---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN の使用パターンを分析する

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

以下のガイドでは、Verizon プロファイルの管理ポータルを使用してコア レポートを表示する手順について説明します。 [Azure Portal を使用](cdn-log-analysis.md)して、Verizon と Akamai 両方のプロファイルについて、コア分析データをストレージ、イベント ハブ、またはログ分析 (oms) にエクスポートすることもできます。

次のレポートを使用して、CDN の使用パターンを表示できます。

* 帯域幅
* 転送されたデータ
* ヒット数
* キャッシュの状態
* キャッシュ ヒット率
* 転送された IPV4/IPV6 データ

## <a name="accessing-core-reports"></a>コア レポートへのアクセス
1. CDN プロファイル ブレードで、 **[管理]** をクリックします。
   
    ![[CDN プロファイル] ブレードの [管理] ボタン](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN 管理ポータルが開きます。
2. **[分析]** タブにマウス ポインターを合わせ、次に **[Core Reports (コア レポート)]** フライアウトにポインターを合わせます。  メニューで目的のレポートをクリックします。
   
    ![CDN management portal - Core Reports menu](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>帯域幅
帯域幅レポートは、特定の期間での HTTP および HTTPS の帯域幅の使用量を示すグラフやデータ テーブルで構成されます。 すべての CDN POP または特定の POP での帯域幅の使用量を表示できます。 CDN POP 間でのトラフィックの急増や分布を Mbps で表示できます。

* [すべての Edge ノード] を選択してすべてのノードのトラフックを確認するか、ドロップダウン リストから特定のリージョン/ノードを選択します。
* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力し、[実行] をクリックして選択内容を更新します。
* データをエクスポート、ダウンロードするには、[実行] の横にある Excel シートのアイコンをクリックします。

レポートは、5 分ごとに更新されます。

![帯域幅レポート](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>転送されたデータ
このレポートは、特定の期間における HTTP および HTTPS のトラフィックの使用量を示すグラフやデータ テーブルで構成されます。 すべての CDN POP または特定の POP でのトラフィックの使用量を表示できます。 CDN POP 間でのトラフィックの急増や分布を GB で表示できます。

* [すべてのエッジ ノード] を選択してすべてのノードのトラフックを確認するか、ドロップダウン リストから特定のリージョン/ノードを選択します。
* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力し、[実行] をクリックして選択内容を更新します。
* データをエクスポート、ダウンロードするには、[実行] の横にある Excel シートのアイコンをクリックします。

レポートは、5 分ごとに更新されます。

![転送されたデータ レポート](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>ヒット数 (状態コード)
このレポートは、コンテンツの要求状態コードの分布を示します。 コンテンツのすべての要求で、HTTP 状態コードが生成されます。 状態コードにより、エッジ POP が要求をどのように処理したかがわかります。 たとえば、2xx 状態コードは、要求がクライアントに正常に提供されことを、4xx 状態コードは、エラーが発生したことを示します。 HTTP 状態コードの詳細については、 [状態コード](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)に関するページを参照してください。

* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力し、[実行] をクリックして選択内容を更新します。
* データをエクスポート、ダウンロードするには、[実行] の横にある Excel シートをクリックします。

![ヒット数レポート](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>キャッシュの状態
このレポートは、クライアント要求のキャッシュ ヒット数とキャッシュ ミス数の分布を示します。 キャッシュ ヒット数から最速のパフォーマンスが得られるため、キャッシュ ミス数と期限切れのキャッシュ ヒット数を最小化することで、データ配信速度を最適化できます。 キャッシュ ミス数を減らすには、配信元サーバーを "キャッシュなし" 応答ヘッダーを割り当てないように構成し、必要でない限りクエリ文字列のキャッシングを回避し、キャッシュできない応答コードを回避します。 期限切れのキャッシュ ヒット数を減らすには、資産の最大期間をできるだけ長くして、配信元のサーバーへの要求数を最小化します。

![キャッシュの状態レポート](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>主なキャッシュの状態には次のようなものがあります。
* TCP_HIT: Edge から提供されます。 オブジェクトはキャッシュ内にあり、その最大期間を超えませんでした。
* TCP_MISS: 配信元から提供されます。 オブジェクトはキャッシュ内になく、応答が配信元に戻りました。
* TCP_EXPIRED _MISS: 配信元との再検証後、配信元から提供されます。 オブジェクトはキャッシュ内にありましたが、その最大期間を超えました。 配信元との再検証により、キャッシュ オブジェクトが配信元からの新しい応答に置き換えられました。
* TCP_EXPIRED _MISS: 配信元との再検証後、Edge から提供されます。 オブジェクトはキャッシュ内にありましたが、その最大期間を超えました。 配信元サーバーとの再検証により、キャッシュ オブジェクトは変更されませんでした。
* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力し、[実行] をクリックして選択内容を更新します。
* データをエクスポート、ダウンロードするには、[実行] の横にある Excel シートのアイコンをクリックします。

### <a name="full-list-of-cache-statuses"></a>キャッシュの状態の完全な一覧
* TCP_HIT - この状態は、要求が POP からクライアントに直接提供された場合に報告されます。 資産は、クライアントに一番近い POP でキャッシュされ、有効な有効期限 (TTL) がある場合に、POP から直ちに提供されます。 TTL は、次の応答ヘッダーによって決定されます。
  
  * Cache-Control: s-maxage
  * Cache-Control: max-age
  * Expires
* TCP_MISS - この状態は、要求された資産のキャッシュされたバージョンが、クライアントに最も近い POP に見つからなかったことを示します。 資産が、配信元サーバーまたは配信元シールド サーバーから要求されます。 配信元サーバーまたは配信元シールド サーバーが資産を返した場合、その資産はクライアントに提供され、クライアントとエッジ サーバーの両方でキャッシュされます。 それ以外の場合、200 以外の状態コード (403 Forbidden、404 Not Found など) が返されます。
* TCP_EXPIRED _HIT - この状態は、期限切れの TTL の資産をターゲットとした要求が (資産の最大期間を超えた場合など)、POP からクライアントに直接提供された場合に報告されます。
  
    通常、期限切れの要求では、配信元のサーバーに再検証の要求が送られます。 TCP_EXPIRED _HIT を発生させるには、配信元のサーバーが、より新しいバージョンの資産が存在しないことを示す必要があります。 このような状況では通常、その資産のCache-Control と Expires ヘッダーが更新されます。
* TCP_EXPIRED _MISS - この状態は、期限切れのキャッシュされた資産のより新しいバージョンが POP からクライアントに提供された場合に報告されます。 これは、キャッシュされた資産の TTL が切れ (max-age が切れた場合など)、配信元サーバーからより新しいバージョンの資産が返された場合に発生します。 この新しいバージョンの資産が、キャッシュされたバージョンの代わりに、クライアントに提供されます。 またこれは、エッジ サーバーとクライアントでキャッシュされます。
* CONFIG_NOCACHE - この状態は、エッジ POP 上のユーザー固有の構成で資産のキャッシュが回避されたことを示します。
* NONE - この状態は、キャッシュ内容更新チェックが実行されなかったことを示します。
* TCP_ CLIENT_REFRESH _MISS - この状態は、使わなくなった資産の新しいバージョンをエッジ POP が配信元サーバーから取得することが HTTP クライアント (ブラウザーなど) により強制された場合に報告されます。
  
    既定では、サーバーで HTTP クライアントはエッジ サーバーに対して新しいバージョンの資産を配信元のサーバーから取得することを強制できません。
* TCP_ PARTIAL_HIT - この状態は、バイト範囲の要求で、部分的にキャッシュされた資産のヒットが発生した場合に報告されます。 要求されたバイト範囲は、POP からクライアントに直ちに提供されます。
* UNCACHEABLE - この状態は、資産の Cache-Control と Expires ヘッダーで、資産が POP で、または HTTP クライアントによりキャッシュされる必要があることが示された場合に報告されます。 この種の要求は、配信元サーバーから提供されます。

## <a name="cache-hit-ratio"></a>キャッシュ ヒット率
このレポートは、キャッシュから直接提供されたキャッシュされた要求の割合を示します。

レポートは次の詳細を示します。

* 要求された内容が依頼者に最も近い POP でキャッシュされたかどうか。
* 要求がネットワークのエッジから直接提供されたかどうか。
* 要求で、配信元サーバーとの再検証が必要だったかどうか。

次の情報は提供されません。

* 国のフィルタリング オプションによって拒否された要求。
* キャッシュできないことがヘッダーで示された資産の要求。 たとえば、Cache-Control: private、Cache-Control: no-cache、または Pragma: no-cache ヘッダーでは資産をキャッシュできません。
* 部分的にキャッシュされた内容のバイト範囲要求。

数式は次のとおりです: (TCP_ HIT/(TCP_ HIT+TCP_MISS))*100

* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力し、[実行] をクリックして選択内容を更新します。
* データをエクスポート、ダウンロードするには、[実行] の横にある Excel シートのアイコンをクリックします。

![キャッシュ ヒット率レポート](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>転送された IPV4/IPV6 データ
このレポートは、IPV4 と IPV6 のトラフィック使用量の分布を示します。

![転送された IPV4/IPV6 データ](./media/cdn-reports/cdn-ipv4-ipv6.png)

* [日付範囲] を選択して今日/今週/今月などのデータを表示するか、カスタム日付を入力します。
* 次に、[実行] をクリックして、選択内容を更新します。

## <a name="considerations"></a>考慮事項
レポートは過去 18 か月分のみ生成できます。


