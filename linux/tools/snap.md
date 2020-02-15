## snap

* linuxのディストリビューション非依存のパッケージ管理ソフト
* 依存パッケージも含めてパッケージ化されており、独立性が高いとか
  * 依存問題に時間取られないのはありがたい
* go未インストールのPCで"go"と打ったらおすすめされた
  ```
  root@tom-PC-LZ550NSB:/home/tom/learn/k8s/sampleDockerfiles/HelloServer# go

  Command 'go' not found, but can be installed with:

  snap install go         # version 1.13.7, or
  apt  install golang-go
  apt  install gccgo-go
  ```
  * goのバージョンもaptより新しい
  * ubuntu公式のdocker imageには入っていなかった

### コマンド

* 利用可能なパッケージバージョン一覧取得
  * snap info (パッケージ名)
* パッケージインストール
  * snap install (パッケージ名)
    * sandboxの外に影響があるものは、警告が出るので --classic 付ける