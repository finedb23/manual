# hasOne結合

## boardテーブルから1件だけ取得する

-- モデル

```php
class Person extends Model {
    use HasFactory;

    protected $guarded=['id'];

    public static $rules=[
        'name'=>'required',
        'mail'=>'email',
        'age'=>'integer|min:0|max:150',
    ];

    public function getData() {
        return $this->id . ': ' . $this->name . '(' . $this->age . ')';
    }
    // 追加 hasOne 主ー>従 (従から1つだけデータを取得する)
    public function board() {
        return $this->hasOne(new Board);
    }
}
```

- コントローラ

```php
public function index() {
    $items = Person::all();
    return view('person.index',compact('items'));
}
```


ビュー

```php
<table>
    <tr>
        <th>Person</th>
        <th>Board</th>
    </tr>

    @foreach($items as $item)
    <tr>
        <td>{{$item->getData()}}</td>
        <td>@if($item->board!=null){{$item->board->getData()}}@endif</td>
    </tr>
    @endforeach
</table>
```

ルータ

```php
Route::get('board',[BoardController::class,'index']);
```

