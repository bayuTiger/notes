# axios

- axiosはPromiseベースのHTTP Clientライブラリ
- GET,POSTのHTTPリクエストを使って、サーバからデータの取得、データ送信を行うことができる
- jsonplaceholderを利用するので、実際のプロダクトにそのまま流用してもうまくいくとは限らない
  - あくまで雛形としての理解に留める

## GET

雛形

```js
axios.get('/user?ID=12345')
  .then(function (response) {
 // handle success(axiosの処理が成功した場合に処理させたいことを記述)
    console.log(response);
  })
  .catch(function (error) {
 // handle error(axiosの処理にエラーが発生した場合に処理させたいことを記述)
    console.log(error);
  })
  .finally(function () {
 // always executed(axiosの処理結果によらずいつも実行させたい処理を記述)
  });
```

- 取得サンプル

```js
mounted(){
  axios.get('https://jsonplaceholder.typicode.com/users')
       .then(response => console.log(response))
       .catch(error => console.log(error))
}
```

## dataに保存する

```js
    data(){
        return {
            users: [],
        }
    },
    mounted(){
        axios.get('https://jsonplaceholder.typicode.com/users')
            .then(response => this.users = response.data)
            .catch(error => console.log(error))
    }
```

## GETメソッドにフィルターをかける

- パラメーターオプションを付ける

```js
axios.get('https://jsonplaceholder.typicode.com/users',
        {
          params: {
            name: 'Glenna Reichert'
          }
        })
      .then(response => this.users = response.data)
      .catch(error => console.log(error))
```

## POST

```js
  data: {
    users: [],
    name: ''
  },
  methods : {
    createNewUser: function(){
     axios.post('https://jsonplaceholder.typicode.com/users',{
            name: this.name
           })
           // unshiftで、戻ってきたデータを先頭に移動させている
          .then(response => this.users.unshift(response.data))
          .catch(error => console.log(error))     
    }
  },
```
