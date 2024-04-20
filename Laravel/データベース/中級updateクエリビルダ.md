# 中級 insertクエリビルダ

- フォーム画面

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

‐ コントローラー

```php
public function edit(Request $request) {
    $id=$request->id;
    $item=DB::table('people')->where('id',$id)->first();
    return view('hello.edit',['form'=>$item]);
}
public function update(Request $request){
    $param=[
        'id'=>$request->id,
        'name'=>$request->name,
        'mail'=>$request->mail,
        'age'=>$request->age,
    ];
    DB::table('people')->where('id',$request->id)->update($param);
    return redirect('/hello');
}
```

- ルーター

```php
Route::get('hello/edit',[HelloController::class,'edit']);
Route::post('hello/update',[HelloController::class,'update']);
```