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
* 関数名変更
    * :GoRename (変更後の名前)
    * 変更前後の名前が同じと判定されて失敗する場合がある。恐らくバグ
* コメントを含む関数全体をコピー、削除
    * normalモードで"func"にカーソルを当てて下記実行
        * vaf: コメント含む関数全体を選択
        * daf: コメント含む関数全体を削除
        * yaf: コメント含む関数全体をコピー
* 関数のbodyをコピー、削除
    * normalモードで"func"にカーソルを当てて下記実行
        * dif: 関数のbodyを削除
        * yif: 関数のbodyをコピー

## fzf

* 最近開いたファイルを検索
    * :History
* gitに含まれるファイルを検索
    * :GFiles
* commit検索
    * :Commits

## rking/ag.vim

* 検索
    * :Ag (検索ワード)

## vim-fugitive

* 横に並べてdiff表示
    * :Gdiffsplit