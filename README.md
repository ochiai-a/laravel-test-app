# Laravel 12 Docker 開発環境

このリポジトリは、Laravel 12 の開発環境を Docker 上に構築するためのテンプレートです。PHP（FPM）、Nginx、MySQL を使用し、Laravel のインストール前までの状態を構成しています。

## 📁 ディレクトリ構成

```plaintext
docker-laravel12/
├── docker/
│   ├── php/
│   │   └── Dockerfile
│   └── nginx/
│       └── default.conf
└── docker-compose.yml
```


## 🚀 使用技術

- PHP 8.3 (FPM)
- Nginx (stable)
- MySQL 8.0
- Docker / Docker Compose

## 🛠 セットアップ手順

### 1. リポジトリをクローン

```bash
git clone https://github.com/ochiai-a/laravel-test-app.git
cd laravel-test-app
2. Laravel プロジェクトの作成
docker-compose run --rm app bash
コンテナ内で以下を実行
composer create-project laravel/laravel:^12.0 .
exit
3. .env ファイルの作成と編集
cp .env.example .env
.env 内の以下の項目を確認・修正してください：

DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
4. コンテナの起動
bash
docker compose up -d --build
docker compose exec app bash
再度コンテナ内に入って以下を実行
chown -R www-data:www-data storage bootstrap/cache
chmod -R 775 storage bootstrap/cache
chmod 666 database/database.sqlite
exit
5. Laravel アプリにアクセス
ブラウザで以下にアクセスしてください：

http://localhost:8001
※ docker-compose.yml で 8001:80 にポートマッピングされています。
