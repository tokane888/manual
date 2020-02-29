## kubernetes環境構築

### 動確した環境

Ubuntu 18.04

### 仮想化がサポートされていることを確認

* 下記を実行し、出力が空でないことを確認
  * grep -E --color 'vmx|svm' /proc/cpuinfo

### Kubectl導入(debパッケージ)

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

### (参考)Kubectl導入(実行ファイルdownload)

* 基本的にはdebパッケージで導入した方が恐らく良い
  * update、削除等楽なので
  * kubectlのバージョンも同じだった
* コピペ用(最新version download & install)
  ```
  curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
  chmod +x ./kubectl
  sudo mv ./kubectl /usr/local/bin/kubectl
  kubectl version --client
  ```
* 参考リンク
  * https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux
* 注意点
  * clusterと、kubectlのバージョンはminor version 1以上の差が無いのが[公式推奨](https://kubernetes.io/docs/tasks/tools/install-kubectl/#before-you-begin)
* kubectlダウンロード
  * curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
    * 下記コマンドで最新バージョン取得してる
      * curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt
        * バージョン固定する場合は上記コマンド部分を固定値に置き換え
* 実行権限付与
  * chmod +x ./kubectl
* PATHに含まれる場所へ移動
  * sudo mv ./kubectl /usr/local/bin/kubectl
* バージョン確認
  * kubectl version --client

### (参考)[Kubectl設定確認](https://kubernetes.io/docs/tasks/tools/install-kubectl/#verifying-kubectl-configuration)

* clusterを作成するか、Minikubeインストールすると、下記に設定ファイルができる
  * ~/.kube/config
     * cluster作成は[kube-up.sh](https://github.com/kubernetes/kubernetes/blob/master/cluster/kube-up.sh)で可能
* (clusterの状態取得できれば正常に動作している)
  * kubectl cluster-info

### (参考)bash補完

kubectl completion bash >/etc/bash_completion.d/kubectl

### Minikube導入

* バージョン一覧を参照し、ダウンロードするバーションを決定
  * https://github.com/kubernetes/minikube/releases
* 上記からダウンロードダウンロードリンクを取得し、ダウンロード
  * wget https://github.com/kubernetes/minikube/releases/download/v1.7.3/minikube_1.7.3-0_amd64.deb
* インストール
  * apt install -y ./minikube_1.7.3-0_amd64.deb
      * dpkg -iでも良いが、aptに履歴をまとめて持たせたい