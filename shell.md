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
echo 'expr $NUM + 1`
echo 'expr $NUM - 1`
echo 'expr $NUM \* 10`
echo 'expr $NUM / 2`
echo 'expr $NUM % 3`
```