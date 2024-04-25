# コンストラクタに引数を設定する

app/Http/MyClasses/MyService2.phpを作成する


```php
namespace App\MyClasses;

class MyService2{
    private $id=-1;
    private $msg='no id...';
    private $data=['Hello','Welocome','Bye'];

    public function __construct(int $id=-1)
    {
        if($id>=0){
            $this->id=$id;
            $this->msg='select:'.$this->data[$id];
        }
    }

    public function say() {
        return $this->msg;
    }
    public function data(int $id) {
        return $this->data[$id];
    }
    public function alldata() {
        return $this->data;
    }
}
```

- コントローラ

```php
class SampleController extends Controller {

    public function index(int $id=1) {

        $myService=app()->makeWith(MyService2::class,['id'=>$id]);
        $data=[
            'msg'=>$myService->say(),
            'data'=>$myService->alldata()
        ];

        return view('sample.index',$data);
    }
}
```

- ビュー

```php
<h1>Sample/Index</h1>

<p>{!! $msg !!}</p>

<ul>
    @foreach ($data as $item)
        <li>{!! $item !!}</li>
    @endforeach
</ul>
```

- ルータ

```php
Route::get('sample', [SampleController::class, 'index'])->name('sample');
```



