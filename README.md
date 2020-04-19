# openshift_install

This repository is an OpenShift installation configuration example.
The environment is as follows. Fedora host is not on the hardware certification list for RHCOS as a production environment. Just test for bare metal.  
このレポジトリはOpenShiftのインストール設定例です。
環境は以下の通り。FedoraはRHCOSのHCLにないため正式なサポート対象とはなりません。
あくまでベアメタル環境の試用目的です。

## Install target box
 - Supermicro X11SSV-Q (Intel Core i7-6700)
 - Memory: 32GB
 - NVMe: 1TB
 - Host OS: Fedora 31
 - kernel-5.5.16-200.fc31.x86_64

# Install

1) go https://www.openshift.com/products/container-platform
  "Get started" FREE TRIAL

2) Select "Create your own cluster"

3) "Deploy it in your datacenter"
   Click "Try it in your IT environment"

4) Login with your Red Hat account
   Select "Run on Bare Metal"


