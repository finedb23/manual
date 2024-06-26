# モデル select文

## id検索があるフォーム


- フォーム画面

```php
<p>検索</p>

<form action="/hello" method="get">
    <input type="text" name="input" value="{{$input}}">
    <input type="submit" value="find">
</form>

<table border="1">
    <tr>
        <th>Name</th>
        <th>Mail</th>
        <th>Age</th>
    </tr>
    @foreach ($items as $item)
        <tr>
            <td>{{$item->name}}</td>
            <td>{{$item->mail}}</td>
            <td>{{$item->getData()}}</td>
        </tr>
    @endforeach
</table>
```

‐ コントローラー

```php
public function index(Request $request) {
    $input='';
    if (isset($request->input)) {
        $input=$request->input;
        $items = Person::where('id',$input)->get(); // 1件だけ取得する場合はfind($id)を使う
    } else {
        $items = Person::all();
    }

    // ローカルスコープの使い方
    $item=Person::nameEqual($request->input)->first();

    // ○以上○以下
        $min=$request->input*1;
        $max=$min+10;
        $item=Person::ageGreaterThan($min)-    >ageLessThan($max)->first();
        $param=['input'=>$request->input,'item'=>$item];
        return view('person.find',$param);
    // ○以上○以下 ここまで

    return view('hello.index', ['input'=>$input,'items' => $items]);
}
```

- モデル
  - ローカルスコープ

```php
class Person extends Model {
    use HasFactory;

    public function getData() {
        return $this->id . ': ' . $this->name . '(' . $this->age . ')';
    }
    // ローカルスコープ
    public function scopeNameEqual($query, $str) {
        return $query->where('name',$str);
    }
    // ○以上○以下
    public function scopeAgeGreaterThan($query,$n) {
        return $query->where('age','>=', $n);
    }

    public function scopeAgeLessThan($query,$n) {
        return $query->where('age','<=',$n);
    }

    // グローバルスコープ
    // bootメソッドをオーバーライドして利用する
    // use Illuminate\Database\Eloquent\Builder;
    protected static function boot() {
        parent::boot();

        static::addGlobalScope('age',function(Builder $builder) {
            $builder->where('age','>',20);
        });
    }
}
```

- ルーター

```php
Route::get('hello',[HelloController::class,'index'])->name('hello.index');
```