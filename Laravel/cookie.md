# クッキー

- viewメソッドに付与する

```Php
class BookController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        $data=[
            'records'=>Book::all(),
        ];
        return response()->view('book.index',$data)->cookie('app_title','laravel!',60*24*30);
    }
}
```

## cookieの引数

1. 名前
2. 値
3. 有効期限(分)
4. パス
5. ドメイン
6. HTTPSでのみ送信するか (true/false)

- クッキーの削除

```php
return response()
->view('state.view')
->withoutCookie('app_title')
```

または、

```php
Cookie::expire('app_title')
```

- 値の取得

```php
public function other(Request $request) {
        return view('book.other',['app_title'=>$request->cookie('app_title')]);
}
```


