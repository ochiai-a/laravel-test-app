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

以下のソフトウェアがインストールされていることを確認してください：

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
Laravel の初期画面が表示されれば成功 🎉
※ docker-compose.yml で 8001:80 にポートマッピングされています。
