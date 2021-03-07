# ansible

## 特徴

* 設定ファイルがyamlで読みやすい
* agent less
  * 変更対象のサーバにagent install不要
  * 変更対象サーバにpythonは必要。通常はデフォルトで入っている
* 処理がモジュール化されている

## 注意点

* yamlなので複雑な条件分岐は困難
* 実行結果の完全性は担保できない
  * テストは実装？する必要がある
* SSHポート必須
  * sshがない環境へのupdateデプロイは困難
  * Connection pluginで、接続方法変更可能
    * dockerへの接続等も可能
* 各プロビジョニングレイヤーをどのツールが担当するかは決めが必要
* プロビジョニングレイヤ
  * Orchestration
    * アプリ実行環境構築+アプリデプロイ等
    * ツール: Fabric, Capistrano
  * Configuration Management
    * OS起動後の初期設定、ミドルウェアのインストール、設定等
    * ツール: Ansible, Chef, Puppet, SaltStack
  * Bootstrapping
    * OSインストール+OS起動し、利用可能な状態にするレイヤ
    * ツール: Terraform, Vagrant, Docker
  
## コントロールノード要件

* python2.7または3.5以上のインストール
* linux系OS
  * Windowsマシンはコントロールノードとして使用不可
* ターゲットノードへのssh接続が必要
* CPU、メモリに関しての公式要件はない
  * ターゲットノード側でタスクを実行するため
  * 仮想サーバでも良い
* デフォルト設定では/usr/bin/pythonにあるpythonを使用してモジュールを実行
  * ディストリビューションによっては/usr/bin/python3に配置されるため"module_stdout"エラーになる
    * 必要に応じてansible_python_interpreterでpython3の実行バイナリパスを設定

