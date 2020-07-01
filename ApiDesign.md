# API設計

APIの設計やOpenAPIの情報を残す。

## OpenAPI3

### DataのType

`integer` や `number` などの仕様の相違点は下記を見ればわかる。

[Data Types](https://swagger.io/docs/specification/data-models/data-types/)

#### type: array

`items:` を忘れないこと。

```yml
  ids:
    type: array
    items:
      type: integer
    example:
      - 1
      - 2
      - 3
```

#### nullを表現するには

厳格にnullを表現することはできないが、 `nullable: true` を使用するべ、nullableであることはわかる。

```yml
# Correct
type: integer
nullable: true
```

> OpenAPI 3.0 does not have an explicit null type as in JSON Schema, but you can use nullable: true to specify that the value may be null. Note that null is different from an empty string "".

[Data Types](https://swagger.io/docs/specification/data-models/data-types/)

### パスにパラメーターを含めるとき

```
paths:
  /ping:
    ...
  /users:
    ...
  /users/{id}:
    ...
```

[Paths and Operations](https://swagger.io/docs/specification/paths-and-operations/)

## PUTメソッドについて

返信するステータスコードは成功時は200、更新処理に失敗した時は４２２を返す。レスポンスについては、使わないのであれば特に返す必要はない。公開APIは幅広く使ってもらえるために更新リソースを返すのが一般的である。422はサーバーへの要求コンテンツの型は正しいものの、中に含まれている指示が処理できないことを表す。

[422 Unprocessable Entity - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Status/422)

### Railsにおける最小構成

```rb
def update
  status :ok  # okは200を返す
end
```

### PATCH

PUTと一緒のステータスコードを返す。PUTとの使い分けは以下の通り

> A new method is necessary to improve interoperability and prevent
errors. The PUT method is already defined to overwrite a resource
with a complete new body, and cannot be reused to do partial changes.
Otherwise, proxies and caches, and even clients and servers, may get
confused as to the result of the operation.

訳すとPATCHメソッドは、相互運用性とエラーを防ぐために必要だ。PUTはリソースを新しいボディで上書きするためにすでに定義されている。そしてそれは、複数に再利用することができない。にも関わらず、プロキシートとキャッシュ、そして毎クライアントとサーバーはオペレーションの結果に関わらず複雑になるだろう。と書いてある。つまりリソースの一部の更新なのに、丸々入れかえる意味のPUTメソッドを使用するのはどうなのかという声があるっぽかった。

[www.rfc-editor.org/rfc/rfc5789.txt](http://www.rfc-editor.org/rfc/rfc5789.txt)

## DELTEメソッドについて

返却するステータスコードは204、No Contentである。つまり何も返さない。

[204 No Content - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Status/204)

OpenAPIのリポジトリでも、DELTEメソッドはリクエストボディを許可してないとある。

[Allow requestBody for the DELETE method. · Issue #1801 · OAI/OpenAPI-Specification](https://github.com/OAI/OpenAPI-Specification/issues/1801)

### Railsにおける最小構成

```rb
def destroy
  head :no_content
end
```

### 一括削除について

リソースを一括で削除するときは、DELETEメソッドを使用しない方が良い。理由はREST FulなAPIはDELETEメソッドを使用するときは単一のリソースの削除という決まりがある。加えて削除対象の `ids` をまとめて受け取るすべがないというのもある。この辺はRFC7231に定義されている。

>    A payload within a DELETE request message has no defined semantics;
   sending a payload body on a DELETE request might cause some existing
   implementations to reject the request.
  

[Swagger openApi Spec 3.0 - DELETE opeartion - Stack Overflow](https://stackoverflow.com/questions/54939681/swagger-openapi-spec-3-0-delete-opeartion)

[RFC 7231 - Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://tools.ietf.org/html/rfc7231#section-4.3)

DELETEメソッドは、リクエストボディは許可されていないので、代替策としては、いくつかあるが僕の目に留まったものだけをあげる。

- MessageBodyに入れ込む
- カスタムHTTPヘッダーに入れる
- クエリパラメータに入れる
- POSTメソッドを使用する

[http - RESTful Alternatives to DELETE Request Body - Stack Overflow](https://stackoverflow.com/questions/14323716/restful-alternatives-to-delete-request-body)

POSTメソッドにして解決する場合は、エンドポイントを `batch_delete` `batchDelete` `delete_batch` などにすると良い。

[Users.messages: batchDelete  |  Gmail API  |  Google Developers](https://developers.google.com/gmail/api/v1/reference/users/messages/batchDelete)

[HTTP - Developers - Dropbox](https://www.dropbox.com/developers/documentation/http/documentation#files-delete_batch)

## Railsでのhttpステータスコードのシンボル一覧

https://github.com/ryuchan00/docs/blob/master/Rails.md#rails%E3%81%A7%E3%81%AEhttp-status-code%E3%81%AE%E4%B8%80%E8%A6%A7

## 参考になる資料

[API 設計ガイド  |  Cloud API  |  Google Cloud](https://cloud.google.com/apis/design?hl=ja)

Googleが指し示したAPIの良い設計とは何かが書いてある。

[Amazon.co.jp： Webを支える技術 ―― HTTP，URI，HTML，そしてREST WEB+DB PRESS plus eBook: 山本 陽平: Kindleストア](https://www.amazon.co.jp/Web%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E6%8A%80%E8%A1%93-HTTP%EF%BC%8CURI%EF%BC%8CHTML%EF%BC%8C%E3%81%9D%E3%81%97%E3%81%A6REST-WEB-PRESS-plus-ebook/dp/B07JK7FZH2/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=)

HTTPとはなんぞやが書いてある良い本、API設計時に手元に置いておきたい。
