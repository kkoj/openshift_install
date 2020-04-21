# OpenShiftインストールして2kg痩せたはなし  
OpenShift Install Diet (for v4.3)  

This repository is an OpenShift installation configuration example.  
The environment is as follows. Fedora host is not on the hardware certification list for RHCOS as a production environment. Just test for bare metal. In my opinion, without the right way to install it, you will lose your weight.  
このレポジトリはOpenShiftのインストール設定例です。
環境は以下の通り。FedoraはRHCOSのHCLにないため正式なサポート対象とはなりません。あくまでベアメタル環境の試用目的です。  
的確なガイドなしだと体重減ります(個人の感想です)。

## Install target box
 - Supermicro X11SSV-Q (Intel Core i7-6700)
 - Memory: 32GB
 - NVMe: 1TB
 - Host OS: Fedora 31
 - kernel-5.5.16-200.fc31.x86_64

 ここで重要なのはとにかく高速なローカルディスクを使うこと。  
 なぜならインストーラーの実行にかかる時間が長いとうまくいかないから。

# インストール Install

## 事前準備

1) https://www.openshift.com/products/container-platform  
  "Get started" FREE TRIAL (かなり下の方)をクリック

2) "Create your own cluster" をクリック

3) "Deploy it in your datacenter"  
   "Try it in your IT environment"をクリック

4) Red Hat Accountでログインする(Red Hat Accountがなければ作る)  
   "Run on Bare Metal"をクリック

5) マニュアルをざっと読む

- Product Documentation for OpenShift Container Platform 4.3  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/

多すぎる。。へこたれそうだ。少なくとも次の2点。他も余裕があれば。

- アーキテクチャー  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/html-single/architecture/index  
(冗長すぎて要点がわかりにくいよ。。が、24時間以内にインストールすることが一番重要な記載。他は無視していい。飾りだと思っていい。概ね次のマニュアルに重複した記載があるから。  
金曜日に準備して、月曜日に続きを、という段取りは甘い。一気にインストールするべし。  
さもなくばSSL通信のパケットダンプ取ったりMan in the Middleつくったり、letsencryptで試したり、すごい消耗することになるから。  というか、その注意点だけ記載してくれていればよかった。。。)  

- ベアメタルへのインストール  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/html-single/installing_on_bare_metal/index  
(概ねこのマニュアル通りに進められる。が、記載は玄人向けで行間の作業が多い。pxebootはHCL環境ではないためかうまくできない。必要な部分は記憶から揮発する前に記載しておかないと。。)  

6) 環境をセットアップする

 - ネットワーク
 - 仮想ホスト
 - dns
 - ロードバランサー
 - httpd
 - install-config.yaml

 (詳細は逐次追記予定)

7) ようやくインストーラーを使う


## openshift-install

つづく