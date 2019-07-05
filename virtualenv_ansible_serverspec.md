# OracleLinux(オフライン)端末におけるansibleとserverspec環境の構築手順
## **virtualenv/ansible**

### **前提パッケージのオフラインインストール**
- gcc
- python-setuptools
- python-devel
- libffi-devel
- openssl-devel

#### **パッケージ取得**
ローカルリポジトリ用ディレクトリの作成
```
# mkdir /usr/my_repo; cd $_
```

ネットワーク接続可能端末(VM等)でパッケージをダウンロードする
```
# yumdownloarder --resolve gcc
# yumdownloarder --resolve python-setuptools
# yumdownloarder --resolve python-devel
# yumdownloarder --resolve libffi-devel
# yumdownloarder --resolve openssl-devel
```

ローカルリポジトリの作成
```
# createrepo .
```

リポジトリの圧縮
```
# cd /tmp
# tar -C /usr/ -zcvf ./my_repo.tar.gz my_repo
```

圧縮したファイルをSCP等を用いて回収する

### **ローカルリポジトリの導入と設定**
回収したtarファイルを対象のサーバ(オフライン)に配置する(/tmp)  
/tmpに配置したリポジトリファイルを展開する
```
# cd /tmp
# tar zcvf ./my_repo.tar.gz
```

展開したディレクトリを/usrディレクトリ配下に配置する
```
# mv -v ./my_repo /usr/
```

リポジトリ設定を追加する
```
# vi /etc/yum.repos.d/my_repo.repo
[my_repo]
enabled=1
name=MyPKG
gpgcheck=0
baseurl=file:///usr/my_repo
```
※既存のリポジトリは無効化しておく

yumのキャッシュを削除する
```
# yum clean all
```

ローカルリポジトリを確認する
```
# yum repolist
## ここで持ち込んだ`my_repo`が表示されることを確認する
```

### **前提パッケージのインストール**
前提パッケージをローカルリポジトリからインストールする
```
# yum install gcc python-setuptools python-devel libffi-devel openssl-devel
```

### **資材配置**
パッケージ配置用ディレクトリの作成
```
# mkdir /tmp/work_ansible; cd $_
```
`/tmp/work_ansible`に必要なパッケージを配置する

- pip-19.1.1.tar.gz
- sshpass-1.06-1.el7.x86_64.rpm
- virtualenvのパッケージ一覧(download_pkg_virtualenv)
- ansibleのパッケージ一覧(download_pkg_ansible)　　
※バージョンは適宜読み替えること

### **virtualenvのインストール**
pipをインストールする
```
# easy_install ./pip-19.1.1.tar.gz
# echo $?
-> 0が出力されること
```

setuptoolsをアップデートする
```
# pip install --no-index ./download_pkg_virtualenv/setuptools-41.0.1.zip
# echo $?
-> 0が出力されること
```

virtualenvのインストール
```
# easy_install ./download_pkg_virtualenv/virtualenv-16.6.1.tar.gz
# echo $?
-> 0が出力されること
```

### **virtualenvを使いansible用仮想環境を構築する**
```
# virtualenv --no-download ansible
# source ./ansible/bin/activate
-> コンソールに(ansible)が表示されること
```

### **ansibleのインストール**
```
# pip install -f ./download_pkg_ansible --no-index ./download_pkg_ansible/ansible-2.8.1.tar.gz
# echo $?
-> 0が出力されること
```

sshpassのインストール
```
# rpm -ivh ./sshpass-1.06-1.el7.x86_64.rpm
```

### ansible動作確認
```
# ansible localhost -m ping
-> successすること
```

### 鍵認証環境下におけるansibleの通信方法


### 仮想環境から抜ける
```
# deactivate
```
___
## **serverspec**
### **前提パッケージのオフラインインストール**
- gcc
- gcc-c++
- readline
- zlib
- openssl
- zlib-devel
- readline-devel
- openssl-devel  

※ansibleと同じPKGも列記している

#### **パッケージ取得**
ローカルリポジトリ用ディレクトリの作成
```
# mkdir /usr/my_repo; cd $_
```

ネットワーク接続可能端末(VM等)でパッケージをダウンロードする
```
# yumdownloarder --resolve gcc
# yumdownloarder --resolve gcc-c++
# yumdownloarder --resolve readline
# yumdownloarder --resolve zlib
# yumdownloarder --resolve openssl
# yumdownloarder --resolve zlib-devel
# yumdownloarder --resolve readline-devel
# yumdownloarder --resolve openssl-devel
```

ローカルリポジトリの作成
```
# createrepo .
```

リポジトリの圧縮
```
# cd /tmp
# tar -C /usr/ -zcvf ./my_repo.tar.gz my_repo
```

圧縮したファイルをSCP等を用いて回収する

### **ローカルリポジトリの導入と設定**
上述を参照

### **前提パッケージのインストール**
前提パッケージをローカルリポジトリからインストールする
```
# yum install gcc gcc-c++ readline zlib openssl zlib-devel readline-devel openssl-devel
```

### **資材配置**
パッケージ配置用ディレクトリの作成
```
# mkdir /tmp/work_serverspec; cd $_
```
`/tmp/work_serverspec`に必要なパッケージを配置する
- serverspecのパッケージ一覧

### **rubyインストール**
rubyファイルの展開
```
# cd /tmp/work_serverspec/ruby
# tar zxvf ruby-2.6.3.tar.gz
```

rubyインストール
```
# cd ruby-2.6.3
# ./configure
# make
# make install
```

### **rubygemsインストール**
rubygemsファイルの展開
```
# cd /tmp/work_serverspec/ruby
# tar zxvf rubygems-3.0.4.tgz
```

rubygemsインストール
```
# cd rubygems-3.0.4
# ruby setup.rb
```

### **serverspecインストール**
```
# cd /tmp/work_serverspec/serverspec
# gem install --local serverspec
```

### **動作確認**
```
# cd ~/
```

serverspec用ディレクトリ作成
```
# mkdir serverspec; cd $_
```

serverspec-initの実行
```
# serverspec-init
--------------------------------
Select OS type:

  1) UN*X
  2) Windows

Select number: 1    <--入力

Select a backend type:

  1) SSH
  2) Exec (local)

Select number: 1    <--入力

Vagrant instance y/n: n
Input target host name: hostname <---ここはテスト対象のホスト名(名前解決可能なこと)
 + spec/
 + spec/hostname/
 + spec/hostname/sample_spec.rb
 + spec/spec_helper.rb
 + Rakefile
 + .rspec
--------------------------------
```

テスト用specファイルの書き換え
```
# vi ./spec/hostname/sample_spec.rb
※全削除後、以下を入力
--------------------------------
require 'spec_helper'

describe command('cat /etc/selinux/config | grep ^SELINUX=') do
  its(:stdout) { should match /disabled/ }
end
describe command('cat /etc/selinux/config | grep ^SELINUXTYPE=') do
  its(:stdout) { should match /targeted/ }
end
--------------------------------
```

serverspecを実行
```
# rake spec
-> failがないこと
```
