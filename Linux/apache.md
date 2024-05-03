# apache インストール

- インストールされてるか確認する
```
apt list --installed | grep apache
```

- リポジトリにAahce2が存在しているか確認する
```
apt list|grep apache2
```

- バージョン
```
apachectl -v
```
- モジュールのリスト
```
apache2 -l (大文字)
```
- start
```
sudo service apache2 start
sudo systemctl start apache2
```
- stop
```
sudo service apache2 stop
sudo systemctl stop apache2
```
- restart
```
sudo service apache2 restart
sudo systemctl restart apache2
```
- 安全な再起動
```
sudo apachectl graceful
```
- status
```
sudo service apache2 status
sudo systemctl status apache2
/etc/init.d/apache2 status
```
- 設定のリロード
```
sudo systemctl reload apache2
```
- 自動起動の有効化
```
sudo systemctl enable apache2
```
- 自動起動の無効化
```
sudo systemctl disable apache2
```

## インストール

- インストールすると、起動もされている
- 再起動後、自動で起動されている
```
sudo apt install apache2
```

## ファイルは /var/www/htmlにある

## 権限の設定

1. 所有者がrootになっているので、ユーザーに委譲する

```
sudo chown -R ems:ems /var/www/html
```

1. 次に、所有者とグループに入っている人に書込実行読込の権限を与え、それ以外のユーザーには読込と実行のみできるようにする

```
sudo chmod 775 -R /var/www/html
```

## ファイアウォール

- ファイアウォールの状態を調べる

```
sudo systemctl status ufw
```

- ファイアウォールの有効化・無効化

```
ufw enable
ufw disable
```

```
sudo ufw app list
```

```
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

- Apache: このプロファイルは、ポート80（通常の暗号化されていないWebトラフィック）のみを開きます。
- Apache Full: このプロファイルは、ポート80（通常の暗号化されていないWebトラフィック）とポート443（TLS/SSL暗号化トラフィック）の両方を開きます。
- Apache Secure: このプロファイルは、ポート443 （TLS/SSL暗号化トラフィック）のみを開きます。

- 変更
```
sudo ufw allo 'Apache'
```

- ステータス
```
sudo ufw status
```