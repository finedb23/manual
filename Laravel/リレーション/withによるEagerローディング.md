# withによるEagerローディング

- モデル

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
class BoardController extends Controller
{
    public function index()
    {
        $items=Board::with('person')->get();
        return view('board.index', compact('items'));
    }
}
```

- ビュー
```php
<table>
    <tr>
        <th>Message</th>
        <th>Name</th>
    </tr>
    @foreach($items as $item)
    <tr>
        <td>{{$item->message}}</td>
        <td>{{$item->person->name}}</td>
    </tr>
    @endforeach
</table>
```

- ルーター

```php
Route::get('board',[BoardController::class,'index']);
```

