# SSH鍵認証ログインの方法

## 命名規則
* ホスト端末[host]：リモートアクセスを行う端末
* ゲスト端末[gest]：リモートアクセスをされる端末

## TeraTerm利用時
1. ホスト端末で鍵を作成する  
  [設定]から[SSH鍵生成]を選択  
  鍵の種類を選択し、[生成]をクリック  
  パスフレーズは空白でも可  
  秘密鍵と公開鍵をそれぞれ生成する  

2. ゲスト端末に必要なファイルを作成し、権限を設定する  
   ゲスト端末に通常のSSHでログインする
```
[gest]$ mkdir ~/.ssh
[gest]$ chmod 700 ~/.ssh/
```

3. ゲスト端末にホスト端末で作成した公開鍵を転送する  
  TeraTermの[ファイル]を選択し、[SSH SCP]をクリック  
  Fromに公開鍵（~.pub)を選択、Toには ~/ を入力して[Send]をクリック  

4. 公開鍵を登録する
```
[gest]$ mv ~/id_rsa.pub ~/.ssh/authorized_keys
```

5. TeraTermにて動作確認
  TeraTermで[ファイル]から[新しい接続]を選択  
  ホストとポートを入力し[OK]をクリックする  
  認証画面が表示されたら[RSA/DSA鍵を使う]を選択し、[秘密鍵]をクリック  
  秘密鍵(id_rsa)を選択したらユーザ名のみ入力し、[OK]をクリック  
　接続されることを確認して終了。  

参考URL：
	https://webkaru.net/linux/tera-term-ssh-login-public-key/

## Command Line
1. 鍵の作成
```
[host]$ cd~/.ssh/
[host]$ ssh-keygen -t rsa
```

2. 鍵の転送
```
[host]$ scp ~/.ssh/id_rsa.pub サーバのユーザ名@サーバのIP:~/
```

3. 鍵の登録
```
[gest]$ mkdir ~/.ssh
[gest]$ chmod 700 ~/.ssh/
[gest]$ mv ~/id_rsa ~/.ssh/authorized_keys
[gest]$ chmod 600 ~/.ssh/authorized_keys
```

4. 動作確認
```  
[host]$ ssh サーバのIP -l ログインユーザ -i id_rsa
```  
>認証なしでログインできればOK  
>ログインユーザは公開鍵を持つユーザにすること
