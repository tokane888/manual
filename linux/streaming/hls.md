# HTTP Live Streaming(HLS)

## 概要

* オンデマンド配信(VOD)とライブ配信(LIVE)に対応
  * mp3, mp4等も配信可能
* HTTPSによる暗号化とユーザ認証に対応
* 映像ストリーム、ファイルを下記に変換して配信
  * インデックスファイル
    * .m3u8
  * 動画を分割したセグメントファイル
    * .ts
* HLS2 RFS
  * https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis-07.html
* 公式仕様
  * 参考
    * https://developer.apple.com/documentation/http_live_streaming/hls_authoring_specification_for_apple_devices
    * https://ja.wikipedia.org/wiki/H.264#%E3%83%97%E3%83%AD%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%A8%E3%83%AC%E3%83%99%E3%83%AB
  * コーデック: H.264又はH.265
  * H.264のコンテナは.mp4又はmpeg transport stream
  * プロファイル
    * 目的用途別の機能の集合
  * レベル
    * 処理の負荷や仕様メモリ量
  * 互換性のため、一部のH.264はhigh profile level 4.1以下が望ましい
  * H.264ではmain, baseline profileよりhigh profileを使うのが望ましい
  * live/linearコンテンツ
    * 1時間以上の長期平均segment bit rateはAVERAGE-BANDWIDTHの110%以下でなくてはならない
    * ピーク時のbit rateはBANDWIDTHの125%以下でなくてはならない
  * liveとvodの違い
    * live: 常に新しい映像が追加で届く可能性がある
    * vod: 今又は将来のある時点で動画が終了
* 参考
  * https://ja.wikipedia.org/wiki/H.264#%E3%83%97%E3%83%AD%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%A8%E3%83%AC%E3%83%99%E3%83%AB

## 配信試し

* mkdir video
* ffmpeg -i (入力) -bsf:v h264_mp4toannexb -c copy -f hls -hls_list_size 0 -flags +global_header video/video.m3u8
  * videoディレクトリ配下に.tsファイルが順次生成されるのみ
    * これを配信するAPIとかどうすれば良いのか確認
* オプション
  * -i 入力.mp4ファイル指定
  * -bsf:v h264_mp4toannexb .tsファイル出力する際に指定
  * -c copy コーデックオプションでcopy modeを選択
  * -hls_list_size 0 マニュフェストに記載されるセグメント数。0はすべて
  * 
* videoディレクトリ中身
  * video.m3u8
    * マニュフェスト
      ```
      #EXTM3U
      #EXT-X-VERSION:3
      #EXT-X-TARGETDURATION:2
      #EXT-X-MEDIA-SEQUENCE:0
      #EXTINF:2.000000,
      video0.ts
      #EXTINF:2.000000,
      video1.ts
      #EXTINF:2.000000,
      video2.ts
      #EXTINF:2.000000,
      video3.ts
      #EXTINF:2.000000,
      video4.ts
      #EXTINF:2.000000,
      video5.ts
      #EXTINF:2.000000,
      video6.ts
      #EXTINF:2.000000,
      video7.ts
      #EXTINF:2.000000,
      video8.ts
      #EXTINF:2.000000,
      video9.ts
      #EXT-X-ENDLIST
      ```
      * EXTM3U: .m3u8ファイルであることを示す
      * EXT-X-VERSION: .m3u8ファイルのバージョン
      * EXT-X-TARGETDURATION: 作成される各.tsファイルの秒数の上限
      * EXT-X-MEDIA-SEQUENCE: 本リストの最初のtsファイルが動画全体の何番目のtsファイルかを示す
      * EXTINF: tsファイルの秒数とファイル名
      * EXT-X-ENDLIST: リストの終わり
  * video0.ts ~ video9.ts
    * 動画のセグメントファイル。恐らく数は動的
    