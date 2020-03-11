ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm
ADD sources.list /etc/apt/
RUN apt-get update
RUN apt-get install zlib1g=1:1.2.8.dfsg-5 -y --allow-downgrades
#RUN apt-get install libssl1.1=1.1.0l-1~deb9u1 -y --allow-downgrades
#openssl libssl-dev libcurl4-openssl-dev
RUN apt-get install -y gcc g++ make autoconf zlib1g-dev libwebp-dev libjpeg-dev libc6-dev libc-dev libpng-dev zip libzip-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev git

RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install gd && docker-php-ext-enable gd 
RUN docker-php-ext-install zip pdo_mysql mysqli sockets
ENV LD_LIBRARY_PATH /usr/local/lib
ENV PATH $LD_LIBRARY_PATH/bin:$PATH
RUN mkdir /data

ARG PHP_REDIS=false
RUN if [ ${PHP_REDIS} != false ]; then \
	cd /data \
    && curl -O http://pecl.php.net/get/redis-${PHP_REDIS}.tgz -L \
    && tar xvf redis-${PHP_REDIS}.tgz \
    && rm -rf redis-${PHP_REDIS}.tgz \
    && cd redis-${PHP_REDIS} \
    && /usr/local/bin/phpize \
    && ./configure --with-php-config=/usr/local/bin/php-config \
    && make && make install \
    && cd ../ && rm -rf redis-${PHP_REDIS} \
    && docker-php-ext-enable redis \
;fi

ARG PHP_SWOOLE=false
RUN if [ ${PHP_SWOOLE} != false ]; then \
    cd /data \
    && curl -O http://pecl.php.net/get/swoole-${PHP_SWOOLE}.tgz -L \
    && tar xvf swoole-${PHP_SWOOLE}.tgz \
    && rm -rf swoole-${PHP_SWOOLE}.tgz \
    && cd swoole-${PHP_SWOOLE} \
    && /usr/local/bin/phpize \
    && ./configure \
    && make && make install \
    && rm -rf swoole-${PHP_SWOOLE} \
    && docker-php-ext-enable swoole \
;fi

ARG PHP_XDEBUG=false
RUN if [ ${PHP_XDEBUG} != false ]; then \
	cd /data \
    && curl -O http://pecl.php.net/get/xdebug-${PHP_XDEBUG}.tgz -L \
	&& pecl install xdebug-${PHP_XDEBUG}.tgz \
	&& rm -rf xdebug-${PHP_XDEBUG}.tgz \
	&& docker-php-ext-enable xdebug \
;fi

#RUN cd /data && curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ 

RUN rm -rf /data
