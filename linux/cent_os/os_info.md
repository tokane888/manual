## CentOSの情報取得

* OSのメジャーバージョン等取得
    * cat /etc/os-release
        ```
        # cat /etc/os-release
        NAME="CentOS Linux"
        VERSION="7 (Core)"
        ID="centos"
        ID_LIKE="rhel fedora"
        VERSION_ID="7"
        PRETTY_NAME="CentOS Linux 7 (Core)"
        ANSI_COLOR="0;31"
        CPE_NAME="cpe:/o:centos:centos:7"
        HOME_URL="https://www.centos.org/"
        BUG_REPORT_URL="https://bugs.centos.org/"

        CENTOS_MANTISBT_PROJECT="CentOS-7"
        CENTOS_MANTISBT_PROJECT_VERSION="7"
        REDHAT_SUPPORT_PRODUCT="centos"
        REDHAT_SUPPORT_PRODUCT_VERSION="7"
        ```
    * 他のOSでも基本的に同様に取得可能
* OSのマイナーバージョン取得
    * cat /etc/centos-release
        ```
        # cat /etc/centos-release
        CentOS Linux release 7.5.1804 (Core)
        ```
* Linux kernel version取得
    * uname -r
    ```
    # uname -r
    3.10.0-862.el7.x86_64
    ```
    * `3.10.0-862`の部分がLinux kernel version