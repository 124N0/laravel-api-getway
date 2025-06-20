# Gunakan base image PHP FPM
FROM php:8.2-fpm-alpine

# Install dependensi sistem yang dibutuhkan oleh Laravel
RUN apk add --no-cache \
    nginx \
    build-base \
    nodejs \
    npm \
    zip \
    unzip \
    git \
    supervisor \
    icu-dev \
    postgresql-dev \
    libpq \
    oniguruma-dev \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    libwebp-dev \
    jpeg-dev \
    libxml2-dev \
    libzip-dev \
    && docker-php-ext-install -j$(nproc) pdo_mysql pdo_pgsql mbstring exif pcntl bcmath gd \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install gd \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/*

# Atur direktori kerja
WORKDIR /var/www/html

# Salin composer.lock dan composer.json untuk install dependensi PHP
COPY composer.json composer.lock ./

# # Install Composer dependencies
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
#     && composer install --no-interaction --prefer-dist --optimize-autoloader --no-dev \
#     && rm -rf ~/.composer

# Salin kode aplikasi
COPY . .

# Perbaiki permission
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache \
    && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# Expose port (opsional, untuk FPM)
EXPOSE 9000

# Command untuk menjalankan FPM
CMD ["php-fpm"]