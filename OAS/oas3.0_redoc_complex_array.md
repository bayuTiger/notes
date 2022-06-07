## 環境
- OpenAPI(YAML形式) 3.0.2
- ReDoc 2.0.0

## 発生している問題
### 以下のJSONを表現したい~
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
### Preview上では適切に表現できているYAML
```yaml
# schema以下から記述
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
## 解決方法
### 表現できているJSON(上記JSONと同じ)
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
### YAML
```yaml
# 省略 
content:
  application/json:
    schema:
      # 上記YAMLのexampleだけを削除したもの
    example:
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
:::note
結局JSONで記述するんかい！という感じですが、Swagger Viewerを使っている方なら**このJSONの記述は簡単に行うことができます**。
以下に補遺としてその手順を記載します。
:::
## 補遺:exampleへのJSON記述
1. まずは記述例2のように素直に記述します
1. 次にSwagger Viewerで**ブラウザで**開きます(デフォルトはVSCode内で開く設定になっています。setting->検索窓にswagger->Swagger Viewer: Preview In Browserの項目にチェックを入れましょう)
1. PreviewにはJSON形式で表示されるので、それをコピーします
1. それをexampleに貼りつけ、schemaからexampleの値を削除すれば完了です！
工程にもよりますが、上記の手順で行えば表示を確認・共有しながら作業を進めていくことができるので、私は以上のフローを採用しています。

## 最後に
補足意見や訂正等ございましたらコメントいただけるとありがたいです。
