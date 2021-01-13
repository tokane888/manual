# shellでのログ解析

* 単一行内の特定文字列出現回数カウント
  * 例）tmp.txt内の"zip"の出現数カウント
    * grep -ow "zip" tmp.txt | wc -l