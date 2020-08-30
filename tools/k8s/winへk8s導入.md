# windowsへのk8s導入手順

* 参考
  * https://minikube.sigs.k8s.io/docs/start/#what-youll-need
* 前提
  * プロダクション環境ではminikube, docker for windowsを用いたクラスタは使用しない
    * 基本的にlocal cluster用
    * プロダクションではkops, kubeadmなどを使用
      * AWS上ではkopsが最良

## minikube、kubectlを用いて環境構築

* minikube, kubectl導入手順
  * docker for windowsのメモリ割り当てを3.5GB以上に設定
    * minikube start時に、k8sが不安定になるとの警告が出るのを防ぐため
      * そのまま終了する場合もある
  * cinst --yes minikube kubernetes-cli
  * minikubeの割当メモリを3GBに設定
    * デフォルトは2GB。しかしこの設定だと警告が出る
    * minikube config set memory 3000
* minikube起動確認
  * minikube start
    * メモリ割り当てが2GB以下だと警告が出る
  * minikube status

## minikubeを用いない場合

* Docker for Windows をタスクトレイから右クリ => settings => Kubernetes => Enable Kubernetes
* 起動しているcontext取得
  * kubectl config get-contexts
  ```
  PS C:\Users\tom> kubectl config get-contexts
  CURRENT   NAME                 CLUSTER          AUTHINFO         NAMESPACE
  *         docker-desktop       docker-desktop   docker-desktop
            docker-for-desktop   docker-desktop   docker-desktop
            minikube             minikube         minikube
  ```
  * minikubeも使用している場合、下記で切り替え可能
    * kubectl config use-context minikube