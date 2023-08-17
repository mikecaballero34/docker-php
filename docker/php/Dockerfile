FROM php:8.0-fpm-buster

RUN apt-get update && apt-get install -y \
    git \
    unzip \
    iputils-ping \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libzip-dev \
    curl \
    default-mysql-client

RUN apt-get -y install libmagickwand-dev --no-install-recommends \
    && pecl install \
    imagick \
    && docker-php-ext-enable \
    imagick

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure hash --with-mhash
RUN docker-php-ext-install -j$(nproc) gd zip pdo_mysql mysqli

RUN useradd -m -d /home/hacker -s /bin/bash hacker && adduser hacker sudo

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-xdebug.ini && \
    echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-xdebug.ini && \
    echo "xdebug.client_port=9003" >> /usr/local/etc/php/conf.d/docker-php-xdebug.ini && \
    echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-xdebug.ini

COPY docker/php/php.ini /usr/local/etc/php/php.ini

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp

WORKDIR /var/www

RUN usermod -aG www-data hacker
RUN chown -R hacker:www-data /var/www
RUN chmod -R 755 /var/www

CMD ["php-fpm"]
