## 本記事の内容
- 1つのファイルに全て記載されているOpenAPIを段階的(Stage1~5)にリファクタリングしていき、最終的に複数のファイルを参照する形に変えていく

## 本記事で扱わないこと
- 複数のファイルに分割したYAMLファイルを1枚のSwaggerファイルにビルドする方法

## 分割の考え方について
オブジェクトを複数のファイルに考え方、構成の仕方は、要件やチームの文化等の要因によって変わってきます。
今回は私のやり方を例として記載するので、一参考としてお役に立てることができれば幸いです。

もし「この方がやりやすい」「この要件ではこういう構成がベストだった」等ございましたら、ぜひ共有していただければと思います。

## Stage1: 何も分割していない状態
### ディレクトリ構成図
```
test/
 └ openapi.yaml <------ New!
```
### 記述例
`/login`にGETリクエストしたユーザーからemailとpasswordを受け取り、idとnameを返す、簡単なログインAPIを記述します
#### openapi.yaml
```yaml
# openapi.yaml
openapi: "3.0.2"
info:
  description: "Qiita Demo"
  version: 1.0.0
  title: Sample API
  license:
    name: MIT
servers:
  - url: http://api-qiitayounosample.com
paths:
  "/login":
    get:
      tags:
        - test
      summary: 一般ユーザーがログインします
      parameters:
        - in: query
          name: user
          schema:
            type: object
            required:
              - email
              - password
            properties:
              email:
                type: string
                format: email
                example: "test@example.com"
              password:
                type: string
                format: password
                example: "hugahuga"
      responses:
        200:
          description: 成功時の処理
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: integer
                    example: 1
                  name:
                    type: string
                    example: "hogehoge"
```
:::note warn
これではファイルがごちゃごちゃしているため、内容の確認や変更が大変になってしまいますね。例えばpathが増えた時のことを考えてみてください。今は`"/login"`だけですが、`/logout`だったり`/{user}/{articleId}`のようにpathが増えていくと、もう見ていられない(文字通り)状態になってしまいます。

まずはリクエストとレスポンスを見やすくするためにcomponentsを活用しましょう！
:::
## Stage2: Componentsを活用しているが1つのファイルに全て記述している状態
### ディレクトリ構成図(変更なし)
```
test/
 └ openapi.yaml
```
### 記述例
#### openapi.yaml
```yaml
# openapi.yaml
openapi: "3.0.2"
info:
  description: "Qiita Demo"
  version: 1.0.0
  title: Sample API
  license:
    name: MIT
servers:
  - url: http://api-qiitayounosample.com
paths:
  "/login":
    get:
      tags:
        - test
      summary: Qiita用
      parameters:
        - in: query
          name: user
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/parameters"
      responses:
        200:
          description: Qiita用です
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/responses"
components:
  schemas:
    parameters: # 命名するときに、この部分はご自身で改良してください。
      type: object
      required:
        - email
        - password
      properties:
        email:
          type: string
          format: email
          example: "test@example.com"
        password:
          type: string
          format: password
          example: "hugahuga"
    responses: # 命名するときに、この部分はご自身で改良してください。
      type: object
      properties:
        id:
          type: integer
          example: 1
        name:
          type: string
          example: "hogehoge"
```
:::note
parametersとresponsesがスッキリしたことによって、応答関係がわかりやすくなりましたね！
:::
:::note warn
ただcomponentsも当然肥大化していくので、schemaの確認・変更時には手間がかかりそうです。また複数人で作業するときはコンフリクトの問題に頭を悩まされることになってしまうことは容易に予想できますね。

そこで次はcomponentsを外部のファイルに分割しましょう！
:::
## Stage3: componentsを分割する
### ディレクトリ構成図
```
test/
 ├ schemas/ <------ New!
 ┃   ├ requests/ <------ New!
 ┃   ┃    └ Login.yaml <------ New!
 ┃   └ responses/ <------ New!
 ┃        └ Login.yaml <------ New!
 └ openapi.yaml
```
### 記述例
#### openapi.yaml
```yaml
# openapi.yaml
openapi: "3.0.2"
info:
  description: "Qiita Demo"
  version: 1.0.0
  title: Sample API
  license:
    name: MIT
servers:
  - url: http://api-qiitayounosample.com
paths:
  "/login":
    get:
      tags:
        - test
      summary: Qiita用
      parameters:
        - in: query
          name: user
          content:
            application/json:
              schema:
                $ref: "./schemas/requests/Login.yaml"
      responses:
        200:
          description: Qiita用です
          content:
            application/json:
              schema:
                $ref: "./schemas/responses/Login.yaml"
```
$refの部分をファイルパスに変更してあります
#### schemas/requests/Login.yaml
```yaml
# schemas/requests/Login.yaml
type: object
required:
  - email
  - password
properties:
  email:
    type: string
    format: email
    example: "test@example.com"
  password:
    type: string
    format: password
    example: "hugahuga"
```
### schemas/responses/Login.yaml
```yaml
# schemas/responses/Login.yaml
type: object
properties:
  id:
    type: integer
    example: 1
  name:
    type: string
    example: "hogehoge"
```
:::note
それぞれのファイルに分割することで、1つのファイルごとの可読性が大幅に向上しました！
これでschemaに関する確認・変更作業負担は大幅に減少しましたね！
:::
:::note warn
さて、この例ではこのままで十分読みやすいファイル構造に改良することができました。
一方、pathsが増えた場合はどうでしょうか？これまた大変なことになってしまいますね。現状たった1つのpathでもファイルの半分以上をpathが占めていることは一目瞭然です。

そこでpathsも分割しましょう！
:::
## Stage4: pathsを分割する
### ディレクトリ構成図
```
test/
 ├ paths/ <------ New!
 ┃   └ login.yaml <------ New!
 ├ schemas/
 ┃   ├ requests/
 ┃   ┃      └ Login.yaml
 ┃   └ responses/
 ┃          └ Login.yaml
 └ openapi.yaml
```
### 記述例 (schemaに変更はないので省略します)
#### openapi.yaml
```yaml
# openapi.yaml
openapi: "3.0.2"
info:
  description: "Qiita Demo"
  version: 1.0.0
  title: Sample API
  license:
    name: MIT
servers:
  - url: http://api-qiitayounosample.com
paths:
  "/login":
    $ref: "./paths/login.yaml"
```
#### paths/login.yaml
```yaml
# paths/login.yaml
get:
  tags:
    - test
  summary: Qiita用
  parameters:
    - in: query
      name: user
      content:
        application/json:
          schema:
            $ref: "../schemas/requests/Login.yaml"
  responses:
    200:
      description: Qiita用です
      content:
        application/json:
          schema:
            $ref: "../schemas/responses/Login.yaml"
```
:::note
最初と比べて、openapi.yamlがかなり読みやすくなりましたね！
これでpathsやschemasがいくら増えても、可読性を保ったままスケールアップしていくことができます。

最後にPreview上でschemasの一覧が見れるように、openapi.yamlにcomponentsを復活させましょう！
:::
## Stage5: componentsにschemasを全て記載する
### ディレクトリ構成図
```
test/
 ├ paths/
 ┃   └ login.yaml
 ├ schemas/
 ┃   ├ requests/
 ┃   ┃      └ Login.yaml
 ┃   ├ responses/
 ┃   ┃      └ Login.yaml
 ┃   └ Index.yaml <------ New!
 └ openapi.yaml
```
### 記述例
#### openapi.yaml
```yaml
# openapi.yaml
openapi: "3.0.2"
info:
  description: "Qiita Demo"
  version: 1.0.0
  title: Sample API
  license:
    name: MIT
servers:
  - url: http://api-qiitayounosample.com
paths:
  "/login":
    $ref: "./paths/login.yaml"
components:
  schemas:
    $ref: "./schemas/Index.yaml"
```
#### schemas/Index.yaml
```yaml
Requests:
  type: object
  properties:
    Login:
      $ref: "./requests/Login.yaml"
Responses:
  type: object
  properties:
    Login:
      $ref: "./responses/Login.yaml"
```
:::note
完璧です！ここまでお疲れ様でした！
これで実際にOpenAPIの記述作業を担当する方にとっても、これを元に実装する人にとっても双方にわかりやすい構成を作ることができました！
:::
## 検討の余地はいっぱいある
ここまで実際に私がよく使う構成をご紹介させていただきました。
ですが、この構成はまだまだ改善の余地があると思います。

例えば「**ファイル名はpathsとschemasで大文字小文字と分かれているが、それは本当に効果があるのか？**」とか「**schemasにはpaths毎に記述しているけど、もっと細かく分割すればいろんなパーツを使いまわせるのではないか？**」等です。

特に後者に関しては私も決まった回答が出せていませんが、**複数人で作業する場合や作業している方が途中で替わる時のリスクを考えた際に、schemasフォルダの中まで構成が統一されていることは、一定のクオリティを担保することにつながり、それこそYAML形式やOpenAPIの良さを引き出すことができる**ので、上記の構成を基本的に採用しています。

ただ構成に関しては要件や環境によって柔軟に変えていくべき、そのほうがチーム全体の生産力の向上に繋がると考えているので、例えばschemaの記述量がとんでもなく多いAPIを表現するときや、[ツール側の問題](https://github.com/swagger-api/swagger-ui/issues/3803)が)が発生しているときには構成の見直しが必要になります。

## 最後に
補足意見や訂正等ございましたら、ぜひコメントをお願いします。
