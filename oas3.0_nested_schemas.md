## 本記事の構成
[表現したいJSON]
[具体的なYAML形式の記述]

## 本記事で扱わないこと
- OpenAPIやSwaggerの歴史
- 基本的な構文やオブジェクトの解説

## 環境
- OpenAPI 3.0.2(YAML形式)
- Visual Studio Code
- Swagger Viewer

## 記述例1 type:object 
### 第2階層まで
#### JSON
```json
{
  "user": {
    "id": 1,
    "name": "hoge"
  },
  "token": "10|j1uEhWklM8IQlWcxFOeqag78gsHTQOAA0v3mK"
}
```
#### YAML
```yaml
type: object
properties:
  user:
    type: object
    properties:
      id:
        type: integer
        example: 1
      name:
        type: string
        example: "hoge"
  token:
    type: string
    example: "10|j1uEhWklM8IQlWcxFOeqag78gsHTQOAA0v3mK"
```
### 第3階層まで
#### JSON
```json
{
  "user": {
    "id": 1,
    "username": "hoge",
    "pets": {
      "dog": "ぽち"
      "cat": "たま"
    }
  },
  "token": "10|j1uEhWklM8IQlWcxFOeqag78gsHTQOAA0v3mK"
}
```
#### YAML
```yaml
type: object
properties:
  user:
    type: object
    properties:
      id:
        type: integer
        example: 1
      username:
        type: string
        example: "hoge"
      pets:
        type: object
        properties:
          dog:
            type: string
            example: "ぽち"
          cat:
            type: string
            example: "たま"
  token:
    type: string
    example: "10|j1uEhWklM8IQlWcxFOeqag78gsHTQOAA0v3mK"
```
さらに階層が深くなっていっても上記例より、`type:object`を重ねていけば良いことがわかりますね
## 記述例2 type:array
### Array of Object
#### JSON
```json
{
  "languages": [
    {
      "id": 1,
      "name": "PHP"
    },
    {
      "id": 2,
      "name": "Javascript"
    }
  ],
  "countries": [
    {
      "id": 1,
      "name": "JP"
    },
    {
      "id": 2,
      "name": "US"
    }
  ]
}
```
#### YAML
```yaml
type: object
properties:
  languages:
    type: array
    items:
      oneOf:
        - type: object
          properties:
            id:
              type: integer
              example: 1
            name:
              type: string
              example: "PHP"
        - type: object
          properties:
            id:
              type: integer
              example: 2
            name:
              type: string
              example: "Javascript"
  countries:
    type: array
    items:
      oneOf:
        - type: object
          properties:
            id:
              type: integer
              example: 1
            name:
              type: string
              example: "JP"
        - type: object
          properties:
            id:
              type: integer
              example: 2
            name:
              type: string
              example: "US"
```
:::note alert
2022/4/15現在この記法では、Swagger Viewerでは正常に表示されますが、**ReDocを用いたHTMLではうまく表示されません**(Response Samplesに配列番号0のものしか表示されません)。
この問題を回避する方法は**schemaと同じ階層にexampleを記述する**ことです。
具体的な記述は[こちらの記事](https://qiita.com/akitika/items/4213398cf5aab8ed6ab5)を参考にしてください。
:::

## 最後に
補足意見や訂正等ございましたらコメントいただけるとありがたいです。
