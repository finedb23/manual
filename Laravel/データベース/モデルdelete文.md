# モデル delete文

- モデル
    - insertと同じ
```php
class Person extends Model {
    use HasFactory;

    protected $guarded=['id'];

    public static $rules=[
        'name'=>'required',
        'mail'=>'email',
        'age'=>'integer|min:0|max:150',
    ];

    public function getData() {
        return $this->id . ': ' . $this->name . '(' . $this->age . ')';
    }
}
```

- コントロール

```php
public function delete(Request $request) {
    $person=Person::find($request->id);
    return view('hello.delete',['form'=>$person]);
}
public function destory(Request $request) {
    Person::find($request->id)->delete();
    return redirect('/hello');
}
```

- ビュー

```php
<form action="/hello/destory" method="post">
    <table>
        @csrf
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

- ルーター

```php
Route::get('hello/delete',[HelloController::class,'delete']);
Route::post('hello/destory',[HelloController::class,'destory']);
```

