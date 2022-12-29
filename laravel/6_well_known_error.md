# Laravel6 よくあるエラ〜

## 注意！
以下の内容は必ずしも「このエラーが出たら、これをすれば解決します！」ということを保証しているわけではありません。
あくまで１つの参考としてお読みください。

## SQLエラー「〇〇カラムはnullを許しません！」

- nullってことはつまり、値が入っていないってこと(前提を確認することは大事)
- まず  `dd($request)` でDB通信の内容を確認する。
- そしたら案の定、input の name属性の指定がタイポしていた、みたいなことがよくある

## 404

Routingを見直す

1. `php artisan route:list` でroutingが正常に設定されているかを確認
1. Apache設定の見直し
    1. 大体Documentroot
    💡 そもそものURL指定は間違ってない？ `localhost/フォルダ名/public/routing` だよ！
        

## バリデーション実装時の403エラー

- Requestのauthorizeメソッドがfalseになってるからだと思うよ
    - false ⇒ trueで解決！

## このRouteはPOSTをサポートしていません

- Routingに記述されてあるものとformのaction指定の記述にズレがあった
