# コンストラクタに引数を設定する

app/Http/MyClasses/MyService3.phpを作成する


```php
namespace App\MyClasses;

class MyService3 {
    private $id = -1;
    private $msg = 'no id...';
    private $data = ['Hello', 'Welocome', 'Bye'];

    public function __construct() {
    }

    public function setId($id) {
        $this->id = $id;
        if ($id >= 0 && $id < count($this->data)) {
            $this->msg = "select id:" . $id . ', data:"' . $this->data[$id] . '"';
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

- サービスプロバイダー AppServiceProvider.php

```php
class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        app()->bind(MyService3::class,function($app){
            $myservice=new MyService3();
            $myservice->setId(0);
            return $myservice;
        });
    }
}
```

- コントローラ

```php
class SampleController extends Controller {

    public function index(MyService3 $myservice,int $id=1) {

        $myservice->setId($id);
        $data=[
            'msg'=>$myservice->say($id),
            'data'=>$myservice->alldata()
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



