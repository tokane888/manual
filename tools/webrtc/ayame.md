# ayame

* 参考) https://gist.github.com/voluntas/90cc9686a11de2f1acca845c6278a824
* 時雨堂のsignaling server
* サインアップ不要で簡単に使用可能なayame laboもある
* 制約
  * 1 roomの参加ユーザー数上限が2名
    * 3名以上の場合はP2Pではなく、SFU, MCUを使うべきとのこと
  * シグナリング方式が独自

## ayame labo使用方法

* momoからsignaling server指定で映像配信
  * ./momo --no-audio-device ayame wss://ayame-labo.shiguredo.jp/signaling hoge
    * 末尾の引数がroom ID
* ブラウザから視聴
  * https://openayame.github.io/ayame-web-sdk-samples/recvonly.html?roomId=hoge
    * roomId=(room ID) はmomoの引数と同じ物を指定

