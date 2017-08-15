
## タイムゾーンの設定
**操作ファイル名**
```
set-timezone.yaml
files/clock
```
* タイムゾーンを"Asia/Tokyo"に設定し、UTCを無効にする

## 日本語環境の設定
**操作ファイル名**
```
set-language.yaml
files/i18n
```
* システムの文字セットを変更する  

## ネットワークの設定
**操作ファイル名**
```
set-network.yaml
files/network
```
* システムのネットワーク設定を行う
> ipv6無効、ホスト名、ゲートウェイの指定など

## インターフェース設定
**操作ファイル名**
```
set-interface.yaml
```
* 省略

## IPv6無効化設定
**操作ファイル名**
```
disabled-ipv6.yaml
files/disabled_ipv6.conf
```
* ipv6を無効化する
## ブートローダの設定
**操作ファイル名**
```
set-mbr.yaml
```
* ブートローダのインストール先 -> MBR

## grubの設定
	[set-grub.yaml]
	
ローカルのfilesからgrub.confを取得し、コピーする

## カーネルダンプの設定
**操作ファイル名**
```
set-kdump.yaml
```
* 省略

## ランレベルの設定
**操作ファイル名**
```
set-runlevel.yaml
files/inittab
```
* デフォルトのランレベルを変更する

## パッケージの追加
```
install-package.yaml
```
* 有償アプリケーションが多いため省略

## yumの設定
```
set-yum.yaml
```
* カーネルのアップデートはしない設定
	

## サービスの起動・停止
	[set-service.yaml]


## カーネルパラメータの設定
	[set-ketnel.yaml]
	

## 名前解決方式
	[set-switch.yaml]
	

## DNSクライアント設定
	[set-resolv.yaml]
	

## 時刻同期設定
	[set-net.yaml]

## ファイアウォール設定
	[set-firewall.yaml]
	

## Selinux設定
	[set-selinux.yaml]
	

## oracleルール設定
	[set-oracle.yaml]
	

## sshd設定
	[set-sshd.yaml]

