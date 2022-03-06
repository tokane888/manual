# ffmpeg

## 配信を録画

* ffmpeg -i (配信url) -c copy output.mp4
  * .mp4等拡張子は自由に設定
  * 下記のようなログが出るが、映像劣化はあるものの、映像が壊れているわけではない
    ```
    [rtsp @ 0x564506b428c0] RTP: missed 21 packets
    [rtsp @ 0x564506b428c0] max delay reached. need to consume packet
    ```

## 入力オプション

* 出力オプションを含む全オプションを確認
  * ffmpeg --help full | less

## 出力オプション

* -t 00:00:30
  * 動画開始時刻から指定時間切り取り
* -crf
  * constant rate factor
  * 映像品質指定
    * -qpで指定すると動きのない部分でもサイズが下がらないが、crfだと自動で調整される
* -preset
  * エンコードの速度と圧縮率に影響するオプション
  * ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow
    * 速度は名前順になるが、圧縮率は必ずしもそうならないとのこと
  * デフォルトはmedium
  * veryfastが圧縮率が良いとのこと

## エンコード時間

### h.264 => h.265

* 対象元動画
  * big buck bunny 1080p avi
    * curl -LO https://download.blender.org/peach/bigbuckbunny_movies/big_buck_bunny_1080p_stereo.avi
* 劣化をほぼ無くす
  * ffmpeg -i in.mp4 -c:a copy -c:v hevc_nvenc -crf 18 out.mp4

## 特定ケース

* 30fps映像を受信し、5秒間隔の、頭にキーフレームを含む.tsファイルに分割
  * ffmpeg -rtsp_transport tcp -stimeout 1000000 -i rtsp://192.168.11.1/video -f hls  -hls_time 5 -g 150 -keyint_min 150 -sc_threshold 0 out.m3u8
    * -hls_time
      * .tsファイルを分割する秒数(推奨値)
    * -g
      * Group Of Pictureに含まれるフレーム数
        * キーフレーム以外も含む
        * ここでは30fpsの映像を5秒で切りたいので、フレーム数は30*5=150
      * なおGroup Of Pictureには最低1つのキーフレームが含まれる
    * -keyint_min
      * 最低キーフレーム間隔
      * ここでは5秒(150フレーム)毎に映像を切るように設定
    * -sc_threshold
      * シーン変更時？などに自動的ににキーフレームを挿入されることを禁止

## mp3編集

* mp3を2回繰り返すmp3を作成
  * ffmpeg -stream_loop 1 -fflags +genpts -i kisho.mp3 -c copy kisho2.mp3
    * genptsは、mp3を接続したあとでptsを振り直している