## Conemu

* 新規タブ作成
    * デフォルトではwin+shift+wがショートカット
        * 何故か動作しない。AHK, clibor, Phrase express等offにしてもだめだった
    * win+shift+a を新規タブ作成のショートカットに割り当てる方法
        * win+alt+p で設定画面表示
        * 左メニューから"キーとマクロ"選択
        * 右上検索欄に"win+shift"入力
        * win+shift+wに"新しいコンソールを作成する"が割り当てられているので、"win+shift+a"に変更
        * "設定を保存"
* 設定Export方法
    * 設定画面右上のexportボタン押下
    * 詳細はStack overflow参照
        * https://superuser.com/questions/450144/how-do-i-export-conemu-settings
* デフォルトのコンソールをubuntu on windowsに変更
    * win+alt+p で設定画面表示
    * 左メニューから"一般"選択
    * "あなたのスタートアップタスク..."から"{Bash::bash}"を選択
    * "設定を保存"
* コンソールサイズが正しく認識されない場合
    * checkwinsizeオプション確認
        * shopt | grep checkwinsize
    * checkwinsize on でない場合設定
        * shopt -s checkwinsize
    + 上記で直らない場合
        * ctrl+L で直るとの情報も
    * 他参考情報
        * https://unix.stackexchange.com/questions/61584/how-to-solve-the-issue-that-a-terminal-screen-is-messed-up-usually-after-a-res
* tmux, vimコンソール使用時に下のステータス表示が徐々にせり上がってくる問題
    * windows terminal使えば問題無し。ConEmuでの解決方法は不明