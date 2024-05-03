# postgreSQL テーブルを構築

```
CREATE USER ems WITH PASSWORD 'pass';
CREATE DATABASE testdb OWNER ems ENCODING 'UTF8';
CREATE TABLE testtbl(
    id SERIAL PRIMARY KEY,
    name text,
    tokuten int
);

INSERT INTO testtbl (name,tokuten) VALUE('t.yamada',63);
INSERT INTO testtbl (name,tokuten) VALUE('m.ogura',75);
INSERT INTO testtbl (name,tokuten) VALUE('j.kawasaki',57);
INSERT INTO testtbl (name,tokuten) VALUE('a.tokoro',82);
INSERT INTO testtbl (name,tokuten) VALUE('n.takaya',53);
```

- コマンドプロンプトを開く。「temp」フォルダを作成する

```
cd C:\temp
```