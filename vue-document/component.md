# コンポーネント

## グローバルコンポーネント

- どのインスタンス(new Vue([]))でも利用できる

```js
Vue.component(コンポーネントの名前,{
  template : 'HTMLに表示させたい内容'
})
```

使用例

```js
Vue.component('hello-world',{
  template : '<h1>Hello World</h1>'
})
```

```html
<div id="app">
 <hello-world></hello-world>
</div>
```

## ローカルコンポーネント

- 特定のインスタンスでのみ利用できる
  - そりゃそう。特定のインスタンスの中に記述するから

```js
var app = new Vue({
  el: '#app',
  data: {
  },
  components: {
    'hello-world': {
      template: '<h1>Hello World</h1>'
    }
  }
})
```

## テンプレートに複数のルート要素を入れる

これはエラーになる

```js
Vue.component('hello-world',{
  template: '<p>{{ message }}</p><p>Hello World</p>',
  data: function(){
    return {
      message : 'Hello Vue'
    }
  }
})
```

- templateの中に、pタグ(ルート要素)が複数入っている
  - templateのルート要素は1つでなければならない(v2.*)
  - 3.*ではエラーが起きない

エラーにならないためには、上位の1つのルート要素で囲む必要がある

```js
Vue.component('hello-world',{
  template: '<div><p>{{ message }}</p><p>Hello World</p></div>',
  data: function(){
    return {
      message : 'Hello Vue'
    }
  }
})
```

## dataを使う

```js
Vue.component('hello-world',{
  template: '<h1>{{ message }}</h1>',
  data: function(){
    return {
      message : 'Hello Vue'
    }
  }
})
```

## methodsを使う

```js
Vue.component('button-counter',{
  template: '<p>カウント:{{ count }} <button v-on:click="countUp">Up</button><button v-on:click="countDown">Down</button></p>',
  data: function(){
    return {
      count : 0
    }
  },
  methods: {
    countUp: function(){
      this.count++
    },    
    countDown: function(){
      this.count--
    },
  }
})
```

## template内のhtmlを改行

- バッククオート「`」を使う

```js
Vue.component('button-counter',{
  template: `<p>
  カウント:{{ count }} 
  <button v-on:click="countUp">
  Up
  </button>
  <button v-on:click="countDown">
  Down
  </button>
  </p>`,
```

## データの受け渡し: 親->子

- コンポーネントを利用する側が親
- **v-bind**と**props**を使用する

```js
Vue.component('hello-world',{
  template: '<h1>Hello World and {{ message }}</h1>',
  props: ['message']
})

var app = new Vue({
  el: '#app',
  data: {
    inputText: ''
  }
})
```

```html
<div id="app">
  <input type="text" v-model="inputText">
  <hello-world :message="inputText"></hello-world>
</div>
```

## リスト ->子

```js
Vue.component('blog-post',{
  template: '<div><h2>{{ post.title }}</h2><p>{{ post.content }}</p></div>',
  props: ['post']
})

var app = new Vue({
  el: '#app',
  data: {
    posts: [
      {'id':0, 'title': 'vue.jsの基礎', 'content': 'about vue.js...'},
      {'id':1, 'title': 'componentの基礎', 'content': 'about component...'},
      {'id':2, 'title': 'Vue CLIの基礎', 'content': 'about Vue CLIの基礎...'},
    ]
  }
})
```

```html
<div id="app">
  <blog-post v-for="post in posts" :key="post.id" :post="post"></blog-post>
</div>
```

## データの受け渡し　子->親

- $emitメソッドとイベントを利用する

1. 子コンポーネント側でボタンに対してクリックイベントを設定する
2. クリックイベントが発生したら、子コンポーネントはそのイベントの処理(clickEvent)で$emitメソッドを実行し、親に渡したいイベント(form-child)を発生させる
3. 親は子から来るイベントに対するalertMessageメソッドを設定し、form-childイベントを待つ
4. 親は子が発生させたイベント(form-child)を受け取ってalertMessageメソッドを実行する

```js
// 1
Vue.component('emit-event',{
 template: '<button v-on:click="clickEvent">ボタン</button>',
 // 2
  methods: {
    clickEvent: function(){
      this.$emit('from-child')
    }
  }
})

// 4
var app = new Vue({
  el: '#app',
  methods:{
    alertMessage: function(){
      alert('子からイベント受け取ったよ')
    }
  }
})
```

```html
// 3
<div id="app">
  <emit-event v-on:from-child="alertMessage"></emit-event>
</div>
```

なお子側で`template='<button v-on:click="$emit('from-child')">ボタン</button>',`と記述することで、clickEventメソッドが不要になる

## data ->親

- $emitは第2引数以降に、渡すデータをオブジェクトで持つことができる

```js
Vue.component('emit-event',{
  template: `<div>
  <input type="text" v-model="inputText">
  <button v-on:click="clickEvent">送信ボタン</button>
  </div>`,
  data: function(){
    return {
      inputText : ''
    }
  },
  methods: {
    clickEvent: function(){
      this.$emit('from-child',this.inputText)
    }
  },
})

var app = new Vue({
  el: '#app',
  data: {
    message: ''
  },
  methods:{
    receiveMessage: function(message){
      this.message = message;
    }
  }
})
```

```html
<div id="app">
  <p>{{ message }}</p>
  <emit-event v-on:from-child="receiveMessage"></emit-event>
</div>
```

## コンポーネントとSlot

- 親側でコンポーネントタグの間に挿入した内容を子コンポーネント側で表示することができる

```html
<div id="app">
  // 親側のコンポーネントタグの間に「スロットによる挿入」を挿入している
  <slot-test>スロットによる挿入</slot-test>
</div>
```

```js
Vue.component('slot-test',{
  // 初期値を設定することもできる
  template: '<p><slot>Slotの初期値</slot></p>',
}),
```

## slotに名前をつける

- slotに挿入する内容が複数あるときに利用
  - 子側には、name属性でslotタグに名前をつける
  - 親側では、slot属性を追加し、その値に子コンポーネントで定義されているslotの名前を入力する
- templateを複数回記述して、name(slot)で別々の内容にしていく感じ

```js
Vue.component('slot-test',{
  template: `<div>
  <h1><slot name="title">タイトル</slot></h1>
  <p><slot name="content">内容</slot></p>
  </div>`,
})
```

```html
<div id="app">
  <slot-test>
    <template slot="title">vue.jsの基礎</template>
    <template slot="content">vue.jsについて</template>
  </slot-test>
</div>
```
