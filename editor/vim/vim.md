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
* visudoにvimを使用
  * sudo update-alternatives --set editor /usr/bin/vim.basic
* ターミナル開く
  * 上に配置
    * :terminal
  * 左に配置
    * :vert terminal
* 最近開いたファイル
  * :old => 'q' => 一覧から番号入力で開く
* エクスプローラでカレントディレクトリを開く
  * :silent ! start .
* デフォルト設定使用
  * vim -u NONE -N
* 公式ガイド
  * :h vimtutor
* ファイル名表示
  * Ctrl+g