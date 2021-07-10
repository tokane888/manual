# ffmpeg

## 配信を録画

* ffmpeg -i (配信url) -c copy output.mp4
  * .mp4等拡張子は自由に設定
  * 下記のようなログが出るが、映像劣化はあるものの、映像が壊れているわけではない
    ```
    [rtsp @ 0x564506b428c0] RTP: missed 21 packets
    [rtsp @ 0x564506b428c0] max delay reached. need to consume packet
    ```

## その他コマンド

* 映像ファイルのメタデータ確認
  * forever -i ***.mp4