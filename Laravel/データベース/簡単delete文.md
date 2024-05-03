# 簡単 Delete文

‐ フォーム画面

```php
<form action="/hello/destory" method="post">
    <table>
        @csrf
        @method('DELETE')// 追加
        <input type="hidden" name="id" value="{{$form->id}}">
        <tr>
            <th>name:</th>
            <td>{{$form->name}}</td>
        </tr>
        <tr>
            <th>mail:</th>
            <td>{{$form->mail}}</td>
        </tr>
        <tr>
            <th>age:</th>
            <td>{{$form->age}}</td>
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
public function delete(Request $request){
    $param=['id'=>$request->id];
    $item=DB::select('select * from people where id=:id',$param);
    return view('hello.delete',['form'=>$item[0]]);
}
public function destory(Request $request) {
    $param=['id'=>$request->id];
    DB::delete('delete from people where id=:id',$param);
    return redirect('/hello');
}
更に上級の書き方

$b=Person::findOrFail($id);
$person->delete();
return redirect('hello');
```

- ルーター

```php
Route::get('hello/delete',[HelloController::class,'delete']);
Route::post('hello/destory',[HelloController::class,'destory']);
さらに、上級の書き方
Route::post('hello/{id}',[HelloController::class,'destory']);
```