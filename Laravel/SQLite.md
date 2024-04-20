# SQLiteを使う

- [SQLite(ZIP)のインストール](https://www.sqlite.org/download.html)

sqlite3.exeをWindowsフォルダ内のsystem32フォルダに移動させる

## DB Browser for SQLiteの導入

-[DB Browser for SQLiteのインストール](https://sqlitebrowser.org/)

ファイルは、C:\Program Files\DB Browser for SQLiteの中にある

## サンプルデータベース

peopleテーブル

```sql
CREATE TABLE "people" (
	"id"	INTEGER,
	"name"	TEXT NOT NULL,
	"mail"	TEXT,
	"age"	INTEGER,
	PRIMARY KEY("id" AUTOINCREMENT)
);
```

## Laravelの接続設定

```
DB_CONNECTION=mysql => mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel => db.sqlite
DB_USERNAME=root
DB_PASSWORD=
```