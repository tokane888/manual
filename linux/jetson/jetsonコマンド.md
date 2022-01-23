# jetsonコマンド

* 温度計測
  * 下記で各パーツの温度を計測可能
    ```
    root@jet:~# cat /sys/devices/virtual/thermal/thermal_zone[0-9]/type
    AO-therm
    CPU-therm
    GPU-therm
    PLL-therm
    PMIC-Die
    thermal-fan-est
    root@jet:~# cat /sys/devices/virtual/thermal/thermal_zone[0-9]/temp
    36500
    27500
    29000
    27000
    50000
    28250
    ```
    * PMIC-Dieについては温度計測できないとの情報もあり
    