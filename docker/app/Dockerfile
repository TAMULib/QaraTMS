FROM php:8.3-fpm

ENV COMPOSER_ALLOW_SUPERUSER=1

# Install system dependencies
RUN apt-get update && \
    apt-get install -y libonig-dev libpng-dev zlib1g-dev libxml2-dev zip unzip vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tpm/* /var/tmp/* && \
    mkdir /opt/www && \
    rm -rf /var/www/*

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql gd mbstring exif pcntl bcmath xml

COPY ./docker/app/php.ini /usr/local/etc/php/conf.d/php.ini

RUN curl -sS https://getcomposer.org/installer | php -- \
    --filename=composer --install-dir=/usr/local/bin

COPY . /opt/www

RUN chown -R www-data:www-data /opt/www

WORKDIR /opt/www

RUN mv .env.backup .env && \
    composer install && \
    composer require doctrine/dbal

RUN php artisan key:generate && \
    php artisan cache:clear && \
    php artisan config:clear && \
    php artisan view:clear && \
    php artisan optimize:clear

WORKDIR /var/www

EXPOSE 9000

