# 追加HDDをフォーマットしてマウントする
## vmware playerでHDDを追加してフォーマット、マウントする方法
### vmware playerでHDDを追加する

### 追加HDDをフォーマットする
追加したHDDの名前(/dev/sd?)を特定する。
```
# fdisk -l
```
特定したらそのデバイスをフォーマットする。  
（例として、`/dev/sdb`を追加したとする）
```
# fdisk /dev/sdb
```
対話型でフォーマットが行われるので以下のように入力する。  
`d`（パーティションの削除）を実行する。
```
コマンド (m でヘルプ): d  
Selected partition 1  
Partition 1 is deleted  
  
コマンド (m でヘルプ): d  
No partition is defined yet!  
```
`No partition is defined yet!`が出力されたことを確認する。
`n`（パーティションの新規作成）を実行する。
```
コマンド (m でヘルプ): n  
Partition type:  
   p   primary (0 primary, 0 extended, 4 free)  
   e   extended  
```
`p`を選択する。
```
Select (default p): p
```
複数パーティションを作成する場合はそれに合わせて以下の値を変更する。  
1つしか作成しない場合はすべてデフォルト値で作成を行う。  
＊以下の値と同じになるとは限らない。【初期値】の値をそれぞれの環境で変更すること）
```
パーティション番号 (1-4, default 1): 1  
最初 sector (2048-3907029167, 初期値 2048):　2048  
Last sector, +sectors or +size{K,M,G} (2048-3907029167, 初期値 3907029167):　3907029167  
Partition 1 of type Linux and of size 1.8 TiB is set  
```
`w`（パーティションの書き込み）を実行する。
```
コマンド (m でヘルプ): w  
パーティションテーブルは変更されました！  
  
ioctl() を呼び出してパーティションテーブルを再読込みします。  
ディスクを同期しています。  
```
フォーマットする。
（＊フォーマット形式はext4を指定）
```
# mkfs -t ext4 /dev/sdb1
```

### フォーマットしたHDDをマウントする
マウントするディレクトリを作成する。  
（例として、`/mnt/hdd`にマウントする）
```
# mkdir /mnt/hdd  
# mount /dev/sdb1 /mnt/hdd
```
マウントしただけだと、再起動時にアンマウントされてしまうため、自動マウントの設定を行う。
```
# vi /etc/fstab
```
最終行に以下を追記
```
dev/sdb1		/mnt/hdd	ext4
```
<マウントするデバイス>	<マウント先> <フォーマット形式>


