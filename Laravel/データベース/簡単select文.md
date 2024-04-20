# SELECT文 簡単

## パラメータあり

- 一覧

```php
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
            <td>{{$item->age}}</td>
        </tr>
    @endforeach
</table>
```

- コントローラー

```php
public function index(Request $request) {

    if(isset($request->id)){
        $param=['id'=>$request->id];
        $items=DB::select('select * from people where id=:id',$param);
    }else{
        $items=DB::select('select * From people');
    }
    return view('hello.index',['items'=>$items]);
}
```

- ルーター

```php
Route::get('hello',[HelloController::class,'index'])->name('hello.index');
```