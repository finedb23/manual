# モデル update文

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
public function edit(Request $request) {
    $person=Person::find($request->id);
    return view('hello.edit',['form'=>$person]);
}
public function update(Request $request) {
    $this->validate($request, Person::$rules);
    $person=Person::find($request->id);
    $form=$request->all();
    unset($form['_token']);
    $person->fill($form)->save();
    return redirect('/hello');
}
```

- ビュー

```php
@if (count($errors)>0)
<div>
    <ul>
        @foreach($errors->all() as $error)
        <li>{{$error}}</li>
        @endforeach
    </ul>
</div>
@endif
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

- ルーター

```php
Route::get('hello/edit',[HelloController::class,'edit']);
Route::post('hello/update',[HelloController::class,'update']);
```

