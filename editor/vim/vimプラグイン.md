## プラグイン

### 一覧

* ctrlpvim/ctrlp.vim
  * ctrl+pでファイルを開ける

### 関連コマンド

* プラグインインストール
    * コンソールからインストール
        * vim +PluginInstall +qall
    * vim上でインストール
        * :PluginInstall
* プラグイン削除
    * :PluginList => plugin選択してshift+d => :e ~/.vimrcで当該Pluginの記載削除

### NerdTree

* 新ファイル作成
  * ディレクトリ上でm => a

### go-vim

* 参考
    * vim-go-tutorial
        * https://github.com/fatih/vim-go-tutorial#go-to-definition
* 定義へjump
    * gd
* 直前の位置へ戻る
    * ctrl+t
        * vimの共通コマンドだとctrl+oだが、これは若干動きが微妙らしい
            * TODO: 詳細確認

## fzf

* 最近開いたファイルを検索
    * :History
* gitに含まれるファイルを検索
    * :GFiles
* commit検索
    * :Commits
