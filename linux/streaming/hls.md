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
* ffmpegで配信できるっぽい
* 参考サイト候補
  * http://blog.livedoor.jp/linuxer2006/archives/65936430.html
    * 具体的だがrpm+apache
  * https://www.yaz.co.jp/tec-blog/web%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9/212
    * appleツール使用前提
    * .m3u8ファイルの仕様が詳細

## man ffmpeg

* オプション指定は個々の入出力先指定の前で行う
* 

## 配信試し

* mkdir video
* ffmpeg -re -i input.mp4 -bsf:v h264_mp4toannexb -c copy -f hls -hls_list_size 0 -flags +global_header video/video.m3u8
  * input.mp4は配信する.mp4ファイルのパスに置き換え
  * videoディレクトリ配下に.tsファイルが順次生成されるのみ
    * これを配信するAPIとかどうすれば良いのか確認
* オプション
  * -re ファイルから配信
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
    