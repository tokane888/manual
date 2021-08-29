## docker

### ラズパイ上にインストールする場合

* curl -sSL https://get.docker.com | sh
* 一般ユーザーでもdocker使用可能に
  * usermod -a -G docker tom
  * sudo reboot
* 動作確認
  * sudo docker run hello-world

### 下記動確環境

ubuntu 18.04

### 最新版インストール

* 基本的に公式にしたがってインストール
  * https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
    * ubuntuにインストールする前提の手順なので注意
* docker, docker.io, docker-engineは古いバージョンのdockerなので削除
  * apt remove -y docker docker-engine docker.io containerd runc
* deb repository設定
  ```
  sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```
* docker engineインストール
  ```
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io
  ```
* 動作確認
  * sudo docker run hello-world
* 一般ユーザーでもdocker使用可能に
  * usermod -a -G docker tom
  * sudo reboot

### docker-compose導入

* 下記で最新版のバージョン番号確認
  * https://github.com/docker/compose/releases/
* 下記の(1.25.4)の部分を上記のバージョン番号に置き換えてdocker-composeインストール

sudo su
curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

* 下記を実行して、docker-composeの補完を有効化

```
curl -L https://raw.githubusercontent.com/docker/compose/$(docker-compose version --short)/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

* /etc/bash.bashrc の、bash_completion読み込み処理がコメントアウトされていたらコメントアウト解除して読み込み
* 導入後、ssh接続時に毎回"404: Not found"などと表示されるようになった場合
  * curlのレスポンスが404で、それがそのまま下記に書き込まれているので、削除して再試行
    * /etc/bash_completion.d/docker-compose