## docker

### Amazon linux 2023上にインストールする場合

sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
(OS再起動)

### ラズパイ上にインストールする場合

* curl -sSL https://get.docker.com | sh
* 一般ユーザーでもdocker使用可能に
  * usermod -a -G docker tom
  * sudo reboot
* 動作確認
  * sudo docker run hello-world

#### ラズパイにdocker-composeインストールする場合

* 動確環境
  * raspberry pi zero WH
    * OS: buster
* 下記から最新のarmv6のurl取得
  * https://github.com/docker/compose/releases/
* zero上でdownload
  * 例)
    * curl -L https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-armv6 -o /usr/local/bin/docker-compose
    * chmod +x /usr/local/bin/docker-compose

### 下記動確環境

ubuntu 18.04

### 最新版インストール

* 補足) wsl2上に、docker Desktopを使用しない、wsl2上でのみ動作するdockerを導入する場合も下記手順を使用可能
* 基本的に公式にしたがってインストール
  * https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
    * ubuntuにインストールする前提の手順なので注意
* docker, docker.io, docker-engineは古いバージョンのdockerなので削除
  * apt remove -y docker docker-engine docker.io containerd runc
* deb repository設定
  ```
  sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
  sudo install -m 0755 -d /etc/apt/keyrings
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  sudo chmod a+r /etc/apt/keyrings/docker.gpg
  echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```
* docker engineインストール
  ```
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  ```
* wsl2の場合service明示的に有効化
  * service docker start
    * 自動起動する場合は下記を.bashrcに記載
      ```
      service docker status > /dev/null
      if [ $? != 0 ]; then
        service docker start > /dev/null
      fi
      ```
* 動作確認
  * sudo docker run hello-world
* 一般ユーザーでもdocker使用可能に
  * sudo usermod -a -G docker tom
  * sudo reboot

### docker-compose導入

* 下記で最新版のバージョン番号確認
  * https://github.com/docker/compose/releases/
* 下記のバージョン番号部分を上記のバージョン番号に置き換えてdocker-composeインストール

sudo su
curl -L https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

* WSL2上に導入する場合はシンボリックリンク作成
  * ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

* 下記を実行して、docker-composeの補完を有効化
  * TODO: docker-compose ver 2以降ではcurlが404。恐らく補完未対応なので対応後にコマンド修正

```
curl -L https://raw.githubusercontent.com/docker/compose/$(docker-compose version --short)/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

* /etc/bash.bashrc の、bash_completion読み込み処理がコメントアウトされていたらコメントアウト解除して読み込み
* 導入後、ssh接続時に毎回"404: Not found"などと表示されるようになった場合
  * curlのレスポンスが404で、それがそのまま下記に書き込まれているので、削除して再試行
    * /etc/bash_completion.d/docker-compose
