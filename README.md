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
2. Laravel プロジェクトの作成（まだインストールしていない場合）
bash
docker compose run --rm app composer create-project laravel/laravel .
3. .env ファイルの作成と編集
bash
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
5. Laravel アプリにアクセス
ブラウザで以下にアクセスしてください：

http://localhost:8001
※ docker-compose.yml で 8001:80 にポートマッピングされています。

🧪 よくあるトラブルと対処法
このサイトにアクセスできません と表示される場合 → default.conf が正しくマウントされているか、Nginx の設定が正しいか確認してください。

Laravel の public ディレクトリが表示されない場合 → root /var/www/public; の設定と Laravel のインストール先を確認してください。
