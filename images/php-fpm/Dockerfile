ARG ALPINE_VERSION
FROM alpine:${ALPINE_VERSION}

ARG TIMEZONE

RUN apk update && \
  apk add tzdata && \
  cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
  echo "${TIMEZONE}" > /etc/timezone

RUN apk update && \
  apk add \
    php7-mcrypt \
    php7-soap \
    php7-openssl \
    php7-gmp \
    php7-pdo_odbc \
    php7-json \
    php7-dom \
    php7-pdo \
    php7-zip \
    php7-bcmath \
    php7-gd \
    php7-odbc \
    php7-pdo_mysql \
    php7-gettext \
    php7-xmlreader \
    php7-xmlrpc \
    php7-bz2 \
    php7-iconv \
    php7-pdo_dblib \
    php7-curl \
    php7-ctype \
    php7-fpm \
    php7-apcu \
    php7-mysqli \
    php7-memcached \
    php7-opcache \
    gzip && \
  apk del tzdata && \
  rm -rf /var/cache/apk/*

RUN mkdir -p /var/www/ && \
  adduser -D -s /usr/sbin/nologin nginx && \
  chown nginx:nginx /var/www/ -R

ADD php-fpm.conf /etc/php7/php-fpm.conf
ADD php.ini /etc/php7/php.ini
ADD php-fpm.d /etc/php7/php-fpm.d
CMD ["/usr/sbin/php-fpm7", "--nodaemonize"]
