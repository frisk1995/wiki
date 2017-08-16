# NFSサーバ構築メモ

## 行ったこと
### selinux off => 省略  
### iptables off
* 両サーバにて行う
```
# /etc/rc.d/init.d/iptables stop  
# chkconfig iptables off  
# chkconfig --list iptables  
iptables        0:off   1:off   2:off   3:off   4:off   5:off   6:off
```
### NFSサーバ、NFSクライアントにツールをインストール
* 両サーバにて行う
```
# yum -y install nfs-utils
```

### rpcbindとnfsサービスを起動
* NFSサーバにて行う
```
# /etc/rc.d/init.d/rpcbind start    
# /etc/rc.d/init.d/nfs start  
# chkconfig nfs on  
# chkconfig rpcbind on  
```

### 共有するディレクトリの作成
* NFSサーバにて行う
```
# mkdir -p /export/www  
# chown -R nfsnobody:nfsnobody /export
```

### 共有公開設定
* NFSサーバにて行う
```
# vi /etc/exports
```
以下を追記
```
/export/www 192.168.0.0(rw)
```
> IPアドレスは接続を許可するIPアドレスにすること

### 設定を反映させる
* NFSサーバにて行う
```
# exports -a
```

### マウントするディレクトリを作成する
* NFSクライアントにて行う
```
# mkdir -p /mnt/nfs
```

### マウントする
* NFSクライアントにて行う
```
# mount -t nfs 192.168.0.1:/export/www /mnt/nfs
```
> `192.168.0.1:/export/www /mnt/nfs`  
> IPアドレスはNFSサーバのIPアドレス、`/export/www`は共有するディレクトリ、`/mnt/nfs`は共有をマウントするディレクトリ、を指定する



