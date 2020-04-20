# OpenShift Install Diet (for v4.3)  
OpenShiftインストールしたら2kgやせたメモ

This repository is an OpenShift installation configuration example.
The environment is as follows. Fedora host is not on the hardware certification list for RHCOS as a production environment. Just test for bare metal.  
このレポジトリはOpenShiftのインストール設定例です。
環境は以下の通り。FedoraはRHCOSのHCLにないため正式なサポート対象とはなりません。
あくまでベアメタル環境の試用目的です。
的確なガイドなしだと体重減ります(個人の感想です)。

## Install target box
 - Supermicro X11SSV-Q (Intel Core i7-6700)
 - Memory: 32GB
 - NVMe: 1TB
 - Host OS: Fedora 31
 - kernel-5.5.16-200.fc31.x86_64

# インストール Install

## 事前準備

1) https://www.openshift.com/products/container-platform
  "Get started" FREE TRIAL (かなり下の方)

2) "Create your own cluster" をクリック

3) "Deploy it in your datacenter"
   "Try it in your IT environment"をクリック

4) Red Hat Accountでログインする(Red Hat Accountがなければ作る)
   "Run on Bare Metal"をクリック

5) マニュアルをざっと読む

- Product Documentation for OpenShift Container Platform 4.3  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/

多すぎる。。少なくとも次の2点。他も余裕があれば。

- アーキテクチャー  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/html-single/architecture/index  
(半分広告のような記載なので要点がわかりにくい。24時間以内にインストールする必要があることが一番重要な記載。概ね次のマニュアルに重複した記載がある。)

- ベアメタルへのインストール  
https://access.redhat.com/documentation/ja-jp/openshift_container_platform/4.3/html-single/installing_on_bare_metal/index  
(概ねこのマニュアル通りに進められる。が、記載は玄人向けで行間の作業が多い。pxebootはHCL環境ではないためかうまくできない。)

6) 環境をセットアップする

 - ネットワーク
 - 仮想ホスト
 - dns
 - ロードバランサー
 - httpd
 - install-config.yaml

7) つづく