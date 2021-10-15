FROM php:7.4.24-apache

RUN sed -i s@/deb.debian.org/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN sed -i s@/security.debian.org/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN apt-get clean
RUN apt-get update

RUN docker-php-ext-install -j$(nproc) bcmath calendar exif gettext sockets dba mysqli pcntl pdo_mysql shmop sysvmsg sysvsem sysvshm

#redis
RUN pecl install redis && docker-php-ext-enable redis

#memcached
RUN apt-get install -y --no-install-recommends zlib1g-dev libmemcached-dev 
RUN rm -r /var/lib/apt/lists/*
RUN pecl install memcached && docker-php-ext-enable memcached

#xhprof
RUN pecl install xhprof

# imagemagick
RUN apt-get update
RUN apt-get install -y imagemagick
RUN apt-get install -y --no-install-recommends libmagickwand-dev && rm -rf /var/lib/apt/lists/* 
RUN pecl install imagick && docker-php-ext-enable imagick

# swoole
RUN pecl install swoole && docker-php-ext-enable swoole

# mongodb
RUN pecl install mongodb

RUN sed -i '70 i\ServerName localhost:80' /etc/apache2/apache2.conf
RUN sed -i 's/LogLevel warn/LogLevel error/' /etc/apache2/apache2.conf
RUN sed -i 's/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
RUN sed -i 's/\/var\/www\//\/var\/www\/html\/public\//' /etc/apache2/apache2.conf
RUN sed -i 's/\/var\/www\/html/\/var\/www\/html\/public/' /etc/apache2/sites-enabled/000-default.conf

RUN mkdir /var/www/html/public
RUN chmod -R 777 /var/www/html/
RUN chown -R www-data:www-data /var/www/html/

RUN a2enmod rewrite

RUN ["service","apache2","restart"]