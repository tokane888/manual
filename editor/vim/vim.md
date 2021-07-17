# vim

## vim設定ファイル

* ユーザー設定
  * ~/.vimrc
* システム共通設定
  * /etc/vim/vimrc
* debian固有設定
  * 上記の/etc/vim/vimrc から呼び出される
    * $VIMRUNTIME/debian.vim
      * $VIMRUNTIME はvim起動中しか定義されないので、:terminal の中からのみ確認可能
  * vimのupgradeの度に上書きされるので基本的に編集しない
* デフォルト設定
  * $VIMRUNTIME/defaults.vim
    * $VIMRUNTIME はvim起動中しか定義されないので、:terminal の中からのみ確認可能
    * ユーザーの~/.vimrc がない場合に使用される
    * /etc/vim/vimrc の後で読み込まれる

## vimコマンド

* sudo権限でエディタ上から上書き保存
    * :w !sudo tee > /dev/null %
    * :w !sudo tee %
    * 解説
        * % = 今のファイルの名前
            * teeに渡される => teeが今のファイルへ書き込む
        * :w (バッファ送信先)
            * バッファ送信先がファイル名の場合、ファイルへ保存
            * :w !sudo のようにしてシェルにバッファを送信することも可能
                * 今回はteeコマンドへ送信している
* 置換
  * 全部置換
    * :%s/置換前/置換後/g
  * 最初の一致箇所だけ置換
    * :%s/置換前/置換後/
      * &で繰り返し
* visudoにvimを使用
  * sudo update-alternatives --set editor /usr/bin/vim.basic
* 最近開いたファイル
  * :old => 'q' => 一覧から番号入力で開く
* エクスプローラでカレントディレクトリを開く
  * :silent ! start .
* デフォルト設定使用
  * vim -u NONE -N
    * -u: 使用する.vimrcファイルを指定
    * -N: vi互換モードを使用しない
* 公式ガイド
  * :h vimtutor
* ファイル名表示
  * Ctrl+g
* 行内の1文字を前方から検索
  * f[文字]
    * ; で1つ先の一致箇所へ
    * , で1つ前の一致箇所へ
* 行内の1文字を後方から検索
  * F[文字]
* 直前の単語を消す
  * db
* 行が折り返し表示されている場合に、表示単位で上下の行に移動
  * ↑: gk
  * ↓: gj
* 履歴表示
  * "q:" => コマンド履歴
  * "q/" 又は "q?" => 検索履歴
* 分割
  * 上下分割でファイル開く
    * :sp
  * 左右分割でファイル開く
    * :vs
      * :set splitright 後に実行すると、右側に新ウィンドウができる
* 切り取り
  * 現在行から11行目まで
    * :.,11d
    * d11G
* 関数一覧表示
  * :g/^func
* file再読み込み
  * :e!
* インデントを保持してinsert modeへ
  * S
* tabとspaceによるインデントを設定どおりに一括変更
  * :retab

### vim terminal

* ターミナル開く
  * 上に配置
    * :terminal
  * 左に配置
    * :vert terminal
* normalモードに遷移
  * ctrl+w => shift+n
* yankした内容を貼り付け
  * ctrl+w => " => "

## トラブルシュート

* vim上で文字列をドラッグしてctrl+shift+cでclip boardにコピーできない
  * 下記を~/.vimrcに記載
    ```
    noremap <LeftDrag> <LeftMouse>
    noremap! <LeftDrag> <LeftMouse>
    ```