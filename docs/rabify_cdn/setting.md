# 設定方法
お申込み手続きが完了いたしましたら、お客様専用の`rabify CDN`のURLが発行されます。お客様もともとのドメインを`example.com`、お客様専用の`rabify CDN`のURLを`rabify.example.com`として、設定方法をご説明いたします。

## 基本的な方法
お客様のHTML、CSSで指定している画像URLのドメインを、`rabify CDN`に差し替えます。

```HTML
<img src="https://example.com/img/hero.jpg" />
```

があったとしたら、以下に差し替えてください。

```HTML
<img src="https://rabify.example.com/img/hero.jpg" />
```

### 画像を縮小する場合
URLの最後に`?d=`の引数をつけてサイズを指定することで、画像を縮小することができます。例えば、以下のURLの場合、横幅200pxに縮小されます。 __なお、beta版では50pxの倍数のみをサポートしています。ご注意ください。__

```HTML
<img src="https://rabify.example.com/img/hero.jpg?d=200" />
```

ブラウザのスクリーン幅によって異なる画像を読み込む`srcset`属性をつかって、複数のサイズの画像を利用する時は以下のようになります。320pxと620pxの場合の縮小画像を用意しています。


```HTML
<img src="https://rabify.example.com/img/hero.jpg"
  srcset="https://rabify.example.com/img/hero.jpg?d=300 300w
           https://rabify.example.com/img/hero.jpg?d=600 600w" />
```

### 画像を切り抜く場合
URLの最後に`?d=`の引数に、縦幅をつけることで画像を切り抜くことができます。例えば、以下のURLの場合、横幅200pxに縮小後、縦幅100pxに切り取られます。

```HTML
<img src="https://rabify.example.com/img/hero.jpg?d=200x100" />
```

なお、切り抜きは、中央を起点にして切り抜かれます。

## WordPress
### Plugin利用
- __WordPressプラグイン公式サイトへの登録作業は申請作業中です__
- [View Rawをクリックしてプラグインをzipでダウンロード](https://github.com/rabify/cdn/blob/master/sample/wordpress/rabify_cdn.zip)

![rabify CDN Plugin画面](../images/plugin.png)

Pluginを有効にすると、`設定`=>`メディア`の下部に以下のような画面がでますので、こちらをご設定ください。

| 設定項目 | 説明 |
|---|---|
| Enable | ドメインの差し替えを有効にします。必ずURLを入力してからチェックするようにしてください |
| rabify CDN URL | 弊社からお送りしたURLを入力します |
| 画像サイズ | 画像サイズのパターンをカンマ区切りで入力します。imgのsrcset属性に反映されます。 |
| img sizes | `img`の`sizes`属性に反映されます。`the_content`は記事本文、`the_expert`は要約記事、`custom-header`はヘッダー画像、`post_thumbnail_html`はサムネイル画像のimgタグに反映されます。なお、画像に`width`と`height`が設定されていない場合はsizesは反映されません。 |
| 強制適用 | rabify CDNをすべてのページに適用します。`ob_start` を利用しているので、他のプラグインと競合する可能性があります。適用後は必ずページをご確認ください。 |


### functions直書き
WordPressは、Themeの`functions.php`に以下を追記することで`<img src>`タグの上書きを行うことができます。以下のソースコードを`functions.php`の最後の行にコピペください。

[cdn/sample/wordpress/functions.php](https://github.com/rabify/cdn/blob/master/sample/wordpress/functions.php)

初期設定では、`the_content()` で表示するコンテンツの画像がすべてrabify CDNに置き換わります。
`header.php`や`footer.php`などのテンプレートに挿入した画像を置き換える時は、

```php
$dir = get_template_directory();
echo rabify_cdn("<img src=${dir}/example.jpg">");
```

のようにお使いください。また、画像にsrcsetの個別サイズを設定したい場合は、第二引数をつかって

```php
$dir = get_template_directory();
echo rabify_cdn("<img src=${dir}/example.jpg">", [100,200,300,400]);
```

のように連想配列でご指定ください。