# knowledge
### root権限取得
```
sudo -i
```

### シャットダウン
```
shutdown -h now
```

### ディスク容量確認
```
fdisk -l
```
```
df -h
```

### 定期実行ジョブ確認
```
crontab -l
```

### 共有ディレクトリの権限変更
```
chmod 777 -R /home/share
```

### 定期実行ジョブ作成
```
vi /usr/bin/shutdown.sh
---------------------------
#!/bin/sh
shutdown -r now
---------------------------
cat /usr/bin/shutdown.sh
chmod 755 /usr/bin/shutdown.sh
crontab -e
---------------------------
59 23 * * * root sh /usr/bin/shutdown.sh
---------------------------
```
