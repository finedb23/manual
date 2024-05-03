# リレーションデータの更新

多対多の場合は、中間テーブル用のモデルを作成し、従来通りの方法で手動で外部キー設定をしてもよいですが、
belongsToManyによってリレーション設定をしている場合は、中間テーブル用のモデルを作らずシンプルに更新可能です

- 多対多のモデルを紐づけ

> モデルのインスタンス->リレーション名()->attach(外部キーorその配列orそのコレクション) 

例: blog_cat(中間)テーブルのblog_idには1、cat_idカラムに2のレコードが追加

> $blog=Blog::find(1);
> $blog->cats()->attach(2);
> // 配列で「複数指定も可」
> $blog->cats()->attach([2,4]);

attachメソッドは第2引数等によって、中間テーブルの外部キー以外のカラムも同時に更新することも可能

デフォルトでは中間テーブルのcreated_atとupdated_atは自動更新されない

自動更新したい場合はリレーション設定時にblogsToManyの後に「withTimestampsメソッドを使用」する

## 多対多のモデルの紐づけ解除

紐づけの解除は中間テーブルの該当のレコードを削除することを意味します

> モデルのインスタンス->リレーション名()->dettach(外部キーorその配列or相手モデルorそのコレクション)

例: blog_cat(中間)テーブルからblog_idが1、cat_idカラムに2のレコードが削除される

> $blog=Blog::find(1);
> $blog->cats()->dettach(2);
> 
> // 配列で「複数指定も可」
> // $blog->cats()->dettach([2,4]);
>
> // 引数を省略で「紐づけをすべて解除も可」
> // $blog->cats()->dettach();

# 多対多のモデルの紐づけと解除(同期)

指定したものだけを紐づけるように更新します
指定していないデータが既に紐づいていた場合、紐づけは解除(中間テーブルから削除)されます

> モデルのインスタンス->リレーション名()->sync([相手モデルの外部キー1,相手モデルの外部キー2])

例

> $blog=Blog::find(1);
> $blog->cat()->sync([1,2]);

# 多対多のモデルの紐づけと解除(切り替え)

指定したものがすでに紐づいていればひも付き解除、そうでなければ紐づけを行うことも簡単にできます

> モデルのインスタンス->リレーション名()->toggle([相手モデルの外部キー1,相手モデルの外部キー2])

例

> $blog=Blog::find(1);
> $blog->cat()->toggle([1,2]);

# Laravel側のコード

## ビュー

```php
<select id="js-pulldown" class="mr-6 w-full" name="cats[]" multiple>
    <option value="">選択してください</option>
    @foreach($cats as $cat)
    <option value="{{$cat->id}}" @if(in_array($cat->id ,old('cats',$blog->cats->pluck('id')->all()))) selected @endif>{{$cat->name}}</option>
    @endforeach
</select>
```

## 検証 Requestクラス

```php
class UpdateBlogRequest extends FormRequest {
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize(): bool {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array<mixed>|string>
     */
    public function rules(): array {
        // テーブルに存在するか？
        return [
            'category_id' => ['required', 'exists:categories,id'],
            'title' => ['required', 'max:255'],
            'image' => [
                'nullable', // 省略可
                'file', // ファイルがアップロードされている
                'image', // 画像ファイルである
                'max:2000', // ファイル容量が2000kb以下である
                'mimes:jpeg,jpg,png', // 形式はjpegかpng
                // 'dimensions:min_width=300,min_height=300,max_width=1200,max_height=1200', // 画像の解像度が300px * 300px ~ 1200px * 1200px
            ],
            'body' => ['required', 'max:20000'],
            'cats.*'=>['distinct','exists:cats,id']
        ];
    }
}
```

- コントロール

```php
public function edit(Blog $blog)
{
    // 1対多
    $categories=Category::all();
    // 多対多
    $cats=Cat::all();
    // 引数にモデル名をつけると不要になる
    //$blog=Blog::findOrFail($id);
    //dd($blog);
    return view('admin.blogs.edit',compact('blog','categories','cats'));
}
public function update(UpdateBlogRequest $request, string $id)
{
    $blog=Blog::findOrFail($id);
    $updateData=$request->validated();

    if($request->has('image')){
        Storage::disk('public')->delete($blog->image);
        $updateData['image']=$request->file('image')->store('blogs','public');
    }
    // dd($updateData);
    $blog->category()->associate($updateData['category_id']);
    $blog->update($updateData);
    // 多対対の更新
    $blog->cats()->sync($updateData['cats']??[]);// NULL合体演算子がないとエラーになる
    
    return to_route('admin.blogs.index')->with('success','ブログを更新しました');
}
```

- モデル Blog

```php
class Blog extends Model
{
    use HasFactory;
    
    protected $fillable=['title','body','image'];

    public function category() {
        return $this->belongsTo(Category::class);
    }
    // 多対多
    public function cats(){
        return $this->belongsToMany(Cat::class)->withTimestamps();
    }
}
```

- モデル Cat

```php
class Cat extends Model
{
    use HasFactory;

    public function blogs(){
        return $this->belongsToMany(Blog::class);
    }
}
```