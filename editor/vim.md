## vimコマンド

* プラグインインストール
    * コンソールからインストール
        * vim +PluginInstall +qall
    * vim上でインストール
        * :PluginInstall
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
