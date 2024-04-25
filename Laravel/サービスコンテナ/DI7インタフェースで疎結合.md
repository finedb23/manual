# インタフェースで疎結合にする

引数と戻り値を設定できる

‐ インタフェース

```php
namespace App\MyClasses;

interface MyServiceInterface{
    public function setId(int $id);
    public function say();
    public function data(int $id);
    public function alldata();
}
```

- サービスファイル

```php
class MyService6 implements MyServiceInterface {
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
        app()->bind(MyServiceInterface::class,MyService6::class);
    }
}
```

- コントローラ

```php
class SampleController extends Controller {

    public function index(MyServiceInterface $myservice,int $id=-1) {
        $myservice->setId(2);
        $data=[
            'msg'=>$myservice->say($id),
            'data'=>$myservice->alldata()
        ];
        return view('sample.index',$data);
    }
}
```

‐ ビュー

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
