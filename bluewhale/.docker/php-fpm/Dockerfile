### LOCAL DEV CONFIG
#FROM php:7.3-fpm-alpine
FROM php:7.3.6-fpm-alpine3.9

RUN echo http://nl.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories; \
    echo http://dl-2.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories; \
    echo http://dl-3.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories; \
    echo http://dl-4.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories; \
    echo http://dl-5.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories
RUN apk add --no-cache curl
RUN apk add --update \
     acl \
     sudo \
     bash \
     shadow \
     postgresql-dev \
     nano \
     openssl \
     mysql-client \
     libpng-dev \
     grep \
     git \
     tcpdump \
     libzip-dev

RUN docker-php-ext-install pdo pdo_pgsql pdo_mysql \
    zip \
    pcntl

RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-2.7.2 \
    && docker-php-ext-enable xdebug \
    && apk del --purge autoconf g++ make

ADD .docker/php-fpm/php.ini /usr/local/etc/php/php.ini

RUN rm -rf /var/cache/apk/* && rm -rf /tmp/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

RUN groupmod -g 1000 www-data

RUN usermod -u 1000 www-data

RUN mkdir -p /var/www/webapp

RUN chown -R 1000:1000 /var/www/webapp

ADD . /var/www/webapp

WORKDIR /var/www/webapp

RUN useradd -g root www-data & groups

RUN composer install --no-suggest && \
    composer clear-cache

RUN chown -R 1000:1000 /var/www/webapp

#RUN chown -R 1000:1000 /var/www/webapp/var /var/www/webapp/tests /var/www/webapp/public
#RUN chown -R 1000:1000 /var/www/webapp/var /var/www/webapp/tests /var/www/webapp/public

USER www-data

RUN ls -la .

EXPOSE 9000
