# goデバッグ

## push前の個人repoの変更を反映

* 参考
  * https://stackoverflow.com/questions/39215629/importing-local-changes-of-a-package-without-pushing-code-in-golang
* go.mod末尾に下記記載
  * replace (repo URL) => (local repo path)
    * 例) replace gitlab.com/home_life_management/common_lib => /root/.gvm/pkgsets/go1.17/global/src/gitlab.com/home_life_management/common_lib
