# tmux

- 参考
  - 公式getting started
    - https://github.com/tmux/tmux/wiki/Getting-Started
  - 公式clipboard
    - https://github.com/tmux/tmux/wiki/Clipboard

## 主要コマンド

* 検索
  * ctrl-b => [ => / or ? => (検索文字列) => enter
* 確認の上windowをkill
  * ctrl-b => &
* コマンドプロンプトでコマンド実行
  * ctrl-b => : => (コマンド)

## コマンドプロンプトのコマンド

* :list-keys
  * key-bind一覧表示

## ターミナル上のコマンド

* tmux lsk -Tcopy-mode-vi
  * copy-mode-viのkey-bind一覧表示
