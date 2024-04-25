# シングルトーンを使う

app/Http/MyClasses/MyService3.phpを作成する

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
        app()->bind(MyService4::class,function($app){
            //  app()->singleton()に変更する！！！！！！！
            $myservice=new MyService4();
            $myservice->setId(0);
            return $myservice;
        });
    }
}
```

- MyService4クラス

```php
class MyService4 {
    private $serial;
    private $id = -1;
    private $msg = 'no id...';
    private $data = ['Hello', 'Welocome', 'Bye'];

    public function __construct() {
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

‐ コントローラ

```php
class SampleController extends Controller {

    public function __construct(MyService4 $myservice)
    {
        $myservice=app(MyService4::class);
    }

    public function index(MyService4 $myservice,int $id=-1) {

        $myservice->setId($id);
        $data=[
            'msg'=>$myservice->say($id),
            'data'=>$myservice->alldata()
        ];
        return view('sample.index',$data);
    }
}
```
