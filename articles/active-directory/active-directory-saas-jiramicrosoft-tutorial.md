---
title: "チュートリアル: Azure Active Directory と JIRA SAML SSO by Microsoft の統合 | Microsoft Docs"
description: "Azure Active Directory と JIRA SAML SSO by Microsoft の間でシングル サインオンを構成する方法について説明します。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: b5f7813c8244d2964b6894ae49cd64e0ee71b704
ms.contentlocale: ja-jp
ms.lasthandoff: 07/21/2017

---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>チュートリアル: Azure Active Directory と JIRA SAML SSO by Microsoft の統合

このチュートリアルでは、JIRA SAML SSO by Microsoft と Azure Active Directory (Azure AD) を統合する方法について説明します。

JIRA SAML SSO by Microsoft と Azure AD の統合には、次の利点があります。

- JIRA SAML SSO by Microsoft にアクセスする Azure AD ユーザーを制御できます
- ユーザーが自分の Azure AD アカウントで JIRA SAML SSO by Microsoft に自動的にサインオン (シングル サインオン) するように設定できます
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

JIRA SAML SSO by Microsoft と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- 64 ビットの Windows サーバー (オンプレミスまたはクラウド IaaS インフラストラクチャ) にインストールされている JIRA サーバー アプリケーション
- JIRA サーバーの HTTPS が有効になっていること
- JIRA プラグインがサポートされているバージョンであること (下記のセクションをご覧ください)
- JIRA サーバーが認証のためにインターネット、特に Azure AD ログイン ページにアクセスでき、Azure AD からトークンを受け取れること
- 管理者の資格情報が JIRA に設定されていること
- WebSudo が JIRA で無効になっていること
- テスト ユーザーが JIRA サーバー アプリケーションで作成されていること

> [!NOTE]
> このチュートリアルの手順をテストする場合、JIRA の運用環境を使用しないことをお勧めします。 最初にアプリケーションの開発環境またはステージング環境で統合をテストし、そのあとに運用環境を使用してください。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。

## <a name="supported-versions-of-jira"></a>サポートされている JIRA のバージョン 

現時点では、次のバージョンの JIRA がサポートされています。

- JIRA Core と JIRA Software: 6.0 から 7.2.0
- JIRA Service Desk: 3.0 から 3.2

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの JIRA SAML SSO by Microsoft の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>ギャラリーからの JIRA SAML SSO by Microsoft の追加
Azure AD への JIRA SAML SSO by Microsoft の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に JIRA SAML SSO by Microsoft を追加する必要があります。

**ギャラリーから JIRA SAML SSO by Microsoft を追加するには、次の手順を実行します。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![アプリケーション][2]
    
3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![アプリケーション][3]

4. 検索ボックスに「**JIRA SAML SSO by Microsoft**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. 結果ウィンドウで、**[JIRA SAML SSO by Microsoft]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、JIRA SAML SSO by Microsoft で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する JIRA SAML SSO by Microsoft ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと JIRA SAML SSO by Microsoft の関連ユーザーの間で、リンク関係が確立されている必要があります。

JIRA SAML SSO by Microsoft で、Azure AD の **[ユーザー名]** の値を **[Username]** の値として割り当ててリンク関係を確立します。

JIRA SAML SSO by Microsoft で Azure AD のシングル サインオンを構成してテストするには、次の一連の作業を完了させる必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[JIRA SAML SSO by Microsoft テスト ユーザーの作成](#creating-a-jira-saml-sso-by-microsoft-test-user)** - JIRA SAML SSO by Microsoft で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[Testing Single Sign-On](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、JIRA SAML SSO by Microsoft アプリケーションでシングル サインオンを構成します。

**JIRA SAML SSO by Microsoft で Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure Portal の **JIRA SAML SSO by Microsoft** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![[シングル サインオンの構成]][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. **[JIRA SAML SSO by Microsoft Domain and URLs]/(JIRA SAML SSO by Microsoft のドメインと URL\)** セクションで、次の手順を実行します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    a. **[サインオン URL]** ボックスに、`https://<domain:port>/plugins/servlet/saml/auth` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、`https://<domain:port>/` の形式で URL を入力します。

    c. **[応答 URL]** ボックスに、`https://<domain:port>/plugins/servlet/saml/auth` のパターンを使用して URL を入力します。

    > [!NOTE] 
    > これらは実際の値ではありません。 実際の識別子、応答 URL、サインオン URL でこれらの値を更新します。 名前付き URL である場合は、ポートは省略できます。 これらの値は JIRA プラグインの構成中に受け取ります (これについてはこのチュートリアルの後半で説明します)。
 
4. **メタデータ** URL を生成するには、次の手順を実行します。

    a. **[アプリの登録]** をクリックします。
    
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    b. **[エンドポイント]** をクリックして **[エンドポイント]** ダイアログ ボックスを開きます。  
    
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    c. コピー ボタンをクリックして、**フェデレーション メタデータ ドキュメント**の URL をコピーしてノートパッドに貼り付けます。
    
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    d. 次に、**JIRA SAML SSO by Microsoft** のプロパティ ページに移動し、**[コピー]** ボタンで**[アプリケーション ID]** をコピーしてメモ帳に貼り付けます。
 
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    e. `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` パターンを使用して**メタデータ URL** を生成し、あとでプラグインの構成の際に使用するために、この値をメモ帳にコピーしておきます。

5. **[保存]** ボタンをクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. JIRA プラグインについて [Microsoft](mailto:waadpartners@microsoft.com) にお問い合わせいただく際は、次の情報をお知らせください。
    
    *   お名前:
    *   プライマリ ドメイン名:
    *   Azure AD Premium: はい/いいえ (プラグインは Free、Basic、Premium SKU のすべてのお客様にご利用いただけます)
    *   この統合を使用するユーザーの数:
    *   JIRA のバージョン:
    *   説明:

7. 別の Web ブラウザー ウィンドウで、JIRA インスタンスに管理者としてログインします。

8. 歯車をポイントし、**[Add-ons]\(アドオン\)** をクリックします。
    
    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. [アドオン] タブ セクションで、**[アドオンの管理]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. Microsoft が提供しているプラグインを手動でアップロードします。 プラグインがインストールされると、**[アドオンの管理]** セクションの **[User Installed]\(ユーザー インストール\)** アドオン セクションに表示されます。

11. **[Configure]\(構成\)** をクリックして、新しいプラグインを構成します。

12. 構成ページで次の手順を実行します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    a. **メタデータ URL** に Azure AD から生成した**メタデータ URL** を貼り付け、**[解決]** をクリックします。 IdP メタデータ URL が読み取られ、すべてのフィールド情報が設定されます。

    > [!Note]
    > 既定の SAML ユーザー ID の場所は、名前識別子です。 属性オプションでこれを変更して、適切な属性名を入力できます。

    > [!TIP]
    > メタデータの解決でエラーが発生しないように、アプリに対してマップされている証明書が 1 つしかないようにします。 証明書が複数ある場合は、メタデータの解決の際に管理者に対してエラーが表示されます。
    
    b. **識別子、応答 URL、サインオン URL** の値をコピーして、Azure Portal の **[JIRA SAML SSO by Microsoft Domain and URLs]/(JIRA SAML SSO by Microsoft のドメインと URL\)** セクションにある**識別子、応答 URL、サインオン URL** ボックスにそれぞれ貼り付けます。

    c. ユーザーのログイン画面に表示するボタン名を **[Login Button Name]\(ログイン ボタン名\)** に入力します。

    d. **[SAML User ID Locations]\(SAML ユーザー ID の場所\)** で、**[User ID is in the NameIdentifier element of the Subject statement]\(Subject ステートメントの NameIdentifier 要素内のユーザー ID\)**、または **[User ID is in an Attribute element]\(Attribute 要素内のユーザー ID\)** を選択します。  この ID は JIRA ユーザー ID である必要があります。 ユーザー ID が一致しない場合、システムがユーザーのログインを許可しません。 
    
    e. **[User ID is in an Attribute element]\(Attribute 要素内のユーザー ID\)** を選択する場合は、ユーザー ID がある属性の名前を **[属性名]** ボックスに入力します。 

    f.SAML 属性の属性名またはスキーマ リファレンスを入力します。 Azure AD でフェデレーション ドメイン (ADFS など) を使用している場合は、**[Enable Home Realm Discovery]\(ホーム領域の検出を有効にする\)** をクリックして **[ドメイン名]** を構成します。
    
    g. ADFS ベースでログインする場合は、**[ドメイン名]** にドメイン名を入力します。

    h. ユーザーが JIRA からログアウトしたときに Azure AD からもログアウトさせる場合は、**[Enable Single Sign out]\(シングル サインアウトを有効にする\)** をオンにします。 

    i. **[Save (保存)]** ボタンをクリックして、設定を保存します。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 組み込みドキュメント機能の詳細については、[Azure AD の組み込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関する記事をご覧ください。
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure Portal** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. ダイアログの上部にある **[追加]** をクリックして、**[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d. ページの下部にある「**[Create]**」を参照してください。
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a>JIRA SAML SSO by Microsoft テスト ユーザーの作成

オンプレミス サーバーで Azure AD ユーザーの JIRA へのログインを有効にするには、そのユーザーを JIRA SAML SSO by Microsoft にプロビジョニングする必要があります。 JIRA SAML SSO by Microsoft では、手動でプロビジョニングします。

**ユーザー アカウントをプロビジョニングするには、次の手順に従います。**

1. 管理者として、オンプレミス サーバーの JIRA にログインします。

2. 歯車をポイントし、**[User management]\(ユーザー管理\)** をクリックします。

    ![従業員の追加](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. [Administrator Access]\(管理者アクセス\) のページにリダイレクトされるので、**パスワード**を入力し、**[Confirm]\(確認\)** ボタンをクリックします。

    ![従業員の追加](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. **[User management]\(ユーザー管理\)** タブ セクションで、**[create user]\(ユーザーの作成\)** をクリックします。

    ![従業員の追加](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. **[Create new user]\(新しいユーザーの作成\)** ダイアログ ページで、以下の手順を実行します。

    ![従業員の追加](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    a. **[Email address]\(メール アドレス\)** ボックスに、ユーザーのメール アドレス (Brittasimon@contoso.com など) を入力します。

    b. **[Full Name]\(フル ネーム\)** ボックスに、ユーザーの氏名 (Britta Simon など) を入力します。

    c. **[Username]\(ユーザー名\)** ボックスに、ユーザーの電子メール (Brittasimon@contoso.com など) を入力します。

    d. **[Password]\(パスワード\)** ボックスに、ユーザーのパスワードを入力します。

    e. **[Create user]\(ユーザーの作成\)** をクリックします。   

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に JIRA SAML SSO by Microsoft へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**JIRA SAML SSO by Microsoft に Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で、**[JIRA SAML SSO by Microsoft]** を選択します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="testing-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルの [JIRA SAML SSO by Microsoft] のタイルをクリックすると、JIRA SAML SSO by Microsoft アプリケーションに自動的にサインオンされます。
アクセス パネルの詳細については、[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)に関する記事を参照してください。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png


