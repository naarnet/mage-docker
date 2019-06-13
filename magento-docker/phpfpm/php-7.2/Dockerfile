FROM php:7.2-fpm
MAINTAINER Nestor Alain <naarnet10@gmail.com>

RUN apt-get update \
  && apt-get install -y \
    cron \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libxslt1-dev \
    git \
    ssh \
    ca-certificates \
    mysql-client

RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install \
  gd \
  intl \
  mbstring \
  pdo_mysql \
  soap \
  xsl \
  zip

RUN curl -sS https://getcomposer.org/installer | \
    php -- \
      --install-dir=/usr/local/bin \
      --filename=composer \
      --version=1.1.2

ENV PHP_MEMORY_LIMIT 2G
ENV PHP_PORT 9000
ENV PHP_PM dynamic
ENV PHP_PM_MAX_CHILDREN 10
ENV PHP_PM_START_SERVERS 4
ENV PHP_PM_MIN_SPARE_SERVERS 2
ENV PHP_PM_MAX_SPARE_SERVERS 6
ENV APP_MAGE_MODE default

COPY bin/* /usr/local/bin/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/xdebug.ini /usr/local/etc/php/conf.d/

RUN chmod +x /usr/local/bin/start

ENV XDEBUG_CONFIG remote_host={{172.17.0.1}}

RUN apt-get update \
  && apt-get install -y git \
     mysql-client \
     vim

RUN docker-php-ext-install bcmath
#RUN yes | pecl install xdebug

RUN echo "America/Mexico_City" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

WORKDIR /srv/www

RUN sed -i '/session    required     pam_loginuid.so/c\#session    required   pam_loginuid.so' /etc/pam.d/cron

RUN chmod -R 777 /var/www

CMD ["/usr/local/bin/start"]
