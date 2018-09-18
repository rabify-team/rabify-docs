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
URLの最後に`?d=`の引数をつけてサイズを指定することで、画像を縮小することができます。例えば、以下のURLの場合、横幅200pxに縮小されます。

```HTML
<img src="https://rabify.example.com/img/hero.jpg?d=200" />
```

ブラウザのスクリーン幅によって異なる画像を読み込む`srcset`属性をつかって、複数のサイズの画像を利用する時は以下のようになります。320pxと620pxの場合の縮小画像を用意しています。


```HTML
<img src="https://rabify.example.com/img/hero.jpg"
  srcset="https://rabify.example.com/img/hero.jpg?d=320 320w
           https://rabify.example.com/img/hero.jpg?d=620 620w" />
```

## WordPress
WordPressは、Themeの`functions.php`によって`<img src>`タグの上書きを行うことができます。以下のソースコードを`functions.php`の最後の行にコピペください。

```PHP
function rabify_cdn_replace_filter( $the_content ) {
	$cdn_url = 'https://rabify.example.com';
	$site_url = preg_replace('/(http|https):\/\//', '$1:\/\/', site_url());
	$site_url = preg_replace('/\./', '\.', $site_url);
	$pattern = "/${site_url}(.*)(\.jpg|\.jpeg|\.png|\.bmp)/";

	$the_content = preg_replace($pattern, $cdn_url."$1$2", $the_content);
  return $the_content;
}
add_filter('the_content', 'rabify_cdn_replace_filter');
```

## 言語別関数
### PHP

### JavaScript
