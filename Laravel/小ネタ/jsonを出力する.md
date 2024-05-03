# jsonを出力する

```php
public function outJson() {
    return response()
    ->json([
        'name'=>'Yamda',
        'sex'=>'male',
        'age'=>18,
    ]);
}

こちらでも同じJson形式で返す
public function outJson() {
    return [
        'name'=>'Yamda',
        'sex'=>'male',
        'age'=>18,
    ];
}
```

JSONP形式の応答を生成するならば、更新にwithCallbackメソッドを呼び出します

```php
return response()
->json([...])
->withCallback('callback')
```
