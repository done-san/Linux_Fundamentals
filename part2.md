# Linux Fundamentals Part2

## Index
1. ディレクトリ構造
2. Shell
3. Path
4. コマンドライン操作
5. vim


## 3. コマンドライン操作
#### ディレクトリ内の情報を表示
``` ls ```

#### ディレクトリの作成
``` mkdir ```  


#### 空ファイルの作成
``` touch ```  

#### ファイルの中身を表示
``` cat ```  

#### ファイル・ディレクトリの名前の変更・移動
``` mv ```  

#### ファイル・ディレクトリのコピー
``` cp ```  

#### ファイルやディレクトリの作成
``` rm ```  
``` rm -r ```  

## ディレクトリ構造
- /       : ルートディレクトリ
- /bin    : binaryの略。すべてのユーザが使用するファイルを格納。最小限の実行ファイルのみ
- /boot   : システムの起動に必要なファイルを格納。ブートローダに関するファイルがある
- /dev    : デバイスドライバ(OSがデバイスを認識し操作するためのファイル)を格納
- /etc    : Linuxを設定するための各種ファイルを格納
- /home   : ユーザのホームディレクトリを格納
- /lib    : カーネルモジュールファイルとプログラムに必要なライブラリを格納。ライブラリは/binと/sbinに必要
- /media  : /mntとの区別で、/mediaはOSが自動マウントする対象。(例: USBやDVD)
- /mnt    : /mediaとの区別で、取り外し可能な装置に対してファイルシステムを接続
- /tmp    : システム使用中に発生した一時ファイルを格納。すべてのユーザが共同で使用
- /opt    : 追加で使用するアプリケーションパッケージがインストールされるディレクトリ(例: Google Chrome)
- /proc   : 現在実行中のプロセスに関する情報やデータを格納。メモリ上にあるため、仮想ファイルシステムともよばれる
- /sbin   : システム管理者(root)が利用できるプログラムを格納
- /sys    : Linuxカーネルに関する情報を格納。システム全般の内容を提供
- /root   : システム管理者(root)のホームディレクトリ
- /usr    : すべてのユーザが使えるアプリケーションを格納。インストールにはroot権限が必要
- /var    : システム運用中に生成されて削除されるデータを保存するためのディレクトリ。(例: ログファイル)  


## シェルについて
- ユーザからの入力をカーネルに伝える
- カーネルの出力をユーザに伝える
すなわち、カーネルとユーザの仲介役ということ。

- ユーザが入力する言語は、機械語でカーネルには分からない
- 機械語で出力された結果は、ユーザには分からない
カーネルの実体はC言語をコンパイルした機械語と考えると、シェルが必要だということが分かる。

### シェルの種類
1. sh (Bシェル)
    1. Bourne Shell
    2. 古くからあるシェル
2. bash
    1. Bourne Again SHell
    2. CentOSやUbuntuの標準シェル
    3. shを基に機能を強化している
3. csh
    1. C Shell
    2. C言語に類似した構文で記述できる
4. zsh
    1. Z Shell
    2. MacOSの標準のシェル

コマンドライン上で  
``` echo $SHELL ``` と入力すると、現在使用しているシェルを確認できる。
  
  
例) Ubuntu 20.04.3 LTSの場合 
```
/bin/bash
```  
  
  
``` cat /etc/shells ``` で使用可能なシェルを確認できる。  
```
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
```  
  
``` chsh -s /bin/使うシェルのパス ``` で使用するシェルを変更できる  
SSH接続でログインしている状況の例

```
echo $SHELL 
chsh -s /bin/sh
Password: (パスワードを入力する)
logout
ssh -l ユーザ名 対象のホスト名(or IPアドレス)
echo $SHELL
```  

## プロンプト
ユーザからの入力を待ち受けている状態を表す記号や文字。  
たとえば、``` $ ``` などがある。

``` echo $PS1 ``` で現在のプロンプトの表示を確認できる。

## 環境変数
OSが保持し、システム全体で利用する共通の値のこと。  
使用例  
- パスを通すとき
- システムやアプリケーションのコードには直接書くことが推奨されない変数を使うとき
- 頻繁に利用する値を変数に保存するとき
- Linuxでの使用言語を変更するとき  
  
環境変数とその値の一覧表示  
``` env ```  

変数の値を取り出し  
``` echo $変数名 ```  

環境変数の作成(一時的に保存)
``` 変数名=値 ```  

環境変数の削除  
``` unset 変数名 ```  

例)環境変数の作成と削除  
```
env | grep DONE_SAN
export MYNAME=DONE_SAN 
env | grep DONE_SAN
unset MYNAME
env | grep DONE_SAN
```  

### $PATH
コマンドは入力されると、$PATHの中身を見ている。  
($PATHが通っていない場合、コマンド実行ができない)

コマンドの実行ファイルを探す  
``` which コマンド名 ```  

例) lsコマンドの位置を確認する  
``` 
which ls 
> /usr/bin/ls
```

パスを表示する
``` echo $PATH ```  

実行結果例)  
``` 
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
``` 

### ビルドインコマンド
カーネルに組み込まれているコマンド  

コマンドがビルドインコマンドかどうか確認する
``` type コマンド名　```  

例) cdとls、chmodを確認する
``` 
type cd
> cd is a shell builtin

type ls
> ls is aliased to `ls --color=auto'

type chmod
> chmod is /usr/bin/chmod
```  

## コマンドライン操作

### 標準入力・標準出力・標準エラー出力
※以下の説明は、初心者向けであり適切ではありません。
1. 標準入力
    1. キーボードからの入力のこと
2. 標準出力
    1. 画面に表示されるメッセージ
3. 標準エラー出力
    1. 画面に表示されるエラーメッセージ  

### 複数コマンドの実行
``` ; ``` : 複数のコマンドを実行する
``` 
~/$ mkdir sample; cd sample 
~/sample$
```  

``` & ``` : 複数のコマンドを同時に実行する  
```
mkdir sample2 & cd sample2

> [1] 230818
> -bash: cd: sample2: No such file or directory
```  

``` && ``` : 先に実行したコマンドが成功した場合、次のコマンドを実行する  
```
~/$ mkdir sample3 && cd sample3
~/sample3$ 
```  

``` || ``` : 先に実行したコマンドが失敗した場合、次のコマンドを実行する  
```
cd sample4 || echo "cd command is failed"

> -bash: cd: sample4: No such file or directory
> cd command is failed
```  

``` echo ``` : 画面に文字列や数値を表示  
``` 
echo string
> string

echo 1111
> 1111
```  

パイプ ``` | ``` : コマンドを次のコマンドの標準入力とする  
コマンド履歴を先頭10件だけ表示させる場合  
```
history | tail

  217  cd ../
  218  cd sample4 || echo "cd command is failed"
  219  ls
  220  rm -r sample*
  221  ls
  222  ll -a
  223  echo "string"
  224  echo 1111
  225  history | head
  226  history | tail
```  

リダイレクト ``` > ``` : 標準出力先を変更する  
上記のコマンドをhistory.txtというファイルに出力する場合  
```
ls
history | tail > history.txt
ls
cat history.txt

  219  ls
  220  rm -r sample*
  221  ls
  222  ll -a
  223  echo "string"
  224  echo 1111
  225  history | head
  226  history | tail
  227  ls
  228  history | tail > history.txt
```  

リダイレクト(追記) ``` >> ``` : 標準出力先に追記する  
history.txtというファイルに追記する  
```
echo end-of-file >> history.txt

  219  ls
  220  rm -r sample*
  221  ls
  222  ll -a
  223  echo "string"
  224  echo 1111
  225  history | head
  226  history | tail
  227  ls
  228  history | tail > history.txt
end-of-file
```  

``` head ``` : 先頭10行だけ表示する  
ホームディレクトリで、.bashrcを表示  
```
head .bashrc

# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

```  

``` tail ``` : 末尾10行だけ表示する  
ホームディレクトリで、.bashrcを表示  
```
tail .bashrc

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```  

``` sort ``` : 出力の順番を変える(ファイルなど保存はしない)  
num.txtを表示
``` 
cat num.txt

14
23
44
51
63
21
42
12
13
82
2
4
98
9
12
32
52


```  

num.txtをソートで表示  
```
sort num.txt

12
12
13
14
2
21
23
32
4
42
44
51
52
63
82
9
98
```

``` uniq ``` : 重複を取り除いて表示  
num.txtを重複を取り除いて表示  
```
uniq num.txt

14
23
44
51
63
21
42
12
13
82
2
4
98
9
12
32
52
```  

``` grep ``` : 検索に該当する行だけを表示  
num.txtで1がある行だけを表示  
```
grep 1 num.txt

14
51
21
12
13
12
```  

``` find ``` : 検索に合うファイルを表示する  
"txt"という名前を含むファイルを表示  
```
find *.txt

history.txt
num.txt
```  


## vi / vim
- CUI用エディタ
- CentOSやUbuntuでは標準でサポートしている
- サーバ上の設定ファイルの変更などに使用

### 最低限覚えること
1. ``` vi ファイル名 ``` でviもしくはvimを新規ファイルに対して起動する
2. ``` h, j, k, l ``` でカーソル移動する
3. インサートモードで文字入力
    1. ``` i ``` : カーソル位置から文字入力を始める
    2. ``` a ``` : カーソル位置から一文字前に進んで文字入力を始める 
4. インサートモードからノーマルモードに戻る
    1. ``` i ``` や ``` a ``` でインサートモードに移動
    2. ``` Esc ``` でノーマルモードに戻る
5. ファイルを保存する
    1. ノーマルモードで ``` :wq ``` もしくは ``` :wq! ```でコマンドラインのプロンプトに戻る

