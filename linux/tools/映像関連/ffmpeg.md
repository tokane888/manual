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
