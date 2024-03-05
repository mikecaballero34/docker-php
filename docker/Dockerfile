FROM php:8.1-fpm

RUN apt-get update

RUN apt-get install -y default-mysql-client iputils-ping net-tools unzip curl

RUN useradd -m -d /home/mike -s /bin/bash mike && adduser mike sudo

RUN docker-php-ext-install pdo_mysql mysqli

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp

COPY php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY php/php.ini /usr/local/etc/php/php.ini
