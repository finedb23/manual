# これはH1タグです
## これはH2タグです

インライン表示
このように`int i = 0`がインライン表示されます

```javascript
document.querySelectorAll('div.code').forEach(el => {
  // then highlight each
  hljs.highlightElement(el);
});
```

- aa
- bb
- 日本語

_ か * で囲むとHTMLのemタグになります。Qiitaでは *italic type* になります。
__ か ** で囲むとHTMLのstrongタグになります。Qiitaでは **太字** になります。


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


- [x] タスク1
- [x] タスク2
- [ ] 

> 文頭に>を置くことで引用になります。
> 複数行にまたがる場合、改行のたびにこの記号を置く必要があります。
> **引用の上下にはリストと同じく空行がないと正しく表示されません**
> 引用の中に別のMarkdownを使用することも可能です。
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
|:-----------|------------:|:------------:|
| This       | This        | This         |
| column     | column      | column       |
| will       | will        | will         |
| be         | be          | be           |
| left       | right       | center       |
| aligned    | aligned     | aligned      |

今夜は寿司[^1]です。

[^1]: https://jsprimer.net/

今夜は寿司[^2]です。

[^2]: https://jsprimer.net/


