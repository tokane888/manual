# find

* カレントディレクトリ配下の特定拡張子のファイルを探す
  * find . -type f -name "*.html"
* 特定ディレクトリを無視
  * -path /proc -prune
  * 例) find / -path /proc -prune -type f -name "dnsmasq.co
  