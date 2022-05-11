# todoist cli

* 注意) 下記でインストールはしたが、データの同期ができていない
* 検証環境
  * WSL(Ubuntu 20.04)
* 公式
  * https://github.com/sachaos/todoist

## インストール

* 最新GOバイナリインストール
  * go関連の.md参照
* todoistインストール
  ```
  apt update -y
  apt install peco
  mkdir -p $GOPATH/src/github.com/sachaos
  cd $GOPATH/src/github.com/sachaos
  git clone https://github.com/sachaos/todoist.git
  cd todoist
  make install
  (.zshrcで上記ディレクトリ内のtodoist_functions.shをload)
  omz reload
  todoist sync
  (API token入力)
  todoist list
  ```
