## Windows Subsystem Linux

### コマンド

* カレントディレクトリでエクスプローラを開く
  * explorer.exe .
* クリップボードへコピー
  * echo hoge | clip.exe

### 初期設定作業

* ctrl+shift+(c/v)でコピペ可能にする
    * ウィンドウのヘッダ上で右クリック=>Properties押下
    * Optionsタブ選択 => "Use Ctrl+Shift+C/V as Copy/Paste"
    * OK押下
* 日本語入力可能に
    * 通常のubuntuと同じ。locale設定で対応
        * ConEmu上のubuntuは問題ないが、Ubuntuを直接起動すると日本語が文字化け
            * TODO: 対策検討
* ConEmuで起動可能に
    * 起動方法
        * ConEmuヘッダの'+'アイコンの横の下矢印押下
        * "New Console Dialog"押下
        * "新しいコマンド又または{タスク}名"から`{Bash::Git bash}`選択
    * 起動しない場合の手順
        * Settings -> Startup -> Tasks の Predefined tasks に新しいタスクを作成して “C:\Windows\System32\bash.exe ~” を設定
* visual studio codeのデフォルトのshellに設定
    * ctrl+@ 押下
    * コンソール右上からプルダウン押下
    * "Select Default Shell"選択
    * "WSL Bash"選択
        * 一度選択すると、プルダウンに起動するコンソールの候補として表示されるようになる
* ssh用のid_rsa鍵とconfigを~/.ssh配下にコピーして権限設定
    * windowsのユーザーの$HOMEディレクトリは見に行かない
        * windowsのディレクトリでは権限設定(600)ができないので見に行ってもssh接続時にエラーになる

### リセット方法

* Win+i押下
* Apps押下
* Ubuntu押下
* "Advanced options"押下
* Reset押下
* (option)昔MSが実験的に出していたbash on ubuntu on windowsを消す場合
    * [stack over flowの手順](https://superuser.com/questions/1261110/is-it-possible-to-uninstall-bash-on-ubuntu-on-windows-since-the-latest-updates#answer-1389786)に従う
