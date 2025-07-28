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
docker-compose run --rm app bash
```

コンテナ内で以下を実行
```bash
composer create-project laravel/laravel:^12.0 .
exit
```

### 3. .env ファイルの作成と編集
```bash
cp .env.example .env
```

.env 内の以下の項目を確認・修正してください。

```bash
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

### 4. コンテナの起動

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

### 5. Laravel アプリにアクセス
ブラウザで以下にアクセスしてください。

http://localhost:8001
Laravel の初期画面が表示されれば成功 🎉　
※ docker-compose.yml で 8001:80 にポートマッピングされています。

　

# 🚀 Laravel ハンズオン①：「Hello Laravel!」を表示しよう

## 🧱 事前準備

- Docker Desktop のインストール  
- Laravel 12 のインストール  
- VS Code 推奨（拡張機能：Docker, PHP Intelephense）

---
## 🛠 セットアップ手順

### 🏁 Step 1: `/` に「Hello Laravel!」を表示しよう

#### 🎯 目標

Laravel アプリのトップページ（`http://localhost:8001/`）に「Hello Laravel!」という文字列を表示します。

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
> 今回の場合はGETリクエストで「http://localhost:8001/hello」にアクセスした場合、`hello.blade.php` というviewファイルを返す（表示する）という意味になっています。

### ✅ 4. ビューを作成する
``resources/views/```ディレクトリに```hello.blade.php```というファイルを作成し、以下の内容を記述します。
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

`http://localhost:8001`

「Hello Laravel!」と表示されていれば成功です 🎉
