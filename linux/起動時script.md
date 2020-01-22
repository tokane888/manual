## 起動時に実行されるスクリプト

### スクリプト実行順序

#### CentOSの場合

##### ssh接続時記録

1. /etc/profile
2. /etc/profile/*.sh
3. /etc/bashrc

##### sudo su時記録

1. /etc/bashrc
2. /etc/profile/*.sh

##### $HOME配下のスクリプト実行順序(ソース確認結果)

1. 以下を順に探し、最初に見つかったものを呼び出し
    1. ~/.bash_profile
        * ~/.bashrc呼び出し
            * ~/.bash_aliases呼び出し
    2. ~/.bash_login
        * bashのソース上の処理。実際は置かれてない
    3. ~/.profile
        * bashのソース上の処理。実際は置かれてない
            
#### Ubuntuの場合

##### ssh接続時記録

1. /etc/profile
2. /etc/bash.bashrc
3. /etc/profile/*.sh

##### sudo su時記録

1. /etc/bash.bashrc

##### $HOME配下のスクリプト実行順序(ソース確認結果)

1. 以下を順に探し、最初に見つかったものを呼び出し
    1. ~/.bash_profile
        * bashのソース上の処理。実際は置かれてない
    2. ~/.bash_login
        * bashのソース上の処理。実際は置かれてない
    3. ~/.profile
        * ~/.bashrc呼び出し
            * ~/.bash_aliases呼び出し