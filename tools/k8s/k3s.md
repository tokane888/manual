## k3s関連

### 動確した環境

Ubuntu 18.04

### インストール

curl -sfL https://get.k3s.io | sh -
* kubectl, crictl等も同時にインストールされる
* kubeconfigファイルは下記に配置される
  * /etc/rancher/k3s/k3s.yaml
* (option)worker node追加
  * curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -

### アンインストール

* 下記実行
  * /usr/local/bin/k3s-uninstall.sh

### 注意点

* 1年経過で内部の証明書期限切れ