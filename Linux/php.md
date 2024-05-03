# PHPインストール

- php を探す

```
which php
```

- バージョン確認

```
php -v
```

- php.iniを検索

```
php -i | grep php.ini
```

- インストール情報

```
apt show php
```

# インストールコマンド

- もし最新版のバージョンをインストールしたいときは、リポジトリを登録する

```
sudo add-apt-repository ppa:ondrej/php
```

```
sudo apt update
sudo apt upgrade -y
sudo apt install php -y
```

- mysqlが使えるようにする

```
sudo apt install php-mysql
```

