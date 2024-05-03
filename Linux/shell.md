# シェルコマンド

dash(Debian系) > bash > Bourne Shell(UNIX)

最も単純なスクリプト

```shell
backup.sh
mkdir backup
cp *.txt ./backup
echo "Completed"
```
実行
```
$ sh backup.sh
または
$ bash backup.sh (bashは機能拡張版)
```
# スクリプトファイルをコマンド化

シェルスクリプトを記述したファイルそのものを単独コマンドにする

1. スクリプトファイルの先頭に「#!/bin/sh」と書く。

```shell
backup.sh
#!/bin/sh

mkdir backup
cp *.txt ./backup
echo "Completed"
```

2. chmodコマンドで実行属性を与える

```
$ chmod +x backup.sh
あるいは
$ chmod 755 src.sh
chmod u+x src.sh
```
ファイルに実行属性「x」が付き、単体での実行が可能になります。

3. 単独コマンドとして実行する。

```
$ ./backup.sh
```
# 変数

代入 ```VAR="ABCDE"```注意:空白は開けてはいけません

参照① ```$VAR``` $をつける

参照② ```${VAR}``` {}をつけることもできる

```
VAR="ABCDE"
echo "${VAR}"
```

# 数値計算
シェルスクリプトは、通常は数値演算式を解釈できません。

## ①算術演算を評価する((〜))記法
```
I=10
echo $((I*2)) // 20
echo $((I-2)) // 8
```

## ②外部コマンドexprで演算
「expr」という外部コマンドの機能を借りることで数値計算を実現できます。

```
NUM=10
echo `expr $NUM + 1`
echo `expr $NUM - 1`
echo `expr $NUM \* 10`
echo `expr $NUM / 2`
echo `expr $NUM % 3`
```

# コマンド実行結果に置き換える
## バッククォートで囲む方法

```
NUM=1
NUM=`expr $NUM + 1`
echo $NUM
```

## $()で囲む方法

```
DATE=$(date)
echo "$DATE"
```

# 環境変数

シェル変数をexportコマンドにより環境変数に登録するとそれを引き継ぐことができます。

```
env1.sh
echo "$EVAR"

env2.sh 環境変数にEVARを登録する
export EVAR
「env1.sh」を実行すると 「echo "$EVAR"」が実行される
sh ./env1.sh
```

sourceコマンドでスクリプトファイルを実行します。

```
env.sh
EVAR=ABCD
export EVAR
```
スクリプトに書かれている内容を1行ごとにシェルのコマンドライン上に実行させるものです。
```
source env.sh
echo $EVAR
ABCD
```

# 評価 testコマンド

```
test.sh
echo test "$1" -eq "100"
echo $?
「$?」は直前に実行したコマンドの終了ステータスです。
```

実行

```
sh test.sh 100
sh test.sh 90
```

## test コマンド評価式の書式

文字列の比較

|名称|説明
|-|-
|文字列A = 文字列B|文字列AとBが同一である
|文字列A ！= 文字列B|文字列AとBが異なる
|-z 文字列|文字列の長さが0である
|-n 文字列|文字列の長さが1以上である
|文字列|文字列が空文字ではない

数値として比較
|名称|説明
|-|-
|数値A -eq 数値B|数値A = 数値Bである
|数値A -ne 数値B|数値A /= 数値Bである
|数値A -lt 数値B|数値A < 数値Bである
|数値A -le 数値B|数値A <= 数値Bである
|数値A -gt 数値B|数値A > 数値Bである
|数値A -ge 数値B|数値A >= 数値Bである

ファイルの属性評価
|名称|説明
|-|-
|-e FILE|FILEは存在する
|-f FILE|通常ファイルである
|-d FILE|ディレクトリである
|-L FILE|シンボリックリンクである
|-c FILE|キャラクタ特殊ファイルである
|-b FILE|ブロック特殊ファイルである
|-s FILE|ソケットである
|-p FILE|名前付きパイプである
|-r FILE|読み出し可能である
|-w FILE|書き込み可能である
|-x FILE|実行可能である
|-s FILE|ファイルサイズが0以上である
|FILE1 -nt FILE2|FILE1がFILE2より新しい
|FILE1 -ot FILE2|FILE1がFILE2より古い
|FILE1 -ef FILE2|FILE1とFILE2はデバイス番号、およびiノード番号が同一である

条件AND・OR連結
|名称|説明|備考
|-|-|-
|-a|&&|AND (論理積)
|-o| ｜｜|OR(論理和)

# 条件分岐 if

## 注意 角括弧を書くときは前後にスペースを開けること
基本形
```
if [ $="A" ]
then
    echo "Aです"
fi
```

if else ifの基本形
```
if [ $1 = "A" ]
then
    echo "Aである"
elif[ $1 = "B" ]
then
    echo "AではなくBである"
else
    echo "AでもBでもない"
fi
```

# 条件繰り返し
## while

基本形
```
I=0
while [ $I -lt 3 ]
do
    echo "$I"
    I=`expr $I + 1`
done
```
$Iのカウントは、下記のようにもできる
```
I=0
while [ $I -lt 3 ]
do
    echo "$I"
    I=$((I+1))
done
```
注意: Amazonの本では「((I=I+1))」となっているがエラーになる

### 永久ループ

while true は永久ループになる

breakでループから抜ける
```
I=1
while true
do
    echo "$I"
    I=$((I+1))
    if [ $I -gt 3 ]
    then
        break
    fi
done
```


```
I=1
while true
do
    echo "count $I"
    I=$((I+1))
    if [ $I -le 3 ]
    then
        continue
    else
        break
    fi
    echo "next"
done
```

# リスト走査 for文

```
for S in AAA BBB CCC
do
    echo $s
done
```
### 拡張子が「*.txt」を表示する
```
for S in `ls *.txt`
  do
    echo $S
done
```

$@はコマンドライン引数のリストを格納しています。
```
alist.sh
for S in $@; do
    echo $S
done
```
実行:```$ sh alist.sh AA BB CC```

