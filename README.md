# OpenShiftインストールして2kg痩せたはなし  
OpenShift Install Diet (for v4.5)  

This repository is an OpenShift installation configuration example.  
The environment is as follows. Fedora host is not on the hardware certification list for RHCOS as a production environment. Just test for bare metal. In my opinion, without the right way to install it, you will lose your weight.  
このレポジトリはOpenShiftのインストール設定例です。
環境は以下の通り。FedoraはRHCOSのHCLにないため正式なサポート対象とはなりません。あくまでベアメタル環境の試用目的です。  
このインストールは的確なガイドなしだと体重減ります(個人の感想です)。
肝心のhaproxyとdnsのコンフィグは config ディレクトリに参考として集めています。

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

1) https://cloud.redhat.com/openshift/install

2) Red Hat Accountでログインする(Red Hat Accountがなければ作る)  
   "Run on Bare Metal"をクリック

3) Downloads から必要ファイルをダウンロードする  
   "Download Installer"  
   "Download pull secret"  
   "Download RHCOS"  
   "Download command-line tools"  

4) マニュアルをざっと読む

- Product Documentation for OpenShift Container Platform 4.5  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/

多すぎる。。へこたれそうだ。少なくとも次の2点。他も余裕があれば。

- アーキテクチャー  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.5/html-single/architecture/index  
(冗長すぎて要点がわかりにくいよ。。が、24時間以内にインストールすることが一番重要な記載。他は無視していい。飾りだと思っていい。概ね次のマニュアルに重複した記載があるから。  
金曜日に準備して、月曜日に続きを、という段取りは甘い。一気にインストールするべし。  
さもなくばSSL通信のパケットダンプ取ったりMan in the Middleつくったり、letsencryptで試したり、すごい消耗することになるから。  というか、その注意点だけ記載してくれていればよかった。。。)  

- ベアメタルへのインストール  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.5/html-single/installing_on_bare_metal/index  
(概ねこのマニュアル通りに進められる。が、記載は玄人向けで行間の作業が多い。pxebootはHCL環境ではないためかうまくできない。必要な部分は記憶から揮発する前に記載しておかないと。。)  

6) 環境をセットアップする

 - ネットワーク
 - 仮想ホスト
 - dns
 - ロードバランサー
 - httpd
 - install-config.yaml

 (詳細は逐次追記予定)

## Load Balancer

yum -y install haproxy

systemctl enable haproxy
systemctl start haproxy

setsebool -P haproxy_connect_any=1  
setsebool haproxy_connect_any on  

 ようやくインストーラー openshift-install を使う
 
## openshift-install

新たにssh鍵を用意するなら以下(install-config.yamlに入れるssh公開鍵の対になる秘密鍵を指定)  

$ ssh-keygen -t rsa -b 4096 -N '' -f id_rsa
$ eval "$(ssh-agent -s)"  
$ ssh-add id_rsa  

$ mkdir ignition  

install-config.yaml作成  

$ vi ignition/install-config.yaml  
Kubernetesマニフェスト作成  

$ ./openshift-install create manifests --dir=ignition  
INFO Consuming Install Config from target directory  
WARNING Making control-plane schedulable by setting MastersSchedulable to true for Scheduler cluster settings  
$ sed -i -e 's/mastersSchedulable: true/mastersSchedulable: false/g' ignition/manifests/cluster-scheduler-02-config.yml  
または  
$ vi ignition/manifests/cluster-scheduler-02-config.yml  
mastersSchedulable: false  

$ ./openshift-install create ignition-configs --dir=ignition  
INFO Consuming Master Machines from target directory  
INFO Consuming Worker Machines from target directory  
INFO Consuming Openshift Manifests from target directory  
INFO Consuming OpenShift Install (Manifests) from target directory  
INFO Consuming Common Manifests from target directory  

INFO Consuming Common Manifests from target directory  
INFO Consuming Openshift Manifests from target directory  
INFO Consuming Master Machines from target directory  
INFO Consuming Worker Machines from target directory  
INFO Consuming OpenShift Install (Manifests) from target directory

$ scp bootstrap.ign master.ign worker.ign root@www:/var/www/html/ignition

$ ./openshift-install --dir=ignition wait-for bootstrap-complete --log-level=debug
DEBUG OpenShift Installer 4.4.3                    
DEBUG Built from commit 78b817ceb7657f81176bbe182cc6efc73004c841 
INFO Waiting up to 20m0s for the Kubernetes API at https://api.openshift2.example.co.jp:6443... 
INFO API v1.17.1 up                               
INFO Waiting up to 40m0s for bootstrapping to complete... 
DEBUG Bootstrap status: complete                   
INFO It is now safe to remove the bootstrap resources 

$ export KUBECONFIG=ignition/auth/kubeconfig

