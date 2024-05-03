# 最も基本なPHP接続

```php
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>mysql 接続</title>
</head>
<body>
  <?php
$dsn      = 'mysql:dbname=testdb;host=localhost';
$user     = 'root';
$password = '';

// DBへ接続
try {
    $dbh = new PDO($dsn, $user, $password);

    // クエリの実行
    $query = "SELECT * FROM testtbl";
    $stmt = $dbh->query($query);
?>
<table border="1">
    <tr>
        <th>ID</th>
        <th>氏名</th>
        <th>年齢</th>
    </tr>

    <?php

    // 表示処理
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo '<tr>';
        echo "<td>{$row['id']}</td>";
        echo "<td>{$row['name']}</td>";
        echo "<td>{$row['age']}</td>";
        echo '</tr>';
    }
} catch (PDOException $e) {
    print("データベースの接続に失敗しました" . $e->getMessage());
    die();
}

// 接続を閉じる
$dbh = null;
?>  
</table>
</body>
</html>
```