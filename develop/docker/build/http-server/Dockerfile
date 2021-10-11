FROM php:8.0-apache

RUN apt-get update && apt-get install -y \
    libpq-dev \
    libonig-dev \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
		libzip-dev \
    && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip \
    && docker-php-ext-install -j$(nproc) gd

RUN a2enmod rewrite && mkdir /composer-setup && wget https://getcomposer.org/installer -P /composer-setup \
&& php /composer-setup/installer --install-dir=/usr/bin && rm -Rf /composer-setup \
&& ln -s /usr/bin/composer.phar /usr/local/bin/composer

# Xdebug
RUN pecl install install xdebug-3.0.4 \
    && docker-php-ext-enable xdebug

ENV XDEBUGINI_PATH=/usr/local/etc/php/conf.d/xdebug.ini
RUN echo "zend_extension="`find /usr/local/lib/php/extensions/ -iname 'xdebug.so'` > $XDEBUGINI_PATH
COPY xdebug.ini /tmp/xdebug.ini
RUN cat /tmp/xdebug.ini >> $XDEBUGINI_PATH
RUN echo "xdebug.remote_host="`/sbin/ip route|awk '/default/ { print $3 }'` >> $XDEBUGINI_PATH
