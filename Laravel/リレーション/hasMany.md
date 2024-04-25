# hasMany結合

## boardテーブルからすく数行を取得する

-- モデル

```php
class Board extends Model
{
    use HasFactory;

    protected $guarded=['id'];

    public static $rules=[
        'person_id'=>'required',
        'title'=>'required',
        'message'=>'required'
    ];

    public function getData() {
        return $this->id.' : '.$this->title;
    }

    // 1対多
    public function boards() {
        return $this->hasMany(new Board());
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
    @foreach ($items as $item)
    <tr>
        <td>{{$item->getData()}}</td>
        <td>@if($item->board!=null)
            <table width="100%">
                @foreach($item->boards as $obj)
                <tr>
                    <td>{{$obj->getData()}}</td>
                </tr>
                @endforeach
            </table>
            @endif
        </td>
    </tr>
    @endforeach
</table>
```

ルータ

```php
Route::get('board',[BoardController::class,'index']);
```

