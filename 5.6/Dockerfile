FROM php:5.6-fpm-alpine

LABEL author="Outshine"

# 设置时区为上海
RUN apk add tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk del tzdata

# 安装一些依赖和（我认为）必要的组件
RUN apk add --no-cache supervisor autoconf libjpeg-turbo-dev libpng-dev freetype-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install bcmath pdo_mysql mysqli pcntl gd opcache zip

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# 安装composer并设置laravel-china为composer的镜像源，然后引入能并行下载composer的包
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && chmod a+x /usr/local/bin/composer \
    && composer global require hirak/prestissimo -o --prefer-dist

VOLUME [ "/code" ]

WORKDIR /code

EXPOSE 9000

STOPSIGNAL SIGQUIT

COPY entrypoint /usr/local/bin/

RUN chmod +x /usr/local/bin/entrypoint

ENTRYPOINT [ "entrypoint" ]

CMD [ "php-fpm" ]