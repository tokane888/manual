## kubernetes環境構築

### 動確した環境

Ubuntu 18.04実機

### 仮想化がサポートされていることを確認

* 下記を実行し、出力が空でないことを確認
  * grep -E --color 'vmx|svm' /proc/cpuinfo
  * EC2上のUbuntuでは上記の出力が空になることを確認済み

### docker 導入

* 下記に従ってdocker導入
  * linux\tools\docker\docker導入.md

### Minikube導入

* 参考) https://minikube.sigs.k8s.io/docs/start/
* curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
* apt install -y ./minikube_latest_amd64.deb
* minikubeのdockerドライバはrootで使用できないが、dockerはrootでないと使用できないので対応
  * minikubeを使用するuserの権限でdocker使用可能に設定
    * sudo usermod -aG docker $USER && newgrp docker
* rootユーザー以外、dockerグループのuserで、下記実行でminikube起動可能に
  * minikube start
* 下記でminikubeの起動確認
  * minikube status

### kubectl導入(debパッケージ)

* リポジトリ追加+インストール
  * 途中xenialの文字があるが、ubuntu 18.04上でもインストール成功した
    * xenial => bionic に文字列置き換えたらパッケージ見つからず失敗した
  ```
  sudo apt-get update && sudo apt-get install -y apt-transport-https
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
  sudo apt-get update
  sudo apt-get install -y kubectl
  ```

### (参考)[Kubectl設定確認](https://kubernetes.io/docs/tasks/tools/install-kubectl/#verifying-kubectl-configuration)

* clusterを作成するか、Minikubeインストールすると、下記に設定ファイルができる
  * ~/.kube/config
     * cluster作成は[kube-up.sh](https://github.com/kubernetes/kubernetes/blob/master/cluster/kube-up.sh)で可能
* (clusterの状態取得できれば正常に動作している)
  * kubectl cluster-info

### (参考)bash補完

kubectl completion bash >/etc/bash_completion.d/kubectl

