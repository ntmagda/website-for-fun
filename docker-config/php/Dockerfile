FROM php:7.3-fpm

RUN apt-get update && apt-get install -y \
    libfreetype6-dev libjpeg62-turbo-dev \
    libpng-dev libbz2-dev \
    libssl-dev autoconf \
    ca-certificates curl g++ libicu-dev \
    libmagickwand-dev mariadb-client libzip-dev\
    && \
    pecl install imagick-3.4.3 \
    && \
    docker-php-ext-install \
    bcmath bz2 exif \
    ftp gd gettext mbstring opcache \
    shmop sockets sysvmsg sysvsem sysvshm \
    zip iconv pdo_mysql intl \
    && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && \
    docker-php-ext-enable imagick

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

RUN echo "upload_max_filesize = 16M" > /usr/local/etc/php/php.ini && \
    echo "post_max_size = 16M" >> /usr/local/etc/php/php.ini && \
    echo "max_input_vars = 2048" >> /usr/local/etc/php/php.ini && \
    echo "max_execution_time = 300" >> /usr/local/etc/php/php.ini && \
    echo "memory_limit = 256M" >> /usr/local/etc/php/php.ini

COPY --chown=www-data:www-data ./src/config /var/www/html/config
COPY --chown=www-data:www-data ./src/modules /var/www/html/modules
COPY --chown=www-data:www-data ./src/templates /var/www/html/templates
COPY --chown=www-data:www-data ./src/web /var/www/html/web
COPY --chown=www-data:www-data ./src/composer.json /var/www/html/composer.json
COPY --chown=www-data:www-data ./src/craft /var/www/html/craft

# sets custom max children setting in php-fpm container 
COPY --chown=www-data:www-data ./docker-config/php/www2.conf /usr/local/etc/php-fpm.d/www2.conf

RUN mkdir -p /var/www/html/storage/rebrand && \
    mkdir -p /var/www/html/storage/runtime/mutex && \
    mkdir -p /var/www/html/storage/logs && \
    chown -R www-data:www-data /var/www/html/storage


RUN composer install -d /var/www/html/ && \
    chown -R www-data:www-data /var/www/html/vendor && \
    chown -R www-data:www-data /var/www/html/composer.lock

