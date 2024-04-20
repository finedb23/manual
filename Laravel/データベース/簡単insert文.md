# 簡単 insert文

‐ form画面

```php
<form action="/hello/store" method="post">
    <table>
        @csrf
        <tr>
            <th>name:</th>
            <td><input type="text" name="name" id=""></td>
        </tr>
        <tr>
            <th>mail:</th>
            <td><input type="text" name="mail" id=""></td>
        </tr>
        <tr>
            <th>age:</th>
            <td><input type="text" name="age" id=""></td>
        </tr>
        <tr>
            <th></th>
            <td><input type="submit" value="send"></td>
        </tr>
    </table>
</form>
```

- コントローラー

```php
public function add(Request $request){
    return view('hello.add');
}
public function create(Request $request){
    $param=[
        'name'=>$request->name,
        'mail'=>$request->mail,
        'age'=>$request->age,
    ];
    DB::insert('insert into people(name,mail,age) values(:name,:mail,:age)',$param);
    return redirect('/hello');
}
```

‐ ルーター

```php
Route::get('hello/add',[HelloController::class,'add']);
Route::post('hello/store',[HelloController::class,'create']);
```
