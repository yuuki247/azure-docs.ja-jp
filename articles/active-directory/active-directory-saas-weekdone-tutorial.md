---
title: "チュートリアル: Azure Active Directory と Weekdone の統合 | Microsoft Docs"
description: "Azure Active Directory と Weekdone の間でシングル サインオンを構成する方法について説明します。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 1e99bdc4509d8cbab9012d36207f21be2cc75e5e
ms.contentlocale: ja-jp
ms.lasthandoff: 06/07/2017


---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a>チュートリアル: Azure Active Directory と Weekdone の統合
このチュートリアルの目的は、Weekdone と Azure Active Directory (Azure AD) を統合する方法を説明することです。

Weekdone と Azure AD の統合には、次の利点があります。

* Weekdone にアクセスする Azure AD ユーザーを制御できます。
* ユーザーが自分の Azure AD アカウントで自動的に Weekdone にサインオン (シングル サインオン) するように設定できます。
* 1 つの中央サイト (Azure クラシック ポータル) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「 [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」を参照してください。

## <a name="prerequisites"></a>前提条件
Azure AD と Weekdone の統合を構成するには、次のものが必要です。

* Azure AD サブスクリプション
* Weekdone シングル サインオン (SSO) が有効なサブスクリプション

>[!NOTE]
>このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。
> 
> 

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

* 必要な場合を除き、運用環境は使用しないでください。
* Azure AD の評価環境がない場合は、[こちらから](https://azure.microsoft.com/pricing/free-trial/) 1 か月の評価版を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD の SSO をテストできるようにすることです。 

このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの Weekdone の追加
2. Azure AD SSO の構成とテスト

## <a name="adding-weekdone-from-the-gallery"></a>ギャラリーからの Weekdone の追加
Azure AD への Weekdone の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Weekdone を追加する必要があります。

**ギャラリーから Weekdone を追加するには、次の手順に従います。**

1. **Azure クラシック ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
   
    ![Active Directory][1]
2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。
3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
   
    ![[アプリケーション]][2]
4. ページの下部にある **[追加]** をクリックします。
   
    ![アプリケーション][3]
5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。
   
    ![アプリケーション][4]
6. 検索ボックスに、「 **Weekdone**」と入力します。
   
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_01.png)
7. 結果ウィンドウで **[Weekdone]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。
   
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_02.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Weekdone で Azure AD の SSO を構成し、テストする方法を説明することです。

SSO を機能させるには、Azure AD ユーザーに対応する Weekdone ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Weekdone の関連ユーザーの間で、リンク関係が確立されている必要があります。

このリンク関係は、Azure AD の **[ユーザー名]** の値を、Weekdone の **[Username]** の値として割り当てることで確立されます。

Weekdone で Azure AD の SSO を構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[Weekdone テスト ユーザーの作成](#creating-a-weekdone-test-user)** - Weekdone で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成
このセクションの目的は、Azure クラシック ポータルで Azure AD のシングル サインオンを有効にすることと、Weekdone アプリケーションで SSO を構成することです。

**Weekdone で Azure AD SSO を構成するには、次の手順に従います。**

1. Azure クラシック ポータルの **Weekdone** アプリケーション統合ページで **[シングル サインオンの構成]** をクリックし、**[シングル サインオンの構成]** ダイアログを開きます。
   
    ![[シングル サインオンの構成]][6] 
2. **[ユーザーの Weekdone へのアクセスを設定してください]** ページで、**[Azure AD のシングル サインオン]** を選択し、**[次へ]** をクリックします。
   
    ![[シングル サインオンの構成]](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_03.png) 
3. **[アプリケーション設定の構成]** ダイアログ ページで、**IDP 開始モード**でアプリケーションを構成する場合は、次の手順を実行し、**[次へ]** をクリックします。
   
    ![[シングル サインオンの構成]](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_04.png) 

   1. **[応答 URL]** ボックスに、**"https://weekdone.com/a/azure"** の形式で URL を入力します。
   2. **[識別子]** ボックスに、**"https://weekdone.com/a/azure/metadata"** の形式で URL を入力します。
   3. **[次へ]** をクリックします。
4. **[アプリケーション設定の構成]** ダイアログ ページで、**SP 開始モード**でアプリケーションを構成する場合は、**[詳細設定を表示します (オプション)]** を選択し、**[サインオン URL]** と **[識別子]** を入力して **[次へ]** をクリックします。
   
    ![[シングル サインオンの構成]](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_06.png) 
   
   1. **[サインオン URL]** ボックスに、次のパターンを使用して、ユーザーが Weekdone アプリケーションへのサインオンに使用する URL を入力します。**"https://weekdone.com/a/azure"**
   2. **[識別子]** ボックスに、**"https://weekdone.com/a/azure/metadata"** の形式で URL を入力します。
   3. **[次へ]** をクリックします。
2. **[Weekdone でのシングル サインオンの構成]** ページで、次の手順を実行し、**[次へ]** をクリックします。
   
    ![[シングル サインオンの構成]](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_05.png) 
   1. **[証明書のダウンロード]**をクリックし、コンピューターに証明書ファイルを保存します。
   2. **[次へ]** をクリックします。
    
3. お使いのアプリケーション用に構成された SSO を取得するには、Weekdone のサポート チーム (hello@weekdone.com) にお問い合わせください。 
4. Weekdone チーム側で SSO を設定する必要があるため、ダウンロードした証明書ファイルをメールに添付して、メタデータ URL (ISSUER URL、SAML SSO URL、および SINGLE SIGN-OUT SERVICE URL) をチームと共有してください。
5. Azure クラシック ポータルで、シングル サインオンの構成確認を選択し、 **[次へ]**をクリックします。
   
    ![Azure AD のシングル サインオン][10]
6. **[シングル サインオンの確認]** ページで、**[完了]** をクリックします。  
   
    ![Azure AD のシングル サインオン][11]

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure クラシック ポータルで Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][20]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure クラシック ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
   
   ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_09.png) 
    
2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。
3. 上部のメニューで **[ユーザー]**をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 
    
4. 下部にあるツール バーで **[ユーザーの追加]** をクリックして、**[ユーザーの追加]** ダイアログ ボックスを開きます。
   
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 
    
5. **[このユーザーに関する情報の入力]** ダイアログ ページで、次の手順に従います。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_05.png) 
   
   1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
   2. [ユーザー名] **ボックス**に「**BrittaSimon**」と入力します。
   3. **[次へ]** をクリックします。
    
6. **[ユーザー プロファイル]** ダイアログ ページで、次の手順に従います。

   ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_06.png) 
   
   1. **[名]** ボックスに「**Britta**」と入力します。  
   2. **[姓]** ボックスに「**Simon**」と入力します。
   3. **[表示名]** ボックスに「**Britta Simon**」と入力します。
   4. **[ロール]** 一覧で **[ユーザー]** を選択します。
   5. **[次へ]** をクリックします。
  
7. **[一時パスワードの取得]** ダイアログ ページで、**[作成]** をクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_07.png) 
    
8. **[一時パスワードの取得]** ダイアログ ページで、次の手順に従います。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-weekdone-tutorial/create_aaduser_08.png) 
   
   1. **[新しいパスワード]** の値を書き留めます。 
   2. **[完了]** をクリックします。   

### <a name="create-a-weekdone-test-user"></a>Weekdone テスト ユーザーの作成
このセクションの目的は、Weekdone で Britta Simon というユーザーを作成することです。 Weekdone では、Just-In-Time プロビジョニングがサポートされています。この設定は、既定で有効になっています。

このセクションでは、ユーザー側で必要な操作はありません。 存在しない Weekdone ユーザーにアクセスしようとすると、新しいユーザーが自動的に作成されます。 [Azure AD シングル サインオンの構成](#configuring-azure-ad-single-single-sign-on)

>[!NOTE]
>ユーザーを手動で作成する必要がある場合は、Weekdone のサポート チーム (hello@weekdone.com) にお問い合わせください。
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て
このセクションの目的は、Britta Simon に Weekdone へのアクセスを許可することによって、このユーザーが Azure の SSO を使用できるようにすることです。

![ユーザーの割り当て][200] 

**Weekdone に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure クラシック ポータルでアプリケーション ビューを開くために、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
   
    ![ユーザーの割り当て][201] 
2. アプリケーションの一覧で **[Weekdone]**を選択します。
   
    ![[シングル サインオンの構成]](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_50.png) 
3. 上部のメニューで **[ユーザー]**をクリックします。
   
    ![ユーザーの割り当て][203] 
4. ユーザーの一覧で **[Britta Simon]**を選択します。
5. 下部にあるツール バーで **[割り当て]**をクリックします。
   
    ![ユーザーの割り当て][205]

### <a name="test-single-sign-on"></a>シングル サインオンのテスト
このセクションの目的は、アクセス パネルを使用して Azure AD の SSO 構成をテストすることです。

アクセス パネルで [Weekdone] タイルをクリックすると、自動的に Weekdone アプリケーションにサインオンします。

## <a name="additional-resources"></a>その他のリソース
* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_205.png

