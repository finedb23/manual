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