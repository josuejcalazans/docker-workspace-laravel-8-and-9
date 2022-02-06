ARG PHP_VERSION=8.1.1
#8.1.1
FROM php:${PHP_VERSION}-fpm-alpine
WORKDIR "/application"
RUN apk add --update --no-cache freetype \
    libpng libjpeg-turbo freetype-dev \
    libpng-dev libjpeg-turbo-dev libwebp-dev \
    && docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg \
    --with-webp \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j$(nproc) gd \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev \
    && mkdir /var/lib/php  \
    && chown www-data:www-data /var/lib/php -R

RUN docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-enable pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Expose port 9000 and start php-fpm server
# EXPOSE 9000
# CMD ["php-fpm"]
