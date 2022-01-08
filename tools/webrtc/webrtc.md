# webrtc

* 参考
  * https://www.hiramine.com/programming/videochat_webrtc/index.html

## webrtc接続処理概要

* new RTCPeerConnection()
* RTCPeerConnectionにイベントハンドラ構築
* RTCPeerConnectionにメディアストリーム追加
* RTCPeerConnectionのcreateOffer()関数を呼び出し、SDP取得
* setLocalDescription()で上記SDOをローカルSDPとして設定
* ICE Candidateの収集が始まる
  * Interactive Connectivity Establishment candidate
    * 双方向接続確立候補
* ICE candidate収集が完了すると、ICE gathering state changeイベント発生
  * iceGatheringStateはcompleteに
* ICE candidate収集完了後、収集したICE candidateを保持するSDPをブラウザ画面のoffer側のofferSDP用のテキストエリアに貼り付け

## サンプル解析メモ

* サンプル
  * https://www.hiramine.com/programming/videochat_webrtc/03_manualsignaling_createoffersdp.html
* 関数
  * 