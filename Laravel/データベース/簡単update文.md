# 簡単 Update文

‐ フォーム画面
```php
<form action="/hello/update" method="post">
    <table>
        @csrf
        <input type="hidden" name="id" value="{{$form->id}}">
        <tr>
            <th>name:</th>
            <td><input type="text" name="name" value="{{$form->name}}"></td>
        </tr>
        <tr>
            <th>mail:</th>
            <td><input type="text" name="mail" value="{{$form->mail}}"></td>
        </tr>
        <tr>
            <th>age:</th>
            <td><input type="text" name="age" value="{{$form->age}}"></td>
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
public function edit(Request $request) {
    $param=['id'=>$request->id];
    $item=DB::select('select * from people where id=:id',$param);
    return view('hello.edit',['form'=>$item[0]]);
}
public function update(Request $request){
    $param=[
        'id'=>$request->id,
        'name'=>$request->name,
        'mail'=>$request->mail,
        'age'=>$request->age,
    ];
    DB::update('update people set name=:name, mail=:mail,age=:age where id=:id',$param);
    return redirect('/hello');
}
```

- ルーター

```Php
Route::get('hello/edit',[HelloController::class,'edit']);
Route::post('hello/update',[HelloController::class,'update']);
```
