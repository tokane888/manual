# vagrant導入

## 動確環境

- ubuntu 22.04

## 手順

- 参考
  - 公式
    - https://developer.hashicorp.com/vagrant/downloads
- vagrantインストール

    ```
    wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    sudo apt update && sudo apt install vagrant
    ```
- 試験的にbox install
  - vagrant box add hashicorp/bionic64
- Vagrantfile作成
  - vagrant init hashicorpd/bionic64
- (WSL2の場合)Vagrantfileに下記追記
  - config.vm.synced_folder '.', '/vagrant', disabled: true
- vagrant起動
  - vagrant up
- vagrantへ試験的にssh接続
  - vagrant ssh
- vagrant停止
  - vagrant halt
- vagrant停止 + 削除
  - vagrant destroy
    - halt実行せずこちらだけでもok

## オプション(プラグイン)

### vagrant-share導入

- vagrant plugin install vagrant-share
- 依存しているngrok導入
  - 参考) https://ngrok.com/download
  - curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
- このあとport forward周り等設定必要。あまり使わない気がするので未検証

## トラブルシューティング

- WSL2上で動かない
  - Vagrantfileに下記突っ込むと動くこともある

    ```
    config.vm.provider "virtualbox" do |vb|
      vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
    ```
    - 参考: https://github.com/hashicorp/vagrant/issues/8604#issuecomment-303259512
