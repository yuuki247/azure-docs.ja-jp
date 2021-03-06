---
title: "Python を使用して Azure Data Lake Analytics を管理する | Microsoft Docs"
description: "Python を使用して Data Lake Store アカウントを作成し、ジョブを送信する方法について説明します。 "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: ab652d6e1e8e6d9bc443af324943bfe24ce4bdc1
ms.contentlocale: ja-jp
ms.lasthandoff: 06/20/2017


---


# <a name="manage-azure-data-lake-analytics-using-python"></a>Python を使用して Azure Data Lake Analytics を管理する

## <a name="python-versions"></a>Python のバージョン

* Python の 64 ビット バージョンを使用する必要があります。
* **[Python.org ダウンロード](https://www.python.org/downloads/)**にある標準の python ディストリビューションを使用できます。 
* 多くの開発者は、 **[Anaconda Python ディストリビューション](https://www.continuum.io/downloads)**を使用すると便利なことがわかります。  
* この資料は、標準の Python ディストリビューションからの Python バージョン 3.6 を使用して作成されました。

## <a name="install-azure-python-sdk"></a>Azure Python SDK をインストールする

次のモジュールをインストールする必要があります。

* **azure-mgmt-resource** モジュールには、Active Directory 用のその他の Azure モジュールなどが含まれています。
* **azure-mgmt-datalake-store** モジュールには、Azure Data Lake Store アカウント管理操作が含まれています。
* **azure-datalake-store** モジュールには、Azure Data Lake Store ファイル システム操作が含まれています。 
* **azure-datalake-analytics** モジュールには、Azure Data Lake Analytics 操作が含まれています。 

最初に、次のコマンドを実行して最新の `pip` があることを確認します。

```
python -m pip install --upgrade pip
```

このドキュメントは、`pip version 9.0.1` を使用して記述されています。

コマンドラインからモジュールをインストールするには、次の `pip` コマンドを使用します。

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>新しい Python スクリプトを作成する

以下のコードをスクリプトに貼り付けます。

```
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

このスクリプトを実行して、モジュールをインポートできることを確認します。

## <a name="authentication"></a>認証

### <a name="interactive-user-authentication-with-a-pop-up"></a>ポップアップを使用した対話型ユーザー認証

この方法はサポートされていません。

### <a name="interactice-user-authentication-with-a-device-code"></a>デバイス コードを使用した対話型ユーザー認証

```
user = input('Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-a-spi-and-a-secret"></a>SPI およびシークレットを使用した非対話型認証

```
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-a-api-and-a-cetificate"></a>API および証明書を使用した非対話型認証

この方法はサポートされていません。

## <a name="common-script-variables"></a>共通スクリプト変数

これらの変数は、サンプルで使用します。

```
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adls = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a>クライアントを作成する

```
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Azure リソース グループを作成する

```
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics アカウントを作成する

最初にストア アカウントを作成します。

```
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```
次にそのストアを使用する ADLA アカウントを作成します。

```
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics ジョブを送信する

```
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adlaAccountName,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-the-job-to-finish"></a>ジョブの完了を待機する

```
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adlaAccountName, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="next-steps"></a>次のステップ

- 他のツールを使用する同じチュートリアルを表示するには、ページの上部にあるタブ セレクターをクリックします。
- U-SQL の詳細については、「 [Azure Data Lake Analytics U-SQL 言語の使用](data-lake-analytics-u-sql-get-started.md)」を参照してください。
- 管理タスクについては、「 [Azure Portal を使用する Azure Data Lake Analytics の管理](data-lake-analytics-manage-use-portal.md)」をご覧ください。


