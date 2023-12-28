# 4GPI

- 下記に従って構築
  - OS + パッケージ導入
    - https://github.com/mechatrax/4gpi/wiki/%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2#1-%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97
  - network設定
    - https://github.com/mechatrax/4gpi/wiki/%E3%81%9D%E3%81%AE%E4%BB%96#%E6%8E%A5%E7%B6%9A%E8%A8%AD%E5%AE%9A
- その他参考リンク
  - IIJパス
    - https://www.iijmio.jp/hdd/guide/apn.html
- IIJの場合のnmcliコマンド
  - nmcli con add type gsm ifname "*" con-name iijmio apn iijmio.jp user mio@iij password iij
