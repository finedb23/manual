#  mysqlにデータを取り込む方法

## サンプルデータ

```sql
-- testdbが存在する場合のみ、testdbを削除するSQL文。
DROP TABLE IF EXISTS testtbl;

CREATE TABLE testtbl(
    id int PRIMARY KEY AUTO_INCREMENT,
    name varchar(100),
    age int
);

INSERT INTO testtbl (name,age) VALUE('山田',49);
INSERT INTO testtbl (name,age) VALUE('北川',34);
INSERT INTO testtbl (name,age) VALUE('高橋',21);
INSERT INTO testtbl (name,age) VALUE('小林',35);
INSERT INTO testtbl (name,age) VALUE('木下',18);
```

- 接続する

```
mysql -u root -p
パスワードは空
```

- データベースを作成する

```sql
CREATE DATABASE testdb;
USE testdb;
```

- 取り込む

```
source c:\temp\db.sql;
```