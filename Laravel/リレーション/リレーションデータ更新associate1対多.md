# リレーションデータの更新

- 子モデルに親モデルを紐付け(1対多)

> 小モデルのインスタンス->リレーション名()->associate(親モデルのインスタンスor外部キー)

例: blogs(従)テーブルのcategory_id(外部キー)カラムに2を設定

> $blog=Blog::find(1);
> $category2=Category::find(2);
> $blog->category()->associate($category2);

下記も同様

> $blog->category()->associate(2);
> $blog->category_id=2;

## 子モデルから親モデルの紐づけ解除 (1対多)

- 紐づけの解除は小テーブルの外部キーをNULLに設定することを意味します

> 小モデルのインスタンス->リレーション名()->dissociate()

例: blogs(従)テーブルのcategory_id(外部キー)カラムにNULLを設定

> $blog=Blog::find(1);
> $blog->category()->dissociate();

下記も同様

> $blog->category()->associate(null);
> $blog->category_id = null;



