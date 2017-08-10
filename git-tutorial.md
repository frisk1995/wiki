# Gitチュートリアル  

## 前提条件  
* gitアカウントの作成  
* gitインストール  
* gitブラウザでリポジトリを作成しておく  

## 初回実行時
1. Pushするリポジトリ（ディレクトリ）の作成

```
[~]$ mkdir push_test/
[~]$ cd push_test/
```

1. git情報の登録
```
[push_test]$ git init
[push_test]$ git config --global user.name "ユーザ名"
[push_test]$ git config --global user.email "メールアドレス"
```

1. READMEの作成
```
[push_test]$ touch README
```
1. Pushするファイルの追加
```  
[push_test]$ git add ファイル名
```

1. Commit
```
[push_test]$ git commit -m '任意の文字列（コメント）'
```

1. githubにPush
```
[push_test]$ git remote add origin https://リポジトリのURLi.git
```
>上記コマンドは初回の一回だけ実行
```  
[push_test]$ git push -u origin master
```

## 2回目以降のファイル更新時  
1. 変更したファイルの追加
```
[push_test]$ git add ファイル名
```
1. commit
```
[push_test]$ git commit -m '任意の文字列（コメント）'
```
1. push 
```
[push_test]$ git push -u origin master 
```

