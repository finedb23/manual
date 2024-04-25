# 設定情報と環境変数

## 設定情報とConfigクラス

- configフォルダ内に、「sample.php」というファイルを作成してください。

```php
return [
    'message'=>'This is sample config-data!',
    'data'=>['one','two','three']
];
```

- コントローラ

```php
class SampleController extends Controller {
    /**
     * Display a listing of the resource.
     */
    public function index() {
        // $data = [
        //     'msg' => 'SAMPLE-Controller-index!'
        // ];
        // return view('sample.index', $data);

        $sample_msg=config('sample.message');
        $sample_data=config('sample.data');
        $data=[
            'msg'=>$sample_msg,
            'data'=>$sample_data
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

- ルーター

```php
Route::get('sample',[SampleController::class,'index']);
```

## 設定を更新する

他のクラス内で変更された設定は、そのクラス内でのみ利用可能です。


```php
function __construct() {
    config(['sample.message'=>'新しいメッセージ!']);
}
```

## 環境変数の利用

- 「.env」ファイルを開く

```env
SAMPLE_MESSAGE="This is Environment message!"
SAMPLE_DATA=AAA,BBB,CCC
```

- コントローラ

```php
class SampleController extends Controller {
    public function index() {
        $sample_msg=env('SAMPLE_MESSAGE');
        $sample_data=env('SAMPLE_DATA');
        $data=['msg'=>$sample_msg,"data"=>explode(',',$sample_data)];
        return view('sample.index',$data);
    }
}
```
