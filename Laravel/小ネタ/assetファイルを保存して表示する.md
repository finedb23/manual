# assetでリンクを作成する

- シンボリック登録

```
sail artisan storage:link
```

- コントローラ


```php
    public function create() {
        return view('photos.create');
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request) {
        $saveFilePath = $request->file('image')->store('photos', 'public');
        Log::debug($saveFilePath);
        
        $fileName = pathinfo($saveFilePath, PATHINFO_BASENAME);
        
        log::debug($fileName);
        return to_route('photos.show', ['photo' => $fileName])->with('success', 'アップロードしました');
    }


    /**
     * Display the specified resource.
     */
    public function show(string $fileName) {
        return view('photos.show', compact('fileName'));
    }
```

- ビュー create.blade.php

```php
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>画像アップロード</h1>

    @if(session()->has('success'))
        <p>{{session()->get('success')}}</p>
    @endif
    <form action="{{ route('photos.store') }}" method="post" enctype="multipart/form-data">
        @csrf
        <input type="file" name="image">
        <input type="submit" value="アップロード">
    </form>
</body>
</html>
```

- ビュー show.blade.php

```php
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>画像アップロード</h1>

    @if(session()->has('success'))
        <p>{{session()->get('success')}}</p>
    @endif

    ポイント
    <img src="{{asset('storage/photos/'.$fileName)}}" alt="">
</body>
</html>
```

- ルータ

```php
Route::resource('photos',PhotoController::class);
```
