# これは H1 タグです

## これは H2 タグです

インライン表示
このように`int i = 0`がインライン表示されます

```javascript
document.querySelectorAll("div.code").forEach((el) => {
  // then highlight each
  hljs.highlightElement(el);
});
```

```
class Point {
    constructor(x, y) {
        // `this`の代わりにただのオブジェクトを返せる
        return { x, y };
    }
}

// `new`演算子の結果はコンストラクタ関数が返したただのオブジェクト
const point = new Point(3, 4);
console.log(point); // => { x: 3, y: 4 }
// Pointクラスのインスタンスではない
console.log(point instanceof Point); // => false
```


- aa
- bb
- 日本語

\_ か * で囲むと HTML の em タグになります。Qiita では *italic type\* になります。
\_\_ か ** で囲むと HTML の strong タグになります。Qiita では **太字\*\* になります。

打ち消し線を使うには ~~ で囲みます。 ~~打ち消し~~

リンクの挿入

[Qiita](http://qiita.com "Qiita Home")

画像の挿入

![Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")

<details><summary>サンプルコード</summary>

(上に空行が必要)

```rb
puts 'Hello, World'
```

</details>

- [x] タスク 1
- [x] タスク 2
- [ ]

> 文頭に>を置くことで引用になります。
> 複数行にまたがる場合、改行のたびにこの記号を置く必要があります。
> **引用の上下にはリストと同じく空行がないと正しく表示されません**
> 引用の中に別の Markdown を使用することも可能です。
>
> > これはネストされた引用です。

<dl>
  <dt>リンゴ</dt>
  <dd>赤いフルーツ</dd>
  <dt>オレンジ</dt>
  <dd>橙色のフルーツ</dd>
</dl>

テーブル

| Left align | Right align | Center align |
| :--------- | ----------: | :----------: |
| This       |        This |     This     |
| column     |      column |    column    |
| will       |        will |     will     |
| be         |          be |      be      |
| left       |       right |    center    |
| aligned    |     aligned |   aligned    |

今夜は寿司[^1]です。

[^1]: https://jsprimer.net/

今夜は寿司[^2]です。

[^2]: https://jsprimer.net/
