FROM php:7.4-fpm-bullseye

LABEL maintainer="dev"

# ========== 系统依赖 ==========
RUN apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libzip-dev \
        libicu-dev \
        libonig-dev \
        libxml2-dev \
        libaio1 \
        libaio-dev \
        unzip \
        git \
        curl \
    && docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
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
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# ========== Oracle Instant Client + OCI8 ==========
RUN mkdir -p /opt/oracle \
    && cd /opt/oracle \
    && curl -sSL -o basic.zip \
        "https://download.oracle.com/otn_software/linux/instantclient/2115000/instantclient-basic-linux.x64-21.15.0.0.0dbru.zip" \
    && curl -sSL -o sdk.zip \
        "https://download.oracle.com/otn_software/linux/instantclient/2115000/instantclient-sdk-linux.x64-21.15.0.0.0dbru.zip" \
    && unzip -q basic.zip \
    && unzip -q sdk.zip \
    && rm basic.zip sdk.zip \
    && mv instantclient_* instantclient \
    && echo /opt/oracle/instantclient > /etc/ld.so.conf.d/oracle.conf \
    && ldconfig \
    && docker-php-ext-configure oci8 --with-oci8=instantclient,/opt/oracle/instantclient \
    && docker-php-ext-install -j$(nproc) oci8 \
    && rm -rf /opt/oracle/instantclient/sdk

# ========== Composer ==========
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/html

CMD ["php-fpm"]
