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

* 1年経過で内部の証明書期限切れになり、kubectl等のコマンドを受け付けなくなる
  ```
  # kubectl get pods --all-namespaces
  Unable to connect to the server: x509: certificate has expired or is not yet valid
  ```
  * 証明書の期限は下記で確認可能
    * openssl s_client -connect localhost:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
      ```
      # openssl s_client -connect localhost:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
      notAfter=Mar  9 04:29:47 2022 GMT
      ```
  * contributerによると、openssl等のツールで直接ファイルを書き換える以外には恐らく期限延長などは不可とのこと
    * https://github.com/k3s-io/k3s/issues/1621#issuecomment-626115939
  * 関連する鍵は下記のパスにある
    * /var/lib/rancher/k3s/server/tls
