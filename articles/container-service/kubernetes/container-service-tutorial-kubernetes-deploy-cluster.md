---
title: (非推奨) Azure Container Service チュートリアル - クラスターのデプロイ
description: Azure Container Service チュートリアル - クラスターのデプロイ
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b8821f3bb3d48786697cbc4137baf530856774fd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274002"
---
# <a name="deprecated-deploy-a-kubernetes-cluster-in-azure-container-service"></a>(非推奨) Azure Container Service で Kubernetes クラスターをデプロイする

> [!TIP]
> Azure Kubernetes Service を使用するこのチュートリアルの更新版については、「[チュートリアル: Azure Kubernetes Service (AKS) クラスターのデプロイ](../../aks/tutorial-kubernetes-deploy-cluster.md)」を参照してください。

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Kubernetes には、コンテナー化されたアプリケーション用の分散プラットフォームが用意されています。 Azure Container Service を使うと、運用開始準備の整った Kubernetes クラスターのプロビジョニングを簡単かつ迅速に行うことができます。 この 7 部構成の 3 番目のチュートリアルでは、Azure Container Service の Kubernetes クラスターをデプロイします。 手順は次のとおりです。

> [!div class="checklist"]
> * Kubernetes ACS クラスターのデプロイ
> * Kubernetes CLI (kubectl) のインストール
> * kubectl の構成

この後のチュートリアルでは、Azure Vote アプリケーションをクラスターにデプロイ、スケーリング、および更新します。さらに、Kubernetes クラスターを監視するように Log Analytics を構成します。

## <a name="before-you-begin"></a>開始する前に

これまでのチュートリアルでは、コンテナー イメージを作成して、Azure Container Registry インスタンスにアップロードしました。 これらの手順を実行していない場合で、行いたい場合は、「[チュートリアル 1 – コンテナー イメージを作成する](./container-service-tutorial-kubernetes-prepare-app.md)」に戻ってください。

## <a name="create-kubernetes-cluster"></a>Kubernetes クラスターを作成する

[az acs create](/cli/azure/acs#az-acs-create) コマンドを使用して Azure Container Service に Kubernetes クラスターを作成します。 

次の例では、`myK8sCluster` という名前のクラスターを `myResourceGroup` という名前のリソース グループに作成します。 このリソース グループは、[前のチュートリアル](./container-service-tutorial-kubernetes-prepare-acr.md)で作成しました。

```azurecli-interactive
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

制限付き試用版を使用する場合など、Azure サブスクリプションによって Azure リソースへのアクセスが制限される場合もあります。 使用可能なコア数が限られているためにデプロイが失敗した場合は、`--agent-count 1` を [az acs create](/cli/azure/acs#az-acs-create) コマンドに追加して、既定のエージェント数を減らします。 

数分してデプロイが完了すると、この ACS デプロイに関する情報が JSON 形式で表示されます。

## <a name="install-the-kubectl-cli"></a>kubectl CLI をインストールする

クライアント コンピューターから Kubernetes クラスターに接続するには、[kubectl](https://kubernetes.io/docs/user-guide/kubectl/) (Kubernetes コマンドライン クライアント) を使います。 

Azure Cloud Shell を使用している場合、kubectl は既にインストールされています。 ローカルにインストールする場合には、[az acs kubernetes install-cli](/cli/azure/acs/kubernetes) コマンドを使います。

Linux または macOS で実行している場合は、sudo で実行することが必要な場合があります。 Windows では、シェルが管理者として実行されていることを確認します。

```azurecli-interactive
az acs kubernetes install-cli 
```

Windows では、既定のインストールは *c:\program files (x86)\kubectl.exe* です。 Windows パスにこのファイルを追加することが必要な場合があります。 

## <a name="connect-with-kubectl"></a>kubectl を使用して接続する

Kubernetes クラスターに接続するように kubectl を構成するには、[az acs kubernetes get-credentials](/cli/azure/acs/kubernetes) コマンドを実行します。

```azurecli-interactive
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

クラスターへの接続を確認するには、[kubectl get nodes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) コマンドを実行します。

```console
kubectl get nodes
```

出力:

```output
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

チュートリアルが完了し、ACS Kubernetes クラスターはワークロードで使用できるようになりました。 後のチュートリアルでは、マルチコンテナー アプリケーションのクラスターへのデプロイ、スケールアウト、更新、監視を行います。

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、Azure Container Service の Kubernetes クラスターをデプロイしました。 次の手順を完了しました。

> [!div class="checklist"]
> * Kubernetes ACS クラスターのデプロイ
> * Kubernetes CLI (kubectl) のインストール
> * kubectl の構成

次のチュートリアルに進んで、クラスターでのアプリケーションの実行について学習してください。

> [!div class="nextstepaction"]
> [Kubernetes でアプリケーションをデプロイする](./container-service-tutorial-kubernetes-deploy-application.md)
