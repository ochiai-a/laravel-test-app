FROM php:8.3-fpm

# 必要なパッケージのインストール
RUN apt-get update && apt-get install -y \
    zip unzip git curl libzip-dev libonig-dev libpng-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl

# Composerインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

