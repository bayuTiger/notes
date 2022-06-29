# Vue.js 2 チュートリアル

## style

- オブジェクトを利用して適用する場合は、キャメルケースで記述する必要がある
- またcssの区切り文字である「;」*ではなく*「,」で区切る

```html
<h1 id="app">
<a v-bind:style="styleObject">検索エンジン</a>
</h1>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    styleObject: {
      fontSize: '12px',
      color: 'red'
    }
  }
})
```

## class

```html
<div id="app">
 <h1 v-bind:class="classObj">class属性へのバインド</h1>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    classObj:{
      active : true,
      error: true
    }
  }
})
```

三項演算子を使って、変数の値によって適用するclassを分けることもできる

```html
<div class="h-screen" :class="[show ? 'w-52' : 'w-20']">
```

## v-if, v-show

- v-if: v-elseによって別の表示を記述することが可能
  - *条件に合わない場合* -> 要素は作成されない
- v-show: その要素を表示するかしないかのみ
  - **要素自体は作成されている**
  - displayプロパティがnoneになっているだけ

```html
<p v-if="stock_number > 0">販売中です。</p>
<p v-else>在庫切れのため販売していません。</p>
```

```html
<p v-show="stock_number > 0">販売中です。</p>
```

複数の要素の表示/非表示を切り替えたい場合は、templateで囲む

```html
<template v-if="stock_number > 0">
 <p>販売中です。</p>
 <p>まだ在庫は{{ stock_number }}あります。</p>
</template>
```

## 配列

```html
 <div id="app">
  <ul>
   <li v-for="friend in friends">{{ friend }}</li>
  </ul>
 </div>
 <script>
var app = new Vue({
  el: '#app',
  data: {
   friends: ['Ken','Mike','John','Lisa']
  }
})
</script> 
```

## 配列+オブジェクト

indexで配列のキーを取り出すことができる

```html
<ul>
<li v-for="(user,index) in users">{{ index }} {{ user.name }} : {{ user.age }}</li>
</ul>
```

```js
var app = new Vue({
  el: '#app',
  data: {
     users: [
      {id: 1, name:'Ken', age: 20},
      {id: 2, name:'Mike', age: 23},
      {id: 3, name:'John', age: 21},
      {id: 4, name:'Lisa', age: 20},
    ]
  }
})
```

## obj in obj

- v-forの第2引数ではプロパティ名(オブジェクトのキー)を取り出すことができる

```html
<ul>
<li v-for="(user,name,index) in users">{{ name }} {{ user.email }}({{ user.age }})</li>
</ul>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    users: {
      'Ken': {
        age: 20,
        email: 'ken@gmail.com'
      },
      'Mike': {
        age:23,
        email: 'mike@yahoo.com'
      },      
      'John': {
        age:21,
        email: 'john.doe@example.com'
      },      
      'Lisa': {
        age:20,
        email: 'lisa@google.com'
      }
    }
  }
})
```

## 算出プロパティ

dataを利用して別の処理を行う

```html
<div id="app">
  <p>10文字以上の文字列を入力してください。</p>
  <input type="text" v-model="inputCharacters">
  <p v-if="inputNumber < 10">必要な文字数は{{ needNumbers }}文字です</p>
  <p v-else>10文字以上入力されています</p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
   inputCharacters: '' 
  },
  computed:{
   inputNumber : function (){
    return this.inputCharactors.length;
   },
   needNumbers : function(){
    return 10 - this.inputNumber;
   }
  }
})
```

## データの監視

```js

var app = new Vue({
  el: '#app',
  data: {
    count : 0
  },
    watch: {
      count: function(value){
        if(value > 100){
          console.log('入力値が100を超えました')
        }
      }
    }
})
```
