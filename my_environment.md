# 自分の環境構築用
## Cygwin

### https://github.com/frisk1995/wiki/blob/master/cygwin-install.md をみてCygwinをインストール

### apt-cyg インストール
apt-cyg
> パッケージマネージャ
```
$ wget https://githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg  
$ chmod 755 apt-cyg  
$ mv apt-cyg /usr/local/bin/
```
### nkfのインストール
nkf
> 日本語の文字化け対策

* nkfのソースコードをSourceForgeから取得  
http://sourceforge.jp/projects/nkf/
```
$ tar xvfz nkf-*.tar.gz  
$ cd nkf-*  
$ make  
$ make install
```
### pip

### tmux
```
$ pip install tmux
```
### ansible
```
$ pip install ansible
```
### git
```
$ 
```
### vimrcの設定
### bash_profileの設定
### zsh
### tree
```
$ pip install tree
```

## Linux(rhel系統)
### tmux
```
# yum install tmux
```
### git
```
# yum install git
```
### tree
```
# yum install tree
```
### ansible
```
# yum install epel-release
# yum install enablerepo=epel ansible
```
### vimrcの設定
### bash_profileの設定
### zsh
