# dns関連

* 127.0.0.53:53
  * systemdの一部として提供されているリゾルバ(resolved)
* dig +trace (host) が失敗する
  * resolvedはtraceをサポートしていない
    * issue: https://github.com/systemd/systemd/issues/5897
