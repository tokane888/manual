## 起動時に実行されるスクリプト

### スクリプト実行順序

#### StackExchangeより

* [参考](https://askubuntu.com/questions/438150/scripts-in-etc-profile-d-being-ignored)
* シェルの種類
  * terminal-emurator(gnome-terminal)使用時 => interactive, non-login shell
  * sshログイン時 => interactive, login shell
  * その他(graphical shell) => システムによる
* シェルスクリプトを実行すると、non-interactive, non-login shellで実行される
* 実行順序
  * OS起動時
    * /etc/rc.local
  * interactive, login shell又はnon-interactive shellを--login付きで実行時(ssh)
    1. /etc/profile
    2. 下記から最初に見つかったものを実行
      1. ~/.bash_profile
      2. ~/.bash_login
      3. ~/.profile 
  * interactive, non-login shell実行時(sudo su時)
    1. /etc/bash.bashrc
    2. ~/.bashrc

#### CentOSの場合

##### ssh接続時記録

1. /etc/profile
2. /etc/profile.d/*.sh
3. /etc/bashrc

##### sudo su時記録

1. /etc/bashrc
2. /etc/profile.d/*.sh

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
3. /etc/profile.d/*.sh

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