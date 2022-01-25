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
