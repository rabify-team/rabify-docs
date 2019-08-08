# キャッシュの削除
rabify APIを利用することで、CDN上のキャッシュを削除することができます。
rabify CDNにお申し込み頂いた際にお送りするAPIキーを利用してください。

## リクエストのサンプル

### ファイル名を指定して削除
リクエストのbodyに画像の相対パスを指定することで、任意の画像ファイルのキャッシュを削除できます。

```bash
$ curl https://api.cdn.rabify.me/v1/cdn/cache \
   -XDELETE \
   -H "x-api-key: XXXXX" \
   -d '{"path": ["/img/example.png", "/img/example1.png"]}' 
```

|file name|削除される|削除されない|
|:--|:--|:--|
|/img/example.png|◯||
|/img/example1.png|◯||
|/img/photo/john.jpg| |◯|
|/img/photo/jane.jpg| |◯|
|/img/food/ramen.png| |◯|
|/img/food/sushi.gif| |◯|

### ディレクトリを指定して削除
ディレクトリのみ指定することで、指定したディレクトリ配下に保存された画像のキャッシュをまとめて削除できます。

```bash
$ curl https://api.cdn.rabify.me/v1/cdn/cache \
   -XDELETE \
   -H "x-api-key: XXXXX" \
   -d '{"path": ["/img/photo/*", "/img/example.png"]}' 
```

|file name|削除される|削除されない|
|:--|:--|:--|
|/img/example.png|◯||
|/img/example1.png| |◯|
|/img/photo/john.jpg| |◯|
|/img/photo/jane.jpg| |◯|
|/img/food/ramen.png|◯||
|/img/food/sushi.gif|◯||

### すべて削除
リクエストのbodyを送らない場合、全てのキャッシュを削除します。


```bash
$ curl https://api.cdn.rabify.me/v1/cdn/cache \
   -XDELETE \
   -H "x-api-key: XXXXX" 
```

|file name|削除される|削除されない|
|:--|:--|:--|
|/img/example.png|◯||
|/img/example1.png|◯||
|/img/photo/john.jpg|◯||
|/img/photo/jane.jpg|◯||
|/img/food/ramen.png|◯||
|/img/food/sushi.gif|◯||

## 月間の削除操作回数制限
ご契約頂いているプランに応じて回数の上限が設定されております。
どのプランでも最大の削除操作回数は25回とさせて頂いております。