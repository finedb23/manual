# モデル insert文

- モデル

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
public function create(Request $request) {
    return view('hello.create');
}
public function store(Request $request) {
   $this->validate($request, Person::$rules);
    $person = new Person();
    $form = $request->all();
    unset($form['_token']);
    $person->fill($form)->save();
    return redirect('/hello');

    または、
    $person=new Person();
    $person->fill($request->except('_token'))->save();
    return redirect('/hello/create');
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
<form action="/hello/store" method="post">
    <table>
        @csrf
        <tr>
            <th>name:</th>
            <td><input type="text" name="name" value="{{old('name')}}"></td>
        </tr>
        <tr>
            <th>mail:</th>
            <td><input type="text" name="mail" value="{{old('mail')}}"></td>
        </tr>
        <tr>
            <th>age:</th>
            <td><input type="text" name="age" value="{{old('age')}}"></td>
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
Route::get('hello/create',[HelloController::class,'create']);
Route::post('hello/store',[HelloController::class,'store']);
```

