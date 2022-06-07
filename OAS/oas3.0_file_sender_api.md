## 本記事で扱うこと
- ファイル送信API(POST)の記述方法(YAML形式)および関連するオブジェクトの説明

## 本記事の構成
1. ファイル送信APIの記述例
1. 記述例を参照しながら、記述の解説
1. openapi.yaml全体の記述例

## 1. ファイル送信APIの記述例
```yaml
# ~~省略
requestBody:
  content:
    multipart/form-data:
      schema:
        type: object
        properties:
          filename:
            type: string
          file:
            type: string
            format: binary
# 省略~~
```

## 2. 記述例を参照しながら、記述の解説
ファイル送信APIにとって、肝となる部分は`content`と`type`です。

まずPOST通信なので、parametersではなく`requestBody`を使用します。

次にcontentについて、通常であれば`application/json`を定義することが多いと思いますが、ファイル送信APIでは`multipart/form-data`を定義します。
このことにより、Swagger Viewerで確認するときにも、動作の確認を行いやすくなります。

最後にfileプロパティのtypeとformatがstringとbinaryになっています。
これはSwagger3.0に(2.0もそうなのですが)fileタイプが用意されていないため、このような記述になっています。
このことは、公式ドキュメントの[こちら](https://swagger.io/docs/specification/data-models/data-types/#file)に記載されています



## 3. openapi.yaml全体の記述例
```yaml
openapi: "3.0.2"
info:
  version: 1.0.0
  title: Qiita Demo File Upload API
  license:
    name: MIT
servers:
  - url: http://api-qiitayou.com/
paths:
  "/fileUpload":
    post:
      tags:
        - "File Upload API"
      summary: ファイルのアップロードを行います
      requestBody:
        content:
          multipart/form-data:
            schema:
              type: object
              properties:
                filename:
                  type: string
                file:
                  type: string
                  format: binary
      responses:
        200:
          description: レスポンスは省略します
```

