# 引数を渡す

- サービスプロバイダー AppServiceProvider.php

```php
class MyService5 {
    private $serial;
    private $id = -1;
    private $msg = 'no id...';
    private $data = ['Hello', 'Welocome', 'Bye'];

    public function __construct(int $id) {
        $this->setId($id);
        $this->serial=rand();
        echo "[".$this->serial."]";
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

- コントローラ

```php
class SampleController extends Controller {

    public function index(MyService5 $myservice,int $id=-1) {

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

- サービスプロバイダー

```php
class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        app()->when(MyService5::class)
        ->needs('$id')
        ->give(1);
    }
}
```

- ルーター

```php
Route::get('sample', [SampleController::class, 'index'])->name('sample');
```
