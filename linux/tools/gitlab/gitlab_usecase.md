# gitlab use case

* sub group作成
  * group名表示の右辺りから"New subgroup"押下
    * 左上のmenuから作成するとsubgroupではなく完全新規のgroupになるため注意
* branch名変更(main => master)
  * gitlabの場合
    * localブランチ名変更
      * git branch -m main master
    * push
      * git push -u origin HEAD
    * gitlabデフォルトブランチ変更
      * gitlabをブラウザから開く
      * 左メニューの"Settings"=>"Repository"から変更
    * gitlabでmainブランチをprotectedブランチから削除
      * web左メニューの"Settings" => "Repository" => "Protected branches"からmainブランチをunprotected状態に
    * gitlabでmasterブランチをprotectedブランチに
      * web左メニューの"Settings" => "Repository" => "Protected branches"からmasterブランチをprotected状態に
    * localのmainブランチ削除
      * git branch -D main
    * remoteのmainブランチ削除
      * git push origin --delete main
