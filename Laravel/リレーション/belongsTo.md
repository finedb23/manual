# 子テーブルから親テーブルを参照する (逆バージョン)

- 指定のリレーションの値を持つ
モデル:has(リレーション名)->get();
- 指定のリレーションの値を持たない
モデル:doesntHave(リレーション名)->get();


- 子テーブル (Board)

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
        return $this->id.' : '.$this->title . ' ('.$this->person->name. ')';
    }

    // 従ー>主を見る
    public function person() {
        return $this->belongsTo(new Person());
    }
}
```

- コントローラ

```php
public function index() {
    // $items = Person::all();
    $hasItems=Person::has('boards')->get();
    $noItems=Person::doesntHave('boards')->get();
    return view('person.index',compact('hasItems','noItems'));

}
```

- ビュー

```php
<table>
    <tr>
        <th>Person</th>
        <th>Board</th>
    </tr>

    @foreach ($hasItems as $item)
    <tr>
        <td>{{$item->getData()}}</td>
        <td>
            <table width="100%">
                @foreach($item->boards as $obj)
                <tr>
                    <td>{{$obj->getData()}}</td>
                </tr>
                @endforeach
            </table>
        </td>
    </tr>
    @endforeach
</table>

<div style="margin:10px;">
<table>
    <tr>
        <th>Person</th>
    </tr>
    @foreach($noItems as $item)
    <tr>
        <td>{{$item->getData()}}</td>
    </tr>
    @endforeach
</table>
</div>
```

- ルータ

```php
Route::get('person',[PersonController::class,'index']);
```

