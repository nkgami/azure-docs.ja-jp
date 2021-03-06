---
title: Enterprise セキュリティ パッケージを使用した Azure HDInsight アーキテクチャ
description: Enterprise セキュリティ パッケージを使用して HDInsight のセキュリティを計画する方法について説明します。
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 975a4f7b15d1e1c13767cd7026e961e9d4227603
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998929"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>HDInsight で Enterprise セキュリティ パッケージを使用する

標準的な Azure HDInsight クラスターは、シングル ユーザー クラスターです。 これは、大規模なデータ ワークロードを構築する小規模なアプリケーション チームを抱えているほとんどの企業に適しています。 ユーザーごとにオンデマンドの専用クラスターを作成し、不要になったときに破棄することができます。 

多くの企業では、IT チームが管理するクラスターを複数のアプリケーション チームが共有するモデルに移行しています。 これらの大規模企業は、Azure HDInsight での各クラスターへのマルチユーザー アクセスが必要です。

HDInsight は、管理されている方法で、一般的な ID プロバイダーである Active Directory に依存します。 HDInsight を [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md) と統合することによって、ドメイン資格情報を使用してクラスターにアクセスできます。 

HDInsight での仮想マシン (VM) は、指定されたドメインに参加しているドメインです。 そのため、HDInsight で実行されているすべてのサービス (Ambari、Hive サーバー、Ranger、Spark Thrift サーバーなど) は、認証済みユーザーに対してシームレスに動作します。 さらに、管理者は、Apache Ranger を使用して強力な承認ポリシーを作成し、クラスター内のリソースに対するロールベースのアクセス制御を行うことができます。


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight を Active Directory と統合する

オープンソースの Hadoop は、認証とセキュリティのために Kerberos に依存しています。 したがって、Enterprise セキュリティ パッケージ (ESP) を使用する HDInsight クラスター ノードは、Azure AD DS によって管理されるドメインに参加済みです。 クラスター上の Hadoop コンポーネントには Kerberos セキュリティが構成されます。 

Hadoop コンポーネントごとに、サービス プリンシパルが自動的に作成されます。 また、ドメインに参加する各マシンには、対応するマシン プリンシパルも作成されます。 これらのサービスやマシン プリンシパルを格納するには、これらのプリンシパルが配置されているドメイン コントローラー (Azure AD DS) 内の組織単位 (OU) を用意する必要があります。 

これらの点を総合すると、次の要素から成る環境をセットアップする必要があります。

- Active Directory ドメイン (Azure AD DS で管理)。
- Azure AD DS で有効になっている Secure LDAP (LDAPS)。
- 個別の仮想ネットワークを選ぶ場合の HDInsight 仮想ネットワークから Azure AD DS 仮想ネットワークへの適切なネットワーク接続。 HDInsight 仮想ネットワーク内の VM は、仮想ネットワーク ピアリングを使用して Azure AD DS への通信経路を持つ必要があります。 HDInsight と Azure AD DS が同じ仮想ネットワーク内にデプロイされている場合、接続は自動的に提供されるので、これ以上アクションは必要ありません。
- [Azure AD DS で作成された](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) OU。
- 次の操作に必要なアクセス許可を持つサービス アカウント:
    - OU にサービス プリンシパルを作成する。
    - ドメインにマシンを参加させて OU にマシン プリンシパルを作成する。

次のスクリーンショットは、contoso.com に作成された OU を示したものです。 これは、サービス プリンシパルとマシン プリンシパルの一部も表示します。

![ESP を使用する HDInsight クラスターの組織単位](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>異なるドメイン コントローラーの設定
現在、HDInsight では、クラスターの Kerberos 認証に使用するメイン ドメイン コントローラーとして、Azure AD DS だけがサポートされています。 ただし、設定によって HDInsight アクセスに対する Azure AD DS が有効にされることになる場合に限り、その他の複雑な Active Directory 設定が可能です。

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) では、Windows Server Active Directory と完全に互換性のあるマネージド ドメインを提供します。 Microsoft は、高可用性 (HA) 設定のドメインの管理、修正プログラムの適用、監視を行います。 ドメイン コントローラーの管理について心配することなく、クラスターをデプロイすることができます。 

ユーザー、グループ、パスワードは、Azure Active Directory (Azure AD) から同期されます。 Azure AD インスタンスから Azure AD DS への一方向の同期により、同じ会社の資格情報を使用して、ユーザーがクラスターにサインインできます。 

詳細については、[Azure AD DS を使用する ESP への HDInsight クラスター構成](./apache-domain-joined-configure-using-azure-adds.md)に関する記事を参照してください。

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>オンプレミス Active Directory または IaaS VM 上の Active Directory

自分のドメインにオンプレミス Active Directory インスタンスまたはより複雑な Active Directory 設定がある場合、Azure AD Connect を使用してそれらの ID を Azure AD と同期することができます。 その後、Active Directory テナント上で Azure AD DS を有効にできます。 

Kerberos はパスワード ハッシュに依存するため、[Azure AD DS でパスワード ハッシュ同期を有効にする](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)必要があります。 Active Directory フェデレーション サービス (AD FS) でフェデレーションを使用していると、AD FS インフラストラクチャで障害が発生した場合のバックアップとして、オプションでパスワード ハッシュ同期を設定できます。 詳細については、「[Azure AD Connect 同期を使用したパスワード ハッシュ同期の実装](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)」を参照してください。 

Azure AD と Azure AD DS を使用せずに、IaaS VM 単独でオンプレミスの Azure AD または Azure AD DS を使用する構成は、ESP を使用する HDInsight クラスターではサポートされていません。

## <a name="next-steps"></a>次の手順

* [ESP を使用する HDInsight クラスターを構成する](apache-domain-joined-configure-using-azure-adds.md)
* [ESP を使用する HDInsight クラスターのために Hive ポリシーを構成する](apache-domain-joined-run-hive.md)
* [ESP を使用する HDInsight クラスターを管理する](apache-domain-joined-manage.md) 
