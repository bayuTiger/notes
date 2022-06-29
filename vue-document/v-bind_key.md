# v-bind:keyの動作について

[v-bind:keyの記述有無で何が変わるのか](https://reffect.co.jp/vue/v-bind-key-understand-by-developer-tool)

## 結論

1. v-bind:keyは特定の状況以外、記述しなければならない
2. keyの値は、その要素を一意に識別できるものでなければならない

## case.1: テーブル削除

- :key 無し
  - 先頭の要素が削除されるのではなく、下の行の情報によって**上書きされる**形で表現される
- :key 有り
  - 要素が削除され、上書きは起こらない

## case.2: input要素を利用

- :key 無し
  - input要素だけは削除も上書きもされないで**そのまま残る**
- :key 有り
  - 削除され、上書きも怒らない

## column: :key='index'の場合の挙動

- keyを設定しない場合と同じ
- indexではその要素を一意に識別することはできないから
