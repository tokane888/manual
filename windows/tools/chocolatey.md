# chocolatey

## 基本コマンド

* パッケージinstall
  * cinst --yes (package名)
* 全パッケージ更新
  * choco upgrade all --yes
    * updateは同じ処理のコマンドだが、deprecated
* インストール済みのパッケージリスト取得
  * clist -lo
* パッケージuninstall
  * cuninst (package名)
* パッケージ検索
  * clist (package名)

## 補足

* cache-clearは有料版の機能
  * https://github.com/chocolatey/choco/issues/1124#issuecomment-352315426
  * choco-cleanパッケージの下記コマンドでclear可能
    * choco-cleaner.bat
      * 上記はデフォルトでも日曜23時に自動実行される
