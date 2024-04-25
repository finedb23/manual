# 明示的にインスタンスを生成する

- 指定したクラスのインスタンスを取得する

`$変数=app(クラス名);`

```php
class SampleController extends Controller {

    public function index() {

        $myService=app(MyService::class);
        $data=[
            'msg'=>$myService->say(),
            'data'=>$myService->data()
        ];

        return view('sample.index',$data);
    }
}
```

- 指定したクラスのインスタンスを取得する

`$変数=app()->make(クラス名);`

- 指定したクラスのインスタンスを取得する

`$変数=resolve(クラス名);`



