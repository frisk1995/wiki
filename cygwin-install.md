# Cygwin Setup

1.1. http://cygwin.com/よりsetpu.exeをダウンロードして起動

1.2. インストールウィザードが開かれるので【次へ】を選択

1.3. [Choose A Download Source]
「Install from Internet」を確認して【次へ】

1.4. [Select Root Install Directory]
「Just Me」を選択して【次へ】  

1.5. [Select Local Package Directory]
「Local Package Directory」の場所は任意で【次へ】  
※特に理由がなければそのまま  

1.6. [Select Your Internet Connection]
Proxyを設定している場合は、IEにProxyの設定を行って、  
「Use System Proxy Setting」を選択して【次へ】  

1.7. [Choose A Download Site]
「～.jp」を選択して【次へ】  
※特に理由がなければ「ftp://ftp.jaist.ac.jp」を選択  

1.8. Installするパッケージの選択  
以下の項目をインストールする  
※[Skip]の部分をクリックしてバージョン表記に変わることを確認  
※その他はDefaultのまま  

	[Category名]	Package名
	------------------------------------------------------------------
	[Python]	python2:Python 2 language interpreter
	[Python]	python2-yaml:Python LibYAML bindings
	[Python]	python2-setuptools:Python package management tools
	[Python]	python-paramiko:Python library for SSH2 connection
	[Python]	python-crypto:Cryptographic algorithms and protocols for Python
	[Devel]		gcc-core: GNU Compiler Collection(C,OpenMP)
	[Devel]		gcc-g++:GNU Compiler Collection(C++)
	[Devel]		make:The GNU version of the 'make' utility
	[Web]		wget:Utility to retrieve files from the WWW via HTTP and FTP
	[Net]		openssh:The OpenSSH server and client programs
	[Libs]		libyaml-devel:YAML 1.1 parser library
	[Editors]	gvim:GUI for the Vim text editor
	------------------------------------------------------------------

1.10. Cygwin再起動

1.11. pipのインストール
以下のコマンドでpipをインストールする
```
$ python /usr/lib/python2.7/site-packages/easy_install.py pip
```
2.1. ansibleインストール
pipを用いてansibleをインストールする
```
$pip install ansible
```
	
2.2. :ansible用ディレクトリの作成
```
$mkdir /etc/ansible
$cd /etc/ansible
```
	
2-3:インベントリファイルの作成
```
$vi hosts
```
>以下を追記  
>>[server]  
>><仮想マシンのIPアドレス>  

2.4. Ansibleのローカルでの動作確認
以下のコマンドでローカルホストでの動作確認をする。
```
$ ansible localhost -m ping
```
	
2.5. [SUCCESS]の表示を確認
	
3.1. 仮想環境の構築
vmwareにOSをインストールしておく

3.2. 仮想マシンのIPアドレスを取得する
```
$ip a
```

3.3. Selinuxは無効にしておく

4.1. ansibleの接続設定
```
$ vi ansible.cfg
```
>以下を追記  
>>[defaults]  
>>hostfile = ./hosts  
>>ask_pass = True  
>>ask_sudo_pass = True  
>>transport = paramiko  
>>host_key_checking = False  
>>retry_files_enabled = False  

4.2. ansibleからのアクセス確認
```
$ansible all -i hosts -m ping -u root  
SSH password:<ROOTのパスワード>  
SUDO password[defaults to SSH password]:<SSH接続ユーザのSUDO用パスワード>  
```
	
4.3. [SUCCESS]の表示を確認
