---
title: マネージド ドメイン用のハイブリッド Azure Active Directory 参加の構成 | Microsoft Docs
description: マネージド ドメイン用のハイブリッド Azure Active Directory 参加を構成する方法について説明します。
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcd00972c2da0d3d5dafe76a8619e0f0ccaedc19
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "79222989"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>チュートリアル:マネージド ドメイン用のハイブリッド Azure Active Directory 参加の構成

このチュートリアルでは、Active Directory ドメイン参加済みデバイスのハイブリッド Azure Active Directory (Azure AD) 参加を構成する方法について説明します。 この方法では、オンプレミスの Active Directory と Azure AD の両方を含むマネージド環境がサポートされます。

組織内のユーザーと同様、デバイスは保護対象となる主要なアイデンティティです。 デバイスの ID を使用して、いつでもどこからでもリソースを保護できます。 この目標は、Azure AD でデバイスの ID を管理することで達成できます。 以下のいずれかの方法を使用します。

- Azure AD 参加
- ハイブリッド Azure AD 参加
- Azure AD の登録

この記事では、Hybrid Azure AD Join に重点を置いて説明します。

Azure AD に自分のデバイスを取り込んで、クラウドとオンプレミスのリソースでのシングル サインオン (SSO) を実現することで、ユーザーの生産性を最大化できます。 同時に、[条件付きアクセス](../active-directory-conditional-access-azure-portal.md)を使用して、クラウドとオンプレミスのリソースへのアクセスを保護できます。

マネージド環境は、[パスワード ハッシュ同期 (PHS)](../hybrid/whatis-phs.md) または[パススルー認証 (PTA)](../hybrid/how-to-connect-pta.md) の[シームレス シングル サインオン](../hybrid/how-to-connect-sso.md)を使用してデプロイできます。 これらのシナリオでは、フェデレーション サーバーを認証用に構成する必要はありません。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * ハイブリッド Azure AD 参加の構成
> * ダウンレベルの Windows デバイスの有効化
> * 参加済みデバイスの確認
> * トラブルシューティング

## <a name="prerequisites"></a>前提条件

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 以降)
- Azure AD テナントの全体管理者の資格情報
- 各フォレストのエンタープライズ管理者の資格情報

以下の記事の内容を確認します。

- [デバイス ID とは](overview.md)
- [方法: ハイブリッド Azure Active Directory 参加の実装の計画](hybrid-azuread-join-plan.md)
- [ハイブリッド Azure AD 参加の検証を制御する](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD は、マネージド ドメインでのスマートカードや証明書をサポートしていません。

Azure AD Connect で、ハイブリッド Azure AD 参加済みにするデバイスのコンピュータ オブジェクトを Azure AD に対して同期済みであることを確認します。 コンピューター オブジェクトが特定の組織単位 (OU) に属している場合は、これらの OU を Azure AD Connect で同期するように構成します。 Azure AD Connect を使用してコンピューター オブジェクトを同期する方法の詳細については、「[組織単位ベースのフィルター処理](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering)」を参照してください。

バージョン 1.1.819.0 以降の Azure AD Connect には、Hybrid Azure AD Join を構成するためのウィザードが用意されています。 このウィザードを使用すると、構成プロセスを大幅に簡略化できます。 このウィザードで、デバイス登録のためのサービス接続ポイント (SCP) を構成します。

この記事の構成手順は、Azure AD Connect でこのウィザードを使用することに基づいています。

ハイブリッド Azure AD 参加では、デバイスが組織のネットワーク内から次の Microsoft リソースにアクセスできる必要があります。  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (シームレス SSO を使用しているか、使用する予定の場合)

組織がアウトバウンド プロキシ経由でのインターネットへのアクセスを必要とする場合は、Windows 10 コンピューターを Azure AD にデバイス登録できるように、[Web プロキシ自動発見 (WPAD) を実装](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10))することを推奨します。 WPAD の構成と管理の問題に対処するには、「[自動検出のトラブルシューティング](/previous-versions/tn-archive/cc302643(v=technet.10))」を参照してください。

WPAD を使用しない場合は、Windows 10 1709 以降のコンピューター上でプロキシ設定を構成できます。 詳細については、「[GPO によってデプロイされる WinHTTP プロキシ設定](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/)」を参照してください。

> [!NOTE]
> WinHTTP 設定を使用して自分のコンピューター上でプロキシ設定を構成すると、構成されたプロキシに接続できないコンピューターは、インターネットに接続できなくなります。

組織が認証されたアウトバウンド プロキシ経由でのインターネットへのアクセスを必要とする場合、お使いの Windows 10 コンピューターがアウトバウンド プロキシに対して正常に認証されることを確認してください。 Windows 10 コンピューターではマシン コンテキストを使用してデバイス登録が実行されるため、マシン コンテキストを使用してアウトバウンド プロキシ認証を構成します。 構成要件については、送信プロキシ プロバイダーに確認してください。

[デバイス登録接続のテスト](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0)のスクリプトを使用して、デバイスがシステム アカウントで上記の Microsoft リソースにアクセスできることを確認します。

## <a name="configure-hybrid-azure-ad-join"></a>ハイブリッド Azure AD 参加の構成

Azure AD Connect を使用して Hybrid Azure AD Join を構成するには:

1. Azure AD Connect を起動し、 **[構成]** を選択します。

   ![ようこそ](./media/hybrid-azuread-join-managed-domains/welcome-azure-ad-connect.png)

1. **[追加のタスク]** で、 **[デバイス オプションの構成]** を選択し、 **[次へ]** を選択します。

   ![追加のタスク](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-additional-tasks.png)

1. **[概要]** で **[次へ]** を選択します。

   ![概要](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-overview.png)

1. **[Azure AD に接続]** で、Azure AD テナントの全体管理者の資格情報を入力します。  

   ![Azure への接続](./media/hybrid-azuread-join-managed-domains/connect-to-azure-ad-username-password.png)

1. **[デバイス オプション]** で、 **[ハイブリッド Azure AD 参加の構成]** を選択し、 **[次へ]** を選択します。

   ![デバイス オプション](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-options.png)

1. **[SCP の構成]** で、Azure AD Connect で SCP を構成するフォレストごとに次の手順を実行し、 **[次へ]** を選択します。

   1. **フォレスト**を選択します。
   1. **認証サービス**を選択します。
   1. **[追加]** を選択して、エンタープライズ管理者の資格情報を入力します。

   ![SCP](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-scp-configuration.png)

1. **[デバイスのオペレーティング システム]** で、Active Directory 環境内のデバイスで使用されているオペレーティング システムを選択し、 **[次へ]** を選択します。

   ![デバイスのオペレーティング システム](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-operating-systems.png)

1. **[構成の準備完了]** で、 **[構成]** を選択します。

   ![構成の準備完了](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-ready-to-configure.png)

1. **[構成が完了しました]** で、 **[終了]** を選択します。

   ![構成の完了](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-configuration-complete.png)

## <a name="enable-windows-down-level-devices"></a>ダウンレベルの Windows デバイスの有効化

ドメイン参加済みデバイスの一部がダウンレベルの Windows デバイスである場合は、以下の操作が必要です。

- デバイスの登録用のローカル イントラネット設定の構成
- シームレス SSO の構成
- ダウンレベルの Windows コンピューターに対する Microsoft Workplace Join のインストール

> [!NOTE]
> Windows 7 のサポートは 2020 年 1 月 14 日に終了しました。 詳細については、[Windows 7 のサポートの終了](https://support.microsoft.com/help/4057281/windows-7-support-ended-on-january-14-2020)に関するページを参照してください。

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>デバイスの登録用のローカル イントラネット設定の構成

ダウンレベルの Windows デバイスの Hybrid Azure AD Join を完了するため、およびデバイスが Azure AD で認証を受けるときに証明書の指定を求めるメッセージが表示されないようにするために、ドメイン参加済みデバイスにポリシーをプッシュして、以下の URL を Internet Explorer のローカル イントラネット ゾーンに追加することができます。

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

また、ユーザーのローカル イントラネット ゾーンで **[スクリプトを介したステータス バーの更新を許可する]** を有効にする必要があります。

### <a name="configure-seamless-sso"></a>シームレス SSO の構成

Azure AD クラウド認証方法として[パスワード ハッシュ同期](../hybrid/whatis-phs.md)または[パススルー認証](../hybrid/how-to-connect-pta.md)を使用しているマネージド ドメインで、ダウンレベルの Windows デバイスの Hybrid Azure AD Join を完了するには、[シームレス SSO を構成](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature)する必要もあります。

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>ダウンレベルの Windows コンピューターに対する Microsoft Workplace Join のインストール

ダウンレベルの Windows デバイスを登録するには、組織で [Windows 10 以外のコンピューター向けの Microsoft Workplace Join](https://www.microsoft.com/download/details.aspx?id=53554) をインストールする必要があります。 Windows 10 以外のコンピューター向けの Microsoft Workplace Join は、Microsoft ダウンロード センターで入手できます。

 [Microsoft Endpoint Configuration Manager](/configmgr/) などのソフトウェア ディストリビューション システムを使用して、このパッケージをデプロイできます。 このパッケージは、`quiet` パラメーターを使用した標準のサイレント インストール オプションをサポートしています。 Configuration Manager の現在のバージョンには、完了した登録を追跡する機能など、以前のバージョンにはない利点が追加されています。

インストーラーによって、ユーザー コンテキストで実行されるシステムにスケジュール済みタスクが作成されます。 このタスクは、ユーザーが Windows にサインインするとトリガーされます。 このタスクでは、デバイスは Azure AD で認証が行われた後、そのユーザー資格情報を使用してサイレントに Azure AD に参加します。

## <a name="verify-the-registration"></a>登録の確認

**[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** を使用して、Azure テナントのデバイス登録状態を確認します。 このコマンドレットは、[Azure Active Directory PowerShell モジュール](/powershell/azure/install-msonlinev1?view=azureadps-2.0)内にあります。

**Get-MSolDevice** コマンドレットを使用してサービスの詳細を確認する場合:

- Windows クライアントの ID と一致する**デバイス ID** を備えたオブジェクトが存在する必要があります。
- **DeviceTrustType** の値は **[ドメイン参加済み]** です。 この設定は、Azure AD ポータルの **[デバイス]** ページの **[ハイブリッド Azure AD 参加済み]** 状態に相当します。
- 条件付きアクセスで使用されるデバイスの場合、**Enabled** の値は **True**、**DeviceTrustLevel** の値は **Managed** です。

サービスの詳細を確認するには:

1. Windows PowerShell を管理者として開きます。
1. 「`Connect-MsolService`」と入力して Azure テナントに接続します。  
1. 「`get-msoldevice -deviceId <deviceId>`」と入力します。
1. **[有効]** が **[True]** に設定されていることを確認します。

## <a name="troubleshoot-your-implementation"></a>実装のトラブルシューティング

ドメイン参加済み Windows デバイスの Hybrid Azure AD Join を行うときに問題が発生した場合は、以下を参照してください。

- [ハイブリッド Azure Active Directory 参加済みデバイスのトラブルシューティング](troubleshoot-hybrid-join-windows-current.md)
- [ハイブリッド Azure Active Directory 参加済みダウンレベル デバイスのトラブルシューティング](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>次のステップ

次の記事に進んで、Azure portal を使用してデバイス ID を管理する方法を学習します。
> [!div class="nextstepaction"]
> [デバイス ID を管理する](device-management-azure-portal.md)
