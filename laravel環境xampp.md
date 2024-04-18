# Laravelの環境構築 Xampp

[Xamppのインストール](https://www.apachefriends.org/jp/download.html)

### XAMPPコントローラで「emEditor」がアクセス拒否される対処法	

-	「xampp-control.ini」ファイルのアクセスをフルコントロールに変更する

### 環境PATHの設定

(追加)
- C:\xampp\php
- C:\xampp\mysql\bin

### hostsファイルの場所

- C:\Windows\System32\drivers\etc/hosts

### MySQLのrootパスワードの変更
(開発中のときは設定しなくてもよい)

- mysqladmin -u root password

### MySQLのテーブルにテキストファイル(db.sql)をインポートする方法

**ログイン**

```
mysql -u root -p
データベースの作成
CREATE DATABASE selfphp CHARACTER SET utf8;
USE selfphp
インポート
source C:/db.sql  ファイルをCドライブに置く
```

データベース用の新規ユーザーの設定

```
GRANT ALL PRIVILEGES ON selfphp.* TO selfusr@localhost IDENTIFIED BY 'selfpass';
```

## Composerのインストール

[composerのダウンロード](https://getcomposer.org/download/)

## Laravelプロジェクトのインストール

**zipが有効ではないとエラーになるので、php.iniのextension=zipをコメントアウトする**

```
extension=zip 962行目
```

## プロジェクト作成コマンド

```
composer create-project laravel/laravel プロジェクト名 --prefer-dist "10.*"
```

**プロジェクトフォルダに移動する**

### バージョン確認

```
php artisan -v
```

### 簡易サーバーの起動

```
 php artisan serve
```


# 手軽に使えるユーザー登録・ログイン機能をインストールする Breeze

```
$ composer require laravel/breeze --dev
   INFO  Discovering packages.  

  laravel/breeze .................................................................................................................. DONE
  laravel/sail .................................................................................................................... DONE
  laravel/tinker .................................................................................................................. DONE
  nesbot/carbon ................................................................................................................... DONE
  nunomaduro/collision ............................................................................................................ DONE
  nunomaduro/termwind ............................................................................................................. DONE
  spatie/laravel-ignition ......................................................................................................... DONE

85 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan vendor:publish --tag=laravel-assets --ansi --force

   INFO  No publishable resources for tag [laravel-assets].  

No security vulnerability advisories found.
Using version ^2.0 for laravel/breeze
```

- breezeのインストール

```
$ php artisan breeze:install

 ┌ Which Breeze stack would you like to install? ───────────────┐
 │ Blade with Alpine                                            │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like dark mode support? ───────────────────────────┐
 │ No                                                           │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ PHPUnit                                                      │
 └──────────────────────────────────────────────────────────────┘

   INFO  Installing and building Node dependencies.  
```

## ロケールとタイムゾーンの設定

- 環境ファイル「env」の設定
    - APP_TIMEZONE=Asia/Tokyo
    - APP_LOCALE=ja
    - APP_FAKER_LOCALE=ja_JP

- 翻訳ファイルの配置場所

翻訳ファイルの置き場所は、「プロジェクト直下のlangの中」です。

下記のコマンドを実行して、langを作成する必要があります。

```
sail artisan lang:publish
```

## PHP 8.2へのバージョンアップ

[参考にしたサイト](https://php.watch/articles/install-php82-ubuntu-debian)
```
1.PHP拡張機能のリストを一覧表示して保存します
sudo dpkg -l | grep php | tee packages.txt
2.PPAをソフトウェアリポジトリとして追加します。ondrej/php
sudo add-apt-repository ppa:ondrej/php # Press enter when prompted.
sudo apt update
3. PHP 8.2 と拡張機能をインストールする
sudo apt install php8.2 php8.2-cli php8.2-{bz2,curl,mbstring,intl}
4. サーバー API のインストールと有効化
sudo apt install php8.2-fpm
# OR
# sudo apt install libapache2-mod-php8.2

sudo a2enconf php8.2-fpm

# When upgrading from older PHP version:
sudo a2disconf php8.1-fpm

## Remove old packages
sudo apt purge php8.1*
```

## Laravel Breeze日本語化パッケージ

(Laravel Breeze 日本語パッケージ)[https://github.com/askdkc/breezejp]

- jp.jsonファイルをlangディレクトリにコピーする