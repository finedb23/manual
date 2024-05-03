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

さらに、上級の書き方
脆弱性対策
protected $fillable=['name','mail','age'];
```

- コントロール

```php
public function edit(Request $request) {
    $person=Person::find($request->id);
    return view('hello.edit',['form'=>$person]);
}

さらに、上級の書き方
return view('save.edit',['person'=>Person:findOrFail($id)]);


public function update(Request $request) {
    $this->validate($request, Person::$rules);
    $person=Person::find($request->id);
    $form=$request->all();
    unset($form['_token']);
    $person->fill($form)->save();
    return redirect('/hello');
}

    さらに、上級の書き方
public function update(Request $request,$id){
    $person=Person::findOrFail($id);
    $person=fill($reqquest->except('_token','_method'))->save();
    return redirect('hello/list');
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
<form action="/hello/update/{{ $person->id }}" method="post">
    <table>
        @csrf
        @method('PATCH') // 追加
        <input type="hidden" name="id" value="{{$form->id}}">// 削除
        <tr>
            <th>name:</th>
            <td><input type="text" name="name" value="{{$form->name}}"></td>
            <td><input type="text" name="name" value="{{old('name',$form->name)}}"></td>// 変更
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

さらに上級の書き方

Route::get('/book/{id}/edit',[HelloController::class,'edit']);
Route::patch('/book/{id}',[HelloController::class,'update']);
```

- 一覧の修正

```php
<td>
<a href="/save/{{ $record->id}}/edit">編集</a>|<a href="/save/{{ $record->id}}">削除</a></td>
```
