FROM mageinferno/magento2-php:7.1-fpm-0
MAINTAINER Nestor Alain <naarnet10@gmail.com>

ENV PHP_MEMORY_LIMIT 2G
ENV PHP_PORT 9000
ENV PHP_PM dynamic
ENV PHP_PM_MAX_CHILDREN 10
ENV PHP_PM_START_SERVERS 4
ENV PHP_PM_MIN_SPARE_SERVERS 2
ENV PHP_PM_MAX_SPARE_SERVERS 6
ENV PHP_UPLOAD_MAX_FILE_SIZE 150M
ENV PHP_POST_MAX_SIZE 150M
ENV PHP_MAX_EXECUTION_TIME 600
ENV APP_MAGE_MODE default
ENV XDEBUG_CONFIG remote_host={{172.17.0.1}}

RUN apt-get update \
  && apt-get install -y git \
     mysql-client \
     vim

RUN docker-php-ext-install bcmath
RUN yes | pecl install xdebug

COPY bin/* /usr/local/bin/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/xdebug.ini /usr/local/etc/php/conf.d/

RUN echo "America/Mexico_City" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

WORKDIR /srv/www

RUN sed -i '/session    required     pam_loginuid.so/c\#session    required   pam_loginuid.so' /etc/pam.d/cron

CMD ["/usr/local/bin/start"]


