FROM seansin/ubuntu:latest

MAINTAINER seansin <seansin@chungbuk.ac.kr>

ENV NGINX_VERSION 1.12.0
ENV PCRE_VERSION 8.39 
ENV OPENSSL_VERSION 1.0.2q
ENV ZLIB_VERSION 1.2.11
ENV ECHO_NGINX_MODULE_VERSION 0.61

RUN apt-get -qq -y install build-essential wget unzip make && \
    apt-get -qq -y install libssl-dev libpcre3-dev && \
    mkdir -p /tmp/src && cd /tmp/src && \
    wget -q -O - https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz | tar xfz - && \
    wget -q -O - ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-${PCRE_VERSION}.tar.gz | tar xfz - && \
    wget -q -O - http://prdownloads.sourceforge.net/libpng/zlib-${ZLIB_VERSION}.tar.gz?download | tar xfz - && \ 
    wget -q -O - https://github.com/openresty/echo-nginx-module/archive/v${ECHO_NGINX_MODULE_VERSION}.tar.gz | tar xfz - && \
    wget -q -O - http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz | tar xfz -

RUN cd /tmp/src/nginx-${NGINX_VERSION} && \
    ./configure \
        --prefix=/usr/local/nginx \
        --sbin-path=/usr/local/sbin \
        --with-http_realip_module \
        --with-http_ssl_module \
        --with-debug \ 
        --with-http_stub_status_module \
        --with-http_gzip_static_module \
        --with-zlib=../zlib-${ZLIB_VERSION} \
        --with-pcre=../pcre-${PCRE_VERSION} \ 
        --with-openssl=../openssl-${OPENSSL_VERSION} \
        --add-module=../echo-nginx-module-${ECHO_NGINX_MODULE_VERSION} && \ 
    make --silent && \
    make install --silent && \
    apt-get -qq -y --purge remove build-essential wget && \
    apt-get -qq -y autoremove && \
    apt-get clean && \
    rm -rf /tmp/src

ADD ./nginx.conf /usr/local/nginx/conf/nginx.conf
ADD ./proxy_params /usr/local/nginx/conf/proxy_params

# volume
VOLUME /usr/local/nginx/conf/certs
VOLUME /usr/local/nginx/conf/conf.d
VOLUME /usr/local/nginx/conf/sites
VOLUME /usr/local/nginx/conf/streams

WORKDIR /usr/local/nginx

EXPOSE 80 443
CMD /usr/local/sbin/nginx
