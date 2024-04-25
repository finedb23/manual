# モデル select文

## id検索があるフォーム


- フォーム画面

```php
<p>名前検索</p>

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
        $items=Person::nameEqual($input)->get();
    } else {
        $items = Person::all();
    }
    return view('hello.index', ['input'=>$input,'items' => $items]);
}
```

- モデル

```php
class Person extends Model {
    use HasFactory;

    public function getData() {
        return $this->id . ': ' . $this->name . '(' . $this->age . ')';
    }

    public function scopeNameEqual($query, $str) {
        return $query->where('name',$str);
    }
}
```

- ルーター

```php
Route::get('hello',[HelloController::class,'index'])->name('hello.index');
```