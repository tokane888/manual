# todoist cli

* 注意) 下記でインストールはしたが、
* 検証環境
  * raspberry pi 3B+
  * OS: raspbian OS buster
* 公式
  * https://github.com/sachaos/todoist

## インストール

* dockerによるインストールは、archの違いにより失敗するため、下記の手順にしたがって自分でビルドする
* GOPATH通す
  * ~/.bashrc に下記記載
    ```
    export GOPATH=${HOME%/}/.go
    export PATH=$PATH:$GOPATH/bin
    ```
  * . ~/.bashrc
  * GOPATH作成
    * mkdir ~/.go
* 最新GOバイナリインストール
  * 公式によるとgo 1.12が必要とのことだが、aptには1.11までしかない(2020/7時点)
    * 公式) https://github.com/sachaos/todoist#build-it-yourself
  * raspbian OS公式以外のrepositoryにもarmに対応したgolangは見当たらないので、下記からバイナリ取得
    * https://golang.org/dl/
      * 検証時は1.16.6のARMv6使用
  * シンボリックリンク設定
    * ln -s /usr/lib/go-1.16.6/bin/go /usr/bin/go
    * ln -s /usr/lib/go-1.16.6 /usr/lib/go
* todoistインストール
  ```
  mkdir -p $GOPATH/src/github.com/sachaos
  cd $GOPATH/src/github.com/sachaos
  git clone https://github.com/sachaos/todoist.git
  cd todoist
  make install
  todoist
  (API token入力)
  ```
* 同期
  * todoist sync
    * TODO: ここで下記のエラーが詰まったので調査
      * Error: json: cannot unmarshal number 2213172342 into Go struct field .filters.id of type int
      