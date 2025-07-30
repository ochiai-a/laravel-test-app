# Laravel 12 Docker 開発環境

このリポジトリは、Laravel 12 の開発環境を Docker 上に構築するためのテンプレートです。PHP（FPM）、Nginx、MySQL を使用し、Laravel のインストール前までの状態を構成しています。

## 📁 ディレクトリ構成

```plaintext
laravel-test-app/
├── docker/
│   ├── php/
│   │   └── Dockerfile
│   └── nginx/
│       └── conf.d
│   　　　  └── default.conf
└── docker-compose.yml
```


## 🚀 使用技術

- PHP 8.3 (FPM)
- Nginx (stable)
- MySQL 8.0
- Docker / Docker Compose

　
　
# 🐳 Docker × Laravel ハンズオン

## 📦 事前準備

以下のソフトウェアがインストールされていることを確認してください。

- Docker Desktop  
  - [Windows 向けセットアップ手順](https://qiita.com/zembutsu/items/a98f6f25ef47c04893b3)  
  - [Mac 向けセットアップ手順](https://qiita.com/cleyera_f/items/bdd3d33f13527604a663)
- Git
- VS Code（推奨）  
  - 推奨拡張機能：`Docker`, `PHP Intelephense`


## 🛠 セットアップ手順

### 1. リポジトリをクローン

```bash
git clone https://github.com/ochiai-a/laravel-test-app.git
cd laravel-test-app
```

### 2. Laravel プロジェクトの作成

```bash
docker compose run --rm app bash
```

コンテナ内で以下を実行
```bash
composer create-project laravel/laravel:^12.0 .
exit
```

### 3. コンテナの起動

```bash
docker compose up -d --build
docker compose exec app bash
```

再度コンテナ内に入って以下を実行
```bash
chown -R www-data:www-data storage bootstrap/cache
chmod -R 775 storage bootstrap/cache
chmod 666 database/database.sqlite
exit
```

### . Laravel アプリにアクセス
ブラウザで以下にアクセスしてください。

http://localhost:8001
Laravel の初期画面が表示されれば成功 🎉　
※ docker-compose.yml で 8001:80 にポートマッピングされています。

　　
---


# 🚀 Laravel ハンズオン①：「Hello Laravel!」を表示しよう
## 🛠 セットアップ手順

### 🏁 Step 1: `/hello` に「Hello Laravel!」を表示しよう

#### 🎯 目標

Laravel アプリのトップページ（`http://localhost:8001/hello`）に「Hello Laravel!」という文字列を表示します。
---

### 🧭 手順

### ✅ 1. コントローラを作成する

```bash
docker compose exec app php artisan make:controller HelloController
```
```app/Http/Controllers/HelloController.php```が生成されます。

### ✅ 2. コントローラにアクションを追加する
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class HelloController extends Controller
{
    public function index()
    {
        return view('hello');
    }
}
```
index() メソッドが /hello に対応する処理を記述します。

### ✅ 3. ルートを定義する
```routes/web.php```を開いて、以下のように編集します。
```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\HelloController;

Route::get('/', function () {
    return view('welcome');
});

Route::get('/hello', [HelloController::class, 'index']);
```
> ```use``` 文でコントローラを読み込み、```[クラス名, メソッド名]``` 形式でルーティングします。
> 

> 💡 Route::get() は「GETリクエストでアクセスされたときにどう処理するか」を定義する関数です。
> 今回の場合はGETリクエストで「http://localhost:8001/hello」 にアクセスした場合、`hello.blade.php` というviewファイルを返す（表示する）という意味になっています。

http://localhost:8001/hello にアクセスしてみると……？

### ✅ 4. ビューを作成する
```resources/views/```ディレクトリに```hello.blade.php```というファイルを作成し、以下の内容を記述します。
```php
<!-- resources/views/hello.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello Laravel</title>
</head>
<body>
    <h1>Hello Laravel!</h1>
</body>
</html>
```

>💡 ◯◯.blade.php は Laravel のテンプレートエンジン「Blade」のファイル拡張子です。
> HTML/CSSとほぼ同じような書き方ができます。

### ✅ 5. ブラウザで確認
ブラウザで以下のURLにアクセスします。

http://localhost:8001/hello 

「Hello Laravel!」と表示されていれば成功です 🎉

---

  
# 🚀 Laravelハンズオン②：ページを切り替えてみよう！
## 🛠 セットアップ手順

### 🏁 Step 2: Bladeのレイアウトやコントローラの動的ルーティングを使ってみよう！

#### 🎯 目標

任意のURL（例：`/page/about` や `/page/contact`）にアクセスすると、それぞれのページが Blade レイアウト付きで表示されるようにする。


### 🧭 手順

### ✅ 1. レイアウトファイルを作成する

`resources/views/layouts/app.blade.php`を作成します。
```php
<!-- layouts/app.blade.php -->
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>@yield('title')</title>
</head>
<body>
    <header>
        <h1>My Laravel Site</h1>
    </header>

    <main>
        @yield('content')
    </main>

    <footer>
        <p>&copy; 2025 Laravel Hands-on</p>
    </footer>
</body>
</html>
```
>💡 `resources/views/` の下に更に`layouts` フォルダを作り、`app.blade.php`ファイルを作ります。

#### 🧠 Blade のレイアウトとは？

Bladeでは、「共通レイアウト（テンプレート）」と「個別ページの内容」を分けて記述することで、**HTMLを効率よく再利用**できます。

🧩 レイアウト：Webページの「骨組み」（共通枠）  
🧩 セクション：骨組みに「はめこむ中身」

#### 🛠 `@yield` と `@section` の役割

| 構文 | 役割 | 書く場所 |
| --- | --- | --- |
| `@yield('名前')` | 中身の「差し込み口」を作る | レイアウトファイル（layouts） |
| `@section('名前')` | 差し込む中身を書く | 個別ページ（各ビュー） |
| `@extends('レイアウトファイル')` | どのレイアウトを使うか宣言 | 個別ページの先頭 |

### ✅ 2. 動的ルートを設定する
`routes/web.php` に以下を追加します。
```php
use Illuminate\Support\Facades\Route;

……
＜中略＞
……

Route::get('/pages/{name}', function ($name) {
    if (view()->exists("pages.$name")) {
        return view("pages.$name")->with('title', ucfirst($name));
    }
    abort(404);
});
```
> 💡 pages/{name}.blade.php が存在する場合のみ表示。なければ 404 を返します。

### ✅ 3. ページビューを作成する
例1. ：`resources/views/pages/about.blade.php`
```php
@extends('layouts.app')

@section('title', $title)

@section('content')
    <h2>About Page</h2>
    <p>This is the about page content.</p>
@endsection
```
例2. ：`resources/views/pages/contact.blade.php`
```php
@extends('layouts.app')

@section('title', $title)

@section('content')
    <h2>Contact Page</h2>
    <p>You can reach us at contact@example.com</p>
@endsection
```

### ✅ 4. ブラウザで確認する
- `http://localhost:8001/page/about`
- `http://localhost:8001/page/contact`

それぞれのページがレイアウト付きで表示されれば成功です 🎉

## チャレンジ
### /hello/{name}に同じようにページを作成してみよう！
{name}には自分の名前を入れて、レイアウトも好きなように変えてみてください！
