# Laravelの環境構築 Docker

## WSLがインストールされていないことを確認する

コントロールパネル>プログラムと機能>Windowsの機能の有効化または無効化

- Linux用Windowsサブシステムにチェックする
- 仮想マシン プラットフォームにチェックする

## WSL2のインストール

PowerShellを管理者で実行する

- WSLをインストールする

```
wsl --install
```

- ユーザー名をパスワードを設定する

- コマンドシェルの種類を調べる

```
$ echo $SHELL
/bin/bash
```

## Docker Desktopをインストールする

[Dockerダウンロードサイト](https://www.docker.com/products/docker-desktop/)

インストール後

設定>Resources>WSL Integration

- [ ] Ubuntuを有効にする

## 正しくインストールできたか確認する

```
$ wsl --list --verbose
  NAME                   STATE           VERSION
* Ubuntu                 Running         2
  docker-desktop-data    Running         2
  docker-desktop         Running         2
```

# プロジェクトの新規作成

- VSCodeが立ち上がっていたら再起動する
```
$ curl -s https://laravel.build/test-project | bash
```

- プロジェクトディレクトリに移動する
-Lravel Sailを起動する

```
$./vendor/bin/sail up
```

- Laravelのトップ画面を表示する

```
http://localhost/
```

- Laravel Sailを停止する

```
^C コマンド
```

- Laravel Sailをバックグラウンド起動する

```
$ ./vendor/bin/sail up -d
```

- Laravel Sailを停止する

```
$ ./vendor/bin/sail stop
```

## Docker Desktopの自動起動の停止

Docker Desktopは、OSの起動時に自動的に起動するように設定されています。
この設定を変更するには、Docker Desktopの[設定]画面を開き、[General]メニューで[Start Docker Desktop when you sign in to your computer]をオフにします。


## Laravelの起動のエイリアスを設定する

```
$ ./vendor/bin/sail up -d
```
このコマンドのエイリアスを作る

- まず、プロジェクトのあるディレクトリに移動する
- sailを止める

```
$ ./vendor/bin/sail stop
```

- WSL内のホームディレクトリにある.bashrcファイルを編集する


- VSCodeを起動する
- 左下のWSL(青)をクリックする
- 上の検索ボックスから
Linux>Ubuntu>home>ems
に移動する

- .bashrcファイルの最後に、次の行を追加する

```
alias sail="./vendor/bin/sail"
```

- 設定を反映する

```
$ source ~/.bashrc
```

- 正しく設定されているかどうかを確認する

```
$ alias sail
alias sail='./vendor/bin/sail'
```

##  phpMyAdminの追加

設定を始める前に、Laravel Sailが起動している場合は、停止しておきましょう。

```
$ sail stop
```

- プロジェクトの中にあるdocker-compose.ymlを編集する

**redis: の前に記述する**
```
    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        links:
            - mysql:mysql
        ports:
            - 8080:80
        environment:
            MYSQL_USERNAME: '${DB_USERNAME}'
            MYSQL_ROOT_PASSWORD: '${DB_PASSWORD}'
            PMA_HOST: mysql
        networks:
            - sail
```

## Laravel Sailを起動する

```
$ sail up -d
```

## phpMyAdminへログインする

```
http://localhost:8080/
ユーザー名: sail
パスワード: password
```

## Laravelを起動する

```
http://localhost/
```

エラーが出たとき
```
SQLSTATE[42S02]: Base table or view not found: 1146 Table 'cloud18i_dbpethu.sessions' doesn't exist
```

migrateを実行する

```
$ sail artisan migrate
```

# 手軽に使えるユーザー登録・ログイン機能をインストールする

[composerのダウンロード](https://getcomposer.org/download/)

- Breezeパッケージの追加


```
$ sail composer require laravel/breeze --dev
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
$ sail artisan breeze:install

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