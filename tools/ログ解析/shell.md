# shell

## ログ解析

* 単一行内の特定文字列出現回数カウント
  * 例）tmp.txt内の"zip"の出現数カウント
    * grep -ow "zip" tmp.txt | wc -l
* 複数行を、space区切りの単一行に変換
  * cat source.txt | paste -sd' ' -
    * 参考) https://unix.stackexchange.com/questions/593993/convert-multi-lines-to-single-line-with-spaces-and-quotes