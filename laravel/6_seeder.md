# Laravel6 ざっくりSeederを使う

### ターミナル

```php
php artisan make:seeder 〇〇
```

- database\seeds\〇〇が作成される

### Seeds

```php
public function run()
    {
        DB::table('users')->insert([
            'name' => Str::random(10),
            'email' => Str::random(10) . '@gmail.com',
            'password' => Hash::make('password'),
        ]);
    }
```

### DatebaseSeeder

```php
public function run()
    {
        $this->call(UsersTableSeeder::class);
    }
```

- DBSeederは必ず存在している＋作成したSeederが最初はコメントアウトされている
    - ここから個々のSeederを呼び出して実行する仕組みになっているので、コメントアウトを外しておく

### ターミナル

```php
composer dump-autoload

php artisan db:seed
```

- Seederクラスを書き上げたら、Composerのオートローダを再生成する必要がある
    - composer.jsonのautoloadにdatabase/seeds,factoryの記述があるため、書き換えの際に再生成する必要が出てくる
- もし2つ目のコマンドが効かなかったら、生成したデータと既に存在しているデータでコンフリクトが起こっている可能性がある
    - `php artisan migrate:refresh —seed` でマイグレートのリフレッシュとシーディングを同時に行うことができ、問題が解決する
