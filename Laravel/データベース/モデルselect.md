# モデル select文

## php artisan make:modelコマンドのオプション

- --allオプション
- 
```
php artisan make:model Sample --all
```

- 生成ファイル
    - [name].php（モデル）
    - [name]Controller.php（コントローラ）
    - Store[name]Request.php（フォームリクエスト）
    - Update[name]Request.php（フォームリクエスト）
    - [name]Policy.php（ポリシー）
    - [name]Factory.php（ファクトリ）
    - 2022_xx_xx_xxxxxx_create_[name]s_table.php（マイグレーション）
    - [name]Seeder.php（シーダ）

### その他のオプション

- --controller オプション
- --factory オプション
- --force オプション モデルを上書きする
- --migration オプション
- --policy オプション
- --seed オプション
- --pivot オプション
- --resource オプション
- --api オプション
- ‐‐requests オプション
- --test オプション
- -- pest オプション PHPUnitコード

## マイグレーションファイルの設定

```php
public function up(): void
{
    Schema::create('people', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('mail');
        $table->integer('age');
        $table->timestamps();
    });
}
```

## データ型

- $table->integer(フィールド名);
- $table->bigInteger(フィールド名);
- $table->float(フィールド名);
- $table->double(フィールド名);
- $table->char(フィールド名);
- $table->string(フィールド名);
- $table->text(フィールド名);
- $table->longText(フィールド名);
- $table->boolean(フィールド名);
- $table->date(フィールド名);
- $table->datetime(フィールド名);

## マイグレーション オプション

- migrate:status ステータスの確認
- migrate 実行
- migrate:refresh データベースをすべて自動的にrollbackして、migrateを実行する

# シーダーの実行

- テーブル名のシーダーにダミーデータを登録する

```php
class PersonSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        $param=[
            'name'=>'taro',
            'mail'=>'taro@yamada.jp',
            'age'=>12,
        ];
        DB::table('people')->insert($param);
        $param=[
            'name'=>'hamako',
            'mail'=>'hamako@flower.jp',
            'age'=>34,
        ];
        DB::table('people')->insert($param);
        $param=[
            'name'=>'sachiko',
            'mail'=>'sachiko@happy.jp',
            'age'=>56,
        ];
        DB::table('people')->insert($param);
    }
}
```

- DatabaseSeederの設定

```php
class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     */
    public function run(): void
    {
        // \App\Models\User::factory(10)->create();

        // \App\Models\User::factory()->create([
        //     'name' => 'Test User',
        //     'email' => 'test@example.com',
        // ]);
        $this->call(PersonSeeder::class);
    }
}
```

- コマンドの実行

```php
artisan db:seed
```




- フォーム画面

```php


‐ コントローラー

```php

- ルーター

```php
