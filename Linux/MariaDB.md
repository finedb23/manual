# MariaDBをインストールする方法

‐ インストール

```
sudo apt install mariadb-server -y
```

- ステータス

```
systemctl status mariadb
```

‐ バージョン確認

```
sudo mysql -v
```


- 起動

```
systemctl restart mariadb
```

- 再起動

```
systemctl start mariadb
```

- 停止

```
systemctl stop mariadb
```

# 権限の設定

- 権限設定の手順

```
sudo mariadb
GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY 'パスワード(空白にする)' WITH GRANT OPTION;

フラッシュする
FLUSH PRIVILEGES;
```

- Ctrl+Cで終了する



- ログイン

```
mysql -u root -p
パスワードは空白
```

