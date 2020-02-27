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