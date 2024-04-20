# 中級 insertクエリビルダ

- フォーム画面

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

‐ コントローラー

```php
public function add(Request $request) {
    return view('hello.add');
}
public function create(Request $request) {
    $param = [
        'name' => $request->name,
        'mail' => $request->mail,
        'age' => $request->age,
    ];
    DB::table('people')->insert($param);
    return redirect('/hello');
}
```

- ルーター

```php
Route::get('hello/add',[HelloController::class,'add']);
Route::post('hello/store',[HelloController::class,'create']);
```