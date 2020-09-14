---
ID: ab8f1556b8e7e2cbeae9
Title: Ubuntu16.04にVirtualBoxとVagrantをインストール
Tags: Ubuntu,Vagrant,VirtualBox,ubuntu16.04
Author: Kamada Kouki
Private: false
---

# 環境
Ubuntu 16.04
# VirtualBox
```bash
$ sudo vi /etc/apt/sources.list
## 追加
deb http://download.virtualbox.org/virtualbox/debian raring contrib

$ sudo wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
$ sudo apt update
$ sudo apt install virtualbox
```
# Vagrant
[公式サイト](https://www.vagrantup.com/downloads.html)からdebパッケージダウンロード
(aptでインストールする方法はない？)

```bash
$ mkdir ~/src
$ cd ~/src
$ wget https://releases.hashicorp.com/vagrant/2.1.2/vagrant_2.1.2_x86_64.deb
$ sudo dpkg -i vagrant_2.1.2_x86_64.deb
```

## 参考
https://inexio.jp/20150707-944/
http://rriifftt.hatenablog.com/entry/2015/10/23/110833
