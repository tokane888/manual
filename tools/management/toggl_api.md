# toggl api

* 公式
  * https://github.com/toggl/toggl_api_docs/blob/master/toggl_api.md

## 制約

* 呼び出し頻度が高すぎると429エラー
  * 参考) https://github.com/toggl/toggl_api_docs/issues/78
  * 1秒に1回以下なら大丈夫っぽい

## 各種curl

* アカウント情報取得
  * curl -s -u (token):api_token -X GET https://api.track.toggl.com/api/v8/me
* 実行中のエントリ取得
  * curl -s -u (token):api_token -X GET https://api.track.toggl.com/api/v8/time_entries/current
    * 実行中のエントリなしの場合の出力
      ```
      {
        "data": null
      }
      ```
