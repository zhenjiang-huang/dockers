FROM php:7.3-fpm

ADD php.ini /usr/local/etc/php/php.ini

WORKDIR /etc/software/plugins

ADD plugins/libzip-1.3.0.tar.gz /etc/software/plugins/

COPY plugins/redis.tgz /etc/software/plugins/

WORKDIR /etc/software/plugins/libzip-1.3.0

ADD plugins/composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer

RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng-dev \
		zlib1g-dev \
	&& ./configure && make && make install \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) mysqli pdo_mysql \
    && docker-php-ext-install -j$(nproc) zip \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    && pecl install /etc/software/plugins/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/php.ini \
    && pecl install swoole && echo "extension=swoole.so" >> /usr/local/etc/php/php.ini

