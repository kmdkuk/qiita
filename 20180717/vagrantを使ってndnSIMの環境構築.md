---
ID: 50e04982089e12c9b11b
Title: vagrantを使ってndnSIMの環境構築
Tags: Vagrant,VirtualBox,ns3,ndnSIM
Author: Kamada Kouki
Private: false
---

# はじめに
ちょいちょい環境が何故か壊れたり
再インストールしたときに書いてある場所によって書いてあることが違いすぎて
あれどうインストールしたっけとなるので、Vagrantで環境をいい感じにしたくて
Vagrant初挑戦

何をやったかは見たくなく
実践したい人は以下をクローンして
`vagrant up`してください。
https://github.com/kmdkuk/vagrant4ndnsim
＊注意：PyVisを使うためにディスプレイを使ったり
ホストOSでシナリオやエクステンションを編集するためのフォルダ共有のあれこれはまだ

# 環境
Ubuntu16.04
Vagrant 2.1.2

# 手順
debian9に環境を作っていきます。
GuestAdditionインストールのためのvagrant-vbguestもインストール

```bash
$ vagrant init debian/stretch64
$ vagrant plugin install vagrant-vbguest
```
ここで仮想環境を立ち上げるのですが、
ndnSIMをインストールするのにメモリなど足りないので
`vagrant init`でできたVagrantfileをこんな感じに
メモリーとCPUの数を追加していきます。
initした時にいろいろなコメントが入ってありますがとりあえずスルーしました。

```ruby:Vagrantfile
Vagrant.configure("") do |config|
  config.vm.box = "debian/stretch64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
```

こっからはndnSIMのドキュメント通りにインストールする手順を
provisioner.shに記録していきます。
debianにはデフォルトでpkg-configが入ってない？ようだったので
installを追加しています。

```bash:provisioner.sh
sudo apt install -y build-essential libsqlite3-dev libcrypto++-dev libboost-all-dev libssl-dev git python-setuptools
sudo apt install -y python-dev python-pygraphviz python-kiwi python-pygoocanvas python-gnome2 python-rsvg ipython
sudo apt install -y pkg-config
mkdir ndnSIM
cd ndnSIM
git clone https://github.com/named-data-ndnSIM/ns-3-dev.git ns-3
git clone https://github.com/named-data-ndnSIM/pybindgen.git pybindgen
git clone --recursive https://github.com/named-data-ndnSIM/ndnSIM.git ns-3/src/ndnSIM

# Build and install NS-3 and ndnSIM
cd ns-3
./waf configure -d optimized
./waf

sudo ./waf install
cd ..

git clone https://github.com/named-data-ndnSIM/scenario-template.git scenario
cd scenario
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

./waf configure
```
以上の手順をprovision.shに書いて
プロビジョニングの設定をしていきます。

```ruby:Vagrantfile
config.vm.provision "shell", path: "provisioner.sh" #←追加
```


```bash
$ vagrant up
```

仮想環境に入り作業する

```bash
$ vagrant ssh
```

これでコマンドラインからndnSIMを使うことができます。
PyVisを使うためにディスプレイを使ったり
ホストOSでシナリオやエクステンションを編集するためのフォルダ共有のあれこれを追加編集予定
