# desktop設定

## gsettings

- 設定項目全表示
  - for s in $(gsettings list-schemas); do gsettings list-recursively $s; done
- 現在の設定値全表示
  - for s in $(gsettings list-schemas); do gsettings list-recursively $s; done
    - GUIからの設定前後での出力値の変化を比較すればscript化可能

## gnome-shell-extensions

- 拡張検索
  - gnome-extensions-cli search (検索文字列)
- 
