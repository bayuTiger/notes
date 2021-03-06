# DBの基礎的な事柄3(正規化)

以下の記事の続きです

[DBの基礎的な事柄2(演算・トランザクション・ロック)](https://qiita.com/akitika/items/3e1dbfb8d14fcb468d2d)

## テーブルの正規化

### 概要

- 1Fact in 1Placeの実現
- 「正規系」「非正規系」
- メリットは、複数のテーブル間のデータ矛盾を未然に防ぐことができる
- デメリットは、テーブル数が増え問い合わせ速度の低下につながる

### 第一正規化

- テーブルの中に同じ内容を表すカラムの繰り返しが存在しないようにすること
- 繰り返し部分を別のテーブルに分離する

### 第二正規化

- テーブルの主キー以外のカラムが、全て主キーに関数従属している状態にすること
  - 関数従属とは、ある値が決まるとそれに対して一意に値が決められる状態のこと
- 主キー全体ではなく、主キーの一部に関数従属している(部分関数従属)カラム群を別のテーブルに分離する

### 第三正規化

- テーブル内のカラムに推移的関数従属性がない状態にすること
  - 推移的関数従属性とは、ABCの3つのカラムにおいて、Aの値が決まるとBが一意に、Bの値が決まるとCが一意に決まるような状態のこと
- 主キー以外に対して推移的関数従属しているカラム群を別のテーブルに分離する
- 着眼点や手法は第二正規化とほぼ同様だが、第三正規化では主キー以外のカラム間の関数従属を抜きだす
