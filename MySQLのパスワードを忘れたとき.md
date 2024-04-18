# MySQLのパスワードを忘れたとき

- xampp-mysql-bin-my.iniファイルに下記の２行を追加する

```
[mysqld]
skip-grant-tables
```

- XAMPPを再起動する
mysql -u root でログインする
use mysql を選択
- 以下の命令で権限情報のキャッシュ削除し、再度新しく権限情報を読み込み	
FLUSH PRIVILEGES; 
- 以下の命令で新しいパスワードを再設定。	
ALTER USER root@localhost identified BY '新しいパスワード';
- 新しいパスワードを再設定後、MySQLをログアウト	
quit
- シェルを終了	
exit