---
ID: 3a0b09418372629bfd37
Title: archLinuxにRictyフォントをインストールしてて詰まったこと
Tags: font,archLinux
Author: Kamada Kouki
Private: false
---

archLinuxにRictyフォントがコマンド一発でインストールできると聞いて、インストールしようとしたら詰まった話
# yay -S で権限がない
```shell
$ yay -S XXX
Permission denied
```
↑のコマンドで、何やらよくわからないが、権限がないと怒られた。
~/.config/yay/config.json
にかかれているビルドパスの権限を持ってないと、だめらしい
自分の環境では、yayフォルダがroot所有になっていたり、そもそもconfig.jsonが作られていなかった。
デフォルトのビルドパスは、
~/.cache/yay
らしい、案の定root所有になっていたので、
```shell
$ sudo chown -R $(id -u):$(id -g) ~/.cache
```
で所有者を直したら行けた
参考：https://www.reddit.com/r/archlinux/comments/aa4tga/yay_syu_permission_denied/

# yay -S ttf-ricty で403 forbidden
Rictyフォントは、ライセンスの問題で、フォントを作り出すシェルスクリプトが公開されている。そのシェルスクリプトのもともとおいてあった場所が、なくなってしまった模様。
ただ、別の場所で新しく公開されているので、PKGBUILDを編集する必要がある。

```shell
$ yay -S ttf-ricty --editmenu
```

で編集するタイミングでAを入力して、
`http://www.rs.tus.ac.jp/yyusa/ricty/ricty_generator-4.1.1.sh`を
`http://www.yusa.lab.uec.ac.jp/~yusa/ricty/ricty_generator-4.1.1.sh`に書き換えてインストールをすると正常にインストールすることができた

参考：https://aur.archlinux.org/packages/ttf-ricty/
10月21日時点でコメントされているので、近いうちに治るかもしれない？
