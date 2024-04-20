# 中級 selectクエリビルダ

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
    if (isset($request->id)) {
        $id = ['id' => $request->id];
        // 1件のデータを取得したいときは first()
        $items = DB::table('people')->where('id', $id)->first();
        // 配列で取得したい場合は get()
        $items = DB::table('people')->where('id', $id)->get();
    } else {
        $items = DB::table('people')->get();
    }
    return view('hello.index', ['items' => $items]);
}
```

- ルーター

```php
Route::get('hello',[HelloController::class,'index'])->name('hello.index');
```
