# DNS over TLS

* 参考
  * https://www.nic.ad.jp/ja/materials/iw/2019/proceedings/d3/d3-yamaguchi.pdf
* 機密性を高めたdns
  * android 9 以降では通常のdnsは指定不可
* rfc 7858(tls), rfc 8094(dtls)
* port
  * 853(tls, dtls)
    * 暗号化されていてもポートを見ればdtlsだということは分かる
