# 簡単なサンプル

app/MyClassesにMyService.phpを作成する

```php
namespace App\MyClasses;

class MyService{
    private $msg;
    private $data;

    public function __construct()
    {
        $this->msg='Hello! This is MyService!';
        $this->data=['Hello','Welcome','Bye'];
    }

    public function say() {
        return $this->msg;
    }
    public function data() {
        return $this->data;
    }
}
```

- コントローラ

```php
class SampleController extends Controller {
    public function index(MyService $myService) {
        $data=[
            'msg'=>$myService->say(),
            'data'=>$myService->data()
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

