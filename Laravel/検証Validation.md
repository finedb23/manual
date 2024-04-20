# 検証 バリデーション

‐ コマンド

```
php artisan make:rule MyRule
```

```php
namespace App\Rules;

use Closure;
use Illuminate\Contracts\Validation\ValidationRule;

class MyRule implements ValidationRule
{
    private $n;
    public function __construct($n)
    {
        $this->n=$n;
    }
    /**
     * Run the validation rule.
     *
     * @param  \Closure(string): \Illuminate\Translation\PotentiallyTranslatedString  $fail
     */
    public function validate(string $attribute, mixed $value, Closure $fail): void
    {
        if ($value%$this->n!=0) {
            $fail('The:attribute は'.$this->n.'で割り切れる値が必要です')->translate();
        }
    }
}
```

実際に使う

```php
 // post
        $rules=[
            'name'=>'required',
            'mail'=>'email',
            'age'=>['numeric',new MyRule(5)]
        ];
        $messages=[
            'name.required'=>'名前は必ず入力してください',
            'mail.email'=>'メールアドレスが必要です',
            'age.numeric'=>'年齢を整数で記入ください',
           // 'age.between'=>'年齢は0から150で入力してください',
            'age.min'=>'年齢は零歳以上で記入してください',
            'age.max'=>'年齢は200再以下で記入してください',
            'age.hello'=>'Hello!入力は偶数のみ受け付けます。',
        ];
        $validator=Validator::make($request->all(),$rules,$messages);

        // 条件に応じてルールを追加する
        $validator->sometimes('age','min:0',function($input){
            return !is_int($input->age);
        });
        $validator->sometimes('age','max:200',function($input){
            return !is_int($input->age);
        });

        if($validator->fails()){
            return redirect('/hello')->withErrors($validator)->withInput();
        }
        

        // $validate_rule=[];
        // $this->validate($request,$validate_rule);

        return view('hello.post',['msg'=>'正しく入力されました！']);
        ```
