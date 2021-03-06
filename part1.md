# Linux Fundamentals Part1

## Linuxとは
1. LinuxとはOSのひとつ。
2. Unix系のOSで、UNIX互換である。
3. 広義のLinux: Linuxディストリビューション 
4. 狭義のLinux: Linuxカーネル


## そもそも... OS(Operation System)とは
アプリケーションやハードウェアを動かすための基本的なソフトウェア。
ハードウェアとアプリケーションの橋渡し役となる。
コンピュータをコンピュータとして扱うためのソフトウェア。


### ハードウェアの例
1. CPU
2. メモリ
3. ストレージ
4. キーボード
5. モニタ
6. マウス
...etc


### アプリケーションの例
1. Google Chrome
2. Command Line
3. Visual Studio Code
...etc


### パソコン用OS
1. Windows
2. MacOS
...etc


### スマートフォン用OS
1. iOS
2. Android
...etc


### UNIX系OS
1. BSD
2. Linux
3. AIX
...etc


### サーバ系
1. Windows Server
2. Oracle Solaris
3. Red Hat Enterprise Linux


## では... UNIXとは
MIT(マサチューセッツ工科大学)のベル研究所から生まれたのOS。
世界で初めて広く公開されたOS。

元々無料で配布・利用できたものだが、派生OSが多くなり、利用に際してライセンス契約をしなければいけなくなった。
このような流れに対して、嫌気を指した人々が派生OSを作り始めて、独自でソースコードを改変するようになっていった。

このような流れのなかで、UNIXの派生OSであるMINIXを基にしてLinuxが生まれた。


## Linuxカーネルとは
LinuxOSの根幹部分のこと。
- ソフトウェアとハードウェアの仲介。

たとえば、
1. プロセスの制御、処理、管理
2. アプリケーションや周辺機器の監視

## Linuxディストリビューションとは
Linuxカーネルを基にして、主要なライブラリやコマンド、シェルなどを組み合わせてソフトウェアセットとして
一般利用できる形にしたもの。

たとえば、
1. Ubuntu
2. CentOS
3. Kali
4. Arch Linux
...etc


### 主要ディストリビューション
1. RedHat
  1. CentOS
  2. RHEL
  3. Fedora
2. Debian
  1. Debian GNU Linux
  2. Ubuntu


## Linuxを学ぶ意味
- クラウドの市場規模が拡大している
1. AWS - Amazon
2. GCP - Google
3. Azure - Micorosoft

これらのクラウドで動いているサーバとしては、Linuxが多い。

- コマンド操作力の向上
1. 現場では、CUI(文字入出力のインターフェース)が基本となる
2. GUIも使えるが、実稼働する業務系のサーバは基本的にCUIで動作する(ハードウェアリソース節約のため)
(ちなみに、Macは標準でLinuxコマンドが使える。ただし、WSLを入れればWindowsでも変わらない)



## ファイル・ディレクトリ
### 構造について

- dirX  : ディレクトリ
- fileX : ファイル

dir1

|___ dir2 _ file1

|___ dir3 _ file2

|___ dir4 _ file3

|___ file4


### ファイル・ディレクトリのパーミッション
#### パーミッションとは
大きくわけて、ファイル種別・とアクセス権限に分かれる。
ファイル種別は、ファイルやディレクトリ、シンボリックリンクなどを表す。

たとえば、
ディレクトリ : パーミッションの1文字目が "d"
ファイル   　: パーミッションの1文字目が "f"


アクセス権は、ファイルやディレクトリごとに付与される権限のこと。
所有者・ユーザ・グループによって権限を分けることができる。
r : 読み取り (数字:4)
w : 書き込み (数字:2)
x : 実行     (数字:1)

この数字というのは、chmodコマンドで権限を変更する際に使用するもの。

これらのそれぞれのアクセス権限について、コマンドによる設定で詳細にコントロールすることができる。

たとえば、
1. 所有者が読み書き
2. 他のユーザが読み取り
3. 所有グループが読み取り
のファイルの場合、、、

frw-r--r--

となる。


## パスについて
1. ルートディレクトリ   : Linuxにおいて、もっとも上位にあるディレクトリ。 "/"で表される
2. カレントディレクトリ : Linux上でユーザが現在いるディレクトリ。 pwdコマンドで表示できる。また"."で表される。
3. 親ディレクトリ　　   : Linux上でユーザが現在いる一つ上のディレクトリ。 ".."で表される。
4. ホームディレクトリ   : Linux上でユーザが最初にログインするディレクトリ。 "~"で表される。

#### 相対パスについて
今ユーザでいるディレクトリから見て、他のリソース(ファイルやディレクトリ)がどこにあるかを示すパス。

### 絶対パス(フルパス)について
ルートディレクトリから、あるリソースがどこにあるかを確実に示すパス。

