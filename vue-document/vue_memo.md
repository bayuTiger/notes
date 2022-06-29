# vue チュートリアル

- 宣言レンダリング
- バインディング
- 条件分岐
- ループ
- ユーザーの入力制御
- 双方向バインディング
- コンポーネントによる構成

## 宣言的レンダリング

- データとDOMは関連付けられている
- それら全てが**リアクティブ**

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app', // document.getElementById('app'), $('app')[0] も可
  data: {
    message: 'Hello Vue!'
  }
})
```

## バインディング

- 描画されたDOMに特定のリアクティブな振る舞いを与える、ディレクティブの一種である「v-bind:属性」= 「:属性」
- mustacheはHTML属性の内部でしようすることができないため
- 要素の属性を束縛することができる
- 今回の例では「span」要素の「title」属性を束縛

```html
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
```

## 条件分岐

- テキストをデータに束縛できるだけではなく、**DOM構造にデータを束縛できる**
- 要素がvueによって挿入/更新/削除されたとき、自動的にトランジションエフェクトを適用できるトランジションシステムも提供されている

```html
<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>
```

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

## ループ

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

## ユーザーの入力制御

- *methodsの中では*DOM操作を行っておらず、**アプリケーションの状態のみを更新している**
- つまり、全てのDOM操作をVueに任せられるので、背後のロジックを書くことに集中することができる

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

## 双方向バインディング

- v-modelにはプロパティを代入すれば良さそう

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

## コンポーネントによる構成

- Vueにおける「コンポーネント」は本質的に**あらかじめ定義されたオプションを持つVueインスタンス**

>コンポーネントを登録する
>これで他のコンポーネントのテンプレートから、このコンポーネントを利用できるようになる

```js
// コンポーネント(todo-item)を定義
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue( ... )
```

```html
<ol>
  <!-- todos 配列にある各 todo に対して todo-item コンポーネントのインスタンスを作成する -->
  <todo-item></todo-item>
</ol>
```

>ただこれでは全てのtodoで同じテキストが描画されるだけで、おもしろくない
>ここで親のスコープから子コンポーネントへとデータを渡せるようにする

- ここでプロパティ(props)が必要になる
- プロパティは**コンポーネントに登録できるカスタム属性**
- 値がプロパティに渡されると、そのコンポーネントインスタンスのプロパティになる

```js
Vue.component('todo-item', {
  // todo-item コンポーネントはカスタム属性のような "プロパティ" で受け取る
  // 今回のプロパティは todo と呼ばれる = todoはただの引数
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

```html
<div id="app-7">
  <ol>
    <!--
      各 todo-item の内容を表す todo オブジェクトを与えます。
      これにより内容は動的に変化します。
      また後述する "key" を各コンポーネントに提供する必要があります。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```

```js
// 今回はグローバルコンポーネントを作成する
Vue.component('todo-item', {
  // html上で、groceryList[0],[1],[2]それぞれの要素が格納される(propsも配列で定義してるので)
  props: ['todo'],
  // プロパティが受け取った値のtextをkeyとする要素を展開
  template: '<li>{{ todo.text }}</li>'
  // idも一緒に展開すればいいじゃん!となるが、templateは後勝ちなので
  // 例えば以下のようにすると「1.0 2.1 3.2」となってしまう
  // template: '<li>{{ properties.text }}</li>',
  // template: '<li>{{ properties.id }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
