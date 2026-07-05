FROM php:8.2-fpm-alpine

LABEL maintainer="dev"

# 系统依赖
RUN apk add --no-cache \
        $PHPIZE_DEPS \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libzip-dev \
        icu-dev \
        oniguruma-dev \
        libxml2-dev \
        linux-headers \
        unzip \
        git \
        curl \
    # GD 配置
    && docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
    # 编译安装 PHP 扩展
    && docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        mysqli \
        gd \
        zip \
        intl \
        mbstring \
        bcmath \
        opcache \
        soap \
        exif \
    # PECL 扩展
    && pecl install redis \
    && docker-php-ext-enable redis \
    # 安装运行时依赖库，然后清理 -dev 包
    && apk add --no-cache \
        freetype \
        libjpeg-turbo \
        libpng \
        libwebp \
        libzip \
        icu \
        oniguruma \
        libxml2 \
    && apk del \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libzip-dev \
        icu-dev \
        oniguruma-dev \
        libxml2-dev \
        linux-headers

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/html

CMD ["php-fpm"]
