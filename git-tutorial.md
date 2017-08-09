# Gitチュートリアル  

## 前提条件  
* gitアカウントの作成  
* gitインストール  
* gitブラウザでリポジトリを作成しておく  

## 初回実行時
1.Pushするリポジトリ（ディレクトリ）の作成

```
  [~]$ mkdir push_test/
  [~]$ cd push_test/
```

2.git情報の登録
```
  [push_test]$ git init
  [push_test]$ git config --global user.name "ユーザ名"
  [push_test]$ git config --global user.email "メールアドレス"
```

3.READMEの作成
```
  [push_test]$ touch README
```
4.Pushするファイルの追加
```  
[push_test]$ git add ファイル名
```

5.Commit
```
  [push_test]$ git commit -m '任意の文字列（コメント）'
```

6.githubにPush
```
  [push_test]$ git remote add origin https://リポジトリのURLi.git
```
  #上記コマンドは初回の一回だけ実行かも?
```  
[push_test]$ git push -u origin master
```

## 2回目以降のファイル更新時  
1.変更したファイルの追加
```
  $ git add ファイル名
```
2.commit
``
  $ git commit -m '任意の文字列（コメント）'
```
3.push 
```
  $ git push -u origin master 
```
