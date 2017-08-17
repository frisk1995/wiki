# Sambaサーバの構築
Sambaサーバは、Windowsのファイル共有サービス

## sambaのインストール
```
# yum install -y samba4 samba4-client
```
## 共有ディレクトリの作成
```
# mkdir /home/share
# chmod 777 /home/share
```

## sambaの設定
```
# vi /etc/samba/smb.conf
```
```
# 66行目あたり：以下2行追記  
unix charset = UTF-8  
 dos charset = CP932  

# 87行目：変更 ( Windowsに合わせる )  
workgroup = WORKGROUP  

# 93行目：コメント解除してアクセスを許可するIPアドレスを指定  
hosts allow = 127. 10.0.0.  

# 120行目：追記 ( 認証なし設定 )  
security = user  
 passdb backend = tdbsam  
 map to guest = Bad User  

# 最終行に追記  
[Share]# 任意の名前を指定  
    path = /home/share# 共有フォルダを指定  
    writable = yes# 書き込み許可  
    guest ok = yes# ゲストユーザー許可  
    guest only = yes# 全てゲストとして扱う  
    create mode = 0777# フルアクセスでファイル作成  
    directory mode = 0777# フルアクセスでフォルダ作成  
```

## sambaサービスの起動
iptablesは設定をしておく。  
面倒な場合は`# service iptables stop`で無効にしておく。

```
# service smb start
# service nmb start
# chkconfig smb on
# chkconfig nmb on
```

## Windowsでの操作
