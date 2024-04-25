# RESTサービス (Webサービス)

1. コマンド入力
`php artisan make:model Restdata --all`

1. マイグレーションファイルの編集

```php
return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('restdatas', function (Blueprint $table) {
            $table->id();
            $table->string('message');
            $table->string('url');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('restdatas');
    }
};
```

1. シーダーファイルの編集

```php
class RestdataSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        $param=[
            'message'=>'Google Japan',
            'url'=>'https://www.google.co.jp',
        ];
        $restdata=new Restdata();
        $restdata->fill($param)->save();

        $param=[
            'message'=>'Yahoo Japan',
            'url'=>'https://www.yahoo.co.jp',
        ];
        $restdata=new Restdata();
        $restdata->fill($param)->save();

        $param=[
            'message'=>'MSN Japan',
            'url'=>'https://www.msn.com/ja-jp',
        ];
        $restdata=new Restdata();
        $restdata->fill($param)->save();
    }
}
```

```php
class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     */
    public function run(): void
    {
        $this->call(PersonSeeder::class);
        $this->call(BoardSeeder::class);
        $this->call(RestdataSeeder::class);
    }
}
```

1. モデルファイルの設定

```php
class Restdata extends Model
{
    use HasFactory;

    protected $table='restdata';
    protected $guarded=[];

    public static $rules=[
        'message'=>'required',
        'url'=>'required'
    ];

    public function getData() {
        return $this->id.':'.$this->message.'('.$this->url.')';
    }
}
```

1. ルーター

```php
Route::get('hello/rest',[HelloController::class,'rest']);
Route::post('/rest/store',[RestdataController::class,'store']);

Route::get('/rest',[RestdataController::class,'index']);
```

1. コントローラ

```php
class RestdataController extends Controller
{
    public function index()
    {
        $items=Restdata::all();
        return $items->toArray();
    }

    public function create()
    {
        return view('rest.create');
    }

    public function store(HttpRequest $request)
    {
        $restdata=new Restdata();
        $form=$request->all();
        unset($form['_token']);
        $restdata->fill($form)->save();
        return redirect('/rest');
    }

    public function show($id)
    {
        $item=Restdata::find($id);
        return $item->toArray();
    }
}
```

1. ビュー

rest/create.blade.php

```php
<form action="/rest/store" method="post">
    <table>
        @csrf
        <tr>
            <th>message:</th>
            <td><input type="text" name="message" value="{{old('message')}}"></td>
        </tr>
        <tr>
            <th>url:</th>
            <td><input type="text" name="url" value="{{old('url')}}"></td>
        </tr>
        <tr>
            <th></th>
            <td><input type="submit" value="send"></td>
        </tr>
    </table>
</form>
```

1. create.blade.phpを埋め込む

hello.rest.blade.php

```php
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>hello/Rest</title>
</head>
<body>
    <h1>Rest</h1>

    @include('rest.create')
</body>
</html>
```







