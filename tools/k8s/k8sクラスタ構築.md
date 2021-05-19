# k8sクラスタ構築

* ローカルk8sではなく、複数台のPCへの環境構築を想定した手順
* 構築関連ツール
  * kubeadm
  * Flannel
    * ネットワーク構築
  * Rancher
    * OSSのコンテナプラットフォーム
    * 中央集権サーバとしてRancher serverを起動しておき、Rancher serverからk8sクラスタの構築、管理を実施