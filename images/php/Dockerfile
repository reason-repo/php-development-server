FROM php:7.3-fpm

LABEL phptd <admin@mail.ru>

RUN apt-get update && apt-get install -y curl wget git libfreetype6-dev libjpeg62-turbo-dev libzip-dev \
	libmcrypt-dev libpng-dev\
	&& docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip\
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& pecl install -o -f redis \
	&& rm -rf /tmp/pear \
	&& docker-php-ext-enable redis \
	&& pecl install -o -f xdebug \
	&& rm -rf /tmp/pear \
	&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& echo "xdebug.remote_host=192.168.220.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install cron -y

ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

WORKDIR /var/www

CMD ["php-fpm"]
