### Prepare building packages

FROM alpine:latest

ENV NGINX_VERSION=1.17.9
ENV DEV_PKGS="linux-headers build-base zlib-dev pcre-dev openssl-dev wget"
ENV ENV_PKGS="pcre libbz2 ca-certificates openssl tzdata"

RUN \
    apk update && apk --no-cache add --virtual build_deps $DEV_PKGS && \
    apk --no-cache add $ENV_PKGS && \
    cd /tmp && \
    wget https://nginx.org/download/nginx-$NGINX_VERSION.tar.gz -O nginx.tar.gz && \
    tar xzf nginx.tar.gz && \
    cd /tmp/nginx-$NGINX_VERSION && \
    ./configure \
        --prefix=/etc/nginx \
        --sbin-path=/usr/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --pid-path=/var/run/nginx.pid \
        --lock-path=/var/run/nginx.lock \
        --http-client-body-temp-path=/var/cache/nginx/client_temp \
        --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
        --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
        --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
        --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
        --user=nginx \
        --group=nginx \
        --with-http_ssl_module \
        --with-http_realip_module \
        --with-http_addition_module \
        --with-http_sub_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_secure_link_module \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-mail \
        --with-mail_ssl_module \
        --with-file-aio \
        --with-threads \
        --with-stream \
        --with-stream_ssl_module \
        --with-stream_realip_module \
        --with-http_slice_module \
        --with-http_v2_module && \
    make && make install && \
    addgroup -g 8082 -S www-data && \
    adduser -u 8082 -D -S -G www-data www-data && \
    mkdir -p /etc/nginx/conf.d && mkdir -p /etc/nginx/sites-enabled && chown -R www-data:www-data /etc/nginx && \
    mkdir -p /var/cache/nginx && chown -R www-data:www-data /var/cache/nginx && \
    mkdir -p /var/www && chown -R www-data:www-data /var/www && chown -R www-data:www-data /var/log/nginx && \
    cd .. && rm -rf nginx && apk del build_deps && rm /var/cache/apk/*

COPY nginx.conf /etc/nginx/nginx.conf

VOLUME ["/var/cache/nginx"]
VOLUME ["/var/log/nginx"]
VOLUME ["/var/www"]
VOLUME ["/etc/nginx/conf.d"]
VOLUME ["/etc/nginx/sites-enabled"]

EXPOSE 80 443

CMD ["/usr/sbin/nginx", "-g", "daemon off;"]


