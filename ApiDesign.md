# API設計

OpenAPIとかの情報を残す。

## OpenAPI3

### DataのTypes

`integer` や `number` などの仕様の相違点は下記を見ればわかる。

[Data Types](https://swagger.io/docs/specification/data-models/data-types/)

### DELTEメソッドについて

DELETEメソッドは、リクエストボディは許可されていない。代替策としては、いくつかあるが僕の目に留まったものだけをあげる。

- MessageBodyに入れ込む
- カスタムHTTPヘッダーに入れる
- クエリパラメータに入れる
- POSTメソッドを使用する

[http - RESTful Alternatives to DELETE Request Body - Stack Overflow](https://stackoverflow.com/questions/14323716/restful-alternatives-to-delete-request-body)

OpenAPIのリポジトリでも、DELTEメソッドはリクエストボディを許可してないとある。

[Allow requestBody for the DELETE method. · Issue #1801 · OAI/OpenAPI-Specification](https://github.com/OAI/OpenAPI-Specification/issues/1801)

## 参考になる資料

[API 設計ガイド  |  Cloud API  |  Google Cloud](https://cloud.google.com/apis/design?hl=ja)

Googleが指し示したAPIの良い設計とは何かが書いてある。

[Amazon.co.jp： Webを支える技術 ―― HTTP，URI，HTML，そしてREST WEB+DB PRESS plus eBook: 山本 陽平: Kindleストア](https://www.amazon.co.jp/Web%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E6%8A%80%E8%A1%93-HTTP%EF%BC%8CURI%EF%BC%8CHTML%EF%BC%8C%E3%81%9D%E3%81%97%E3%81%A6REST-WEB-PRESS-plus-ebook/dp/B07JK7FZH2/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=)

HTTPとはなんぞやが書いてある良い本、API設計時に手元に置いておきたい。
