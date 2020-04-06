FROM alpine:latest

### Enable SSL repos ###

RUN \
  sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/https\:\/\/alpine.global.ssl.fastly.net/g' /etc/apk/repositories && \
  apk update && apk upgrade

### Download packages ###

RUN apk --no-cache add \
    bash \
    zip unzip \
    php7 \
    php7-ctype \
    php7-curl \
    php7-dom \
    php7-fileinfo \
    php7-fpm \
    php7-ftp \
    php7-gd \
    php7-iconv \
    php7-intl \
    php7-json \
    php7-mbstring \
    php7-mysqlnd \
    php7-openssl \
    php7-pdo \
    php7-pdo_sqlite \
    php7-pdo_mysql \
    php7-pdo_pgsql \
    php7-pear \
    php7-phar \
    php7-posix \
    php7-session \
    php7-simplexml \
    php7-sqlite3 \
    php7-tokenizer \
    php7-xml \
    php7-xmlreader \
    php7-xmlwriter \
    php7-zip \
    php7-zlib

### Add www-data:www-data (8082:8082 uid/guid) ###

RUN addgroup -g 8082 -S www-data && \
    adduser -u 8082 -D -S -G www-data www-data

### Set conf.d directory ###

VOLUME ["/etc/php7/php-fpm.d"]

### Set port to expose ###

EXPOSE 9000

### Start process ###

CMD ["/usr/sbin/php-fpm7", "-F", "-R"]
