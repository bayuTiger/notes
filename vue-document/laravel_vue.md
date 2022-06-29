# Laravel + Vue

## 6/25 勉強会で聞いたこと

### ブラウザ上で削除できない問題に際して

- 破壊的メソッドと非破壊的メソッド
  - 概要
    - 破壊的メソッド
      - 対象となる元の配列の値を変えてしまう
      - 例
        - sort()
        - push()
        - shift()
    - 非破壊的メソッド
      - 対象となる元の配列を変えない
      - その配列のコピーを作成して返す
        - 例
          - slice()
          - filter()
          - map()
          - reduce()

破壊的メソッドに注意しなければならないサンプル

```js
const numbers = (arr) => {
  const downToUp = arr.sort((a,b) => (a < b ? -1 : 1 ));
  const upToDown = arr.sort((a,b) => (a < b ? 1 : -1));
  return [...downToUp, ...upToDown]
} 

console.log(numbers([1,4,3,9]))　// 実際の値は[9,4,3,1,9,4,3,1] <- downToUpにはarrが入っているので、それがupToDownを実行したときに、upToDownの操作によって変化させられてしまう
```

修正後

```js
const numbers = (arr) => {
  const copy = [...arr]
  const downToUp = arr.sort((a,b) => (a < b ? -1 : 1 ));
  const upToDown = copy.sort((a,b) => (a < b ? 1 : -1));
  return [...downToUp, ...upToDown]
} 

console.log(numbers([1,4,3,9]))　//[1, 3, 4, 9, 9, 4, 3, 1]
```

### laravelで取得した値をjsで配列として受け取る

- laravelでDBから受け取った値の型は、どのように値を取得するか(Eloquentのall()等)で決まる
  - もしall()で受け取ったらcollection型になる
- laravelからvueに値を渡すときにjson()を使う
  - このjson()でcollection型から配列に変換してくれている

### 参照渡し 値渡し

