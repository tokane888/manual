# rtsp

## rtspで.mp4ファイルを配信

### UI(VLC Media Player)を使用する方法

* 参考) https://qiita.com/subretu/items/0db769b506bcd0148077
  * 手順に従って実行した場合の"生成されたストリーム出力文字列"
    * :sout=#transcode{vcodec=h264,acodec=mpga,ab=128,channels=2,samplerate=44100,scodec=none}:rtp{sdp=rtsp://:8554/rtsp} :no-sout-all :sout-keep
  * localhost:8554/rtsp
    * 表示できない
    

### コンソールを使用する方法

vlc *.mp4 --sout '#standard{access=http,mux=ogg,dst=localhost:8080}'

繰り返し配信

## 用語

* rtp: 音と映像をまとめてclientへ送信するプロトコル
* rtsp: streaming sessionを生成し、rtp配信の詳細設定を行うプロトコル
