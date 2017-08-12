# Ubuntuチュートリアル

## ファイアーウォール設定

iptablesの一覧取得
```
# iptables --list
```

iptablesに80/tcpを追加
```
# iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

iptablesの保存
```
# apt install iptables-persistent
# iptables save
```
