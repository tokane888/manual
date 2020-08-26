## docker

### 動確環境

ubuntu 18.04

### 最新版インストール

* docker, docker.io, docker-engineは古いバージョンのdockerなので削除
  * apt remove -y docker docker-engine docker.io containerd runc
* [参考リンク](https://docs.docker.com/install/linux/docker-ce/ubuntu#install-docker-engine---community-1)
  * バージョン指定してインストールしたい場合は上記参照
```
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

apt-get update -y
apt-get install -y docker-ce docker-ce-cli containerd.io
```

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