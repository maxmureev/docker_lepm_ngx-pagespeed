ARG UBUNTU_VERSION
FROM ubuntu:${UBUNTU_VERSION}

ENV SOURCE_DIR=/tmp
ARG NGINX_VERSION
ARG NPS_VERSION
ARG OPENSSL_VERSION

RUN apt-get update && \
  apt-get -y install \
    dnsutils \
    iputils-ping \
    unzip \
    gzip \
    wget \
    autoconf \
    automake \
    libtool \
    build-essential \
    zlib1g-dev \
    libpcre3-dev \
    libxslt1-dev \
    libxml2-dev \
    libgd2-xpm-dev \
    libgeoip-dev \
    libgoogle-perftools-dev \
    libperl-dev \
    libjpeg-dev \
    libpng-dev \
    libtiff-dev \
    libgif-dev \
    uuid-dev

RUN cd ${SOURCE_DIR} && \
  wget https://github.com/pagespeed/ngx_pagespeed/archive/v${NPS_VERSION}-stable.zip --quiet && \
  unzip v${NPS_VERSION}-stable.zip

RUN cd ${SOURCE_DIR}/incubator-pagespeed-ngx-${NPS_VERSION}-stable/ && \
  psol_url=$(scripts/format_binary_url.sh PSOL_BINARY_URL) && \
  wget ${psol_url} --quiet && \
  tar -xzvf $(basename ${psol_url})

RUN cd ${SOURCE_DIR} && \
  wget https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz --quiet && \
  tar -xvzf openssl-${OPENSSL_VERSION}.tar.gz

RUN cd ${SOURCE_DIR} && \
  wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz --quiet && \
  tar -xvzf nginx-${NGINX_VERSION}.tar.gz

RUN mkdir -p /var/log/nginx /var/cache/nginx

RUN cd ${SOURCE_DIR}/nginx-${NGINX_VERSION}/ && \
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
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_auth_request_module \
    --without-http_autoindex_module \
    --without-http_ssi_module \
    --with-threads \
    --with-stream \
    --with-stream_ssl_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \
    --with-http_v2_module \
    --with-cc-opt='-g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2' \
    --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,--as-needed' \
    --with-ipv6 \
    --with-pcre-jit \
    --with-openssl=${SOURCE_DIR}/openssl-${OPENSSL_VERSION} \
    --add-module=${SOURCE_DIR}/incubator-pagespeed-ngx-${NPS_VERSION}-stable && \
  make && \
  make install

RUN apt-get purge -y \
  automake \
  autoconf \
  libtool \
  build-essential \
  zlib1g-dev \
  libpcre3-dev \
  libxslt1-dev \
  libxml2-dev \
  libgd2-xpm-dev \
  libgeoip-dev \
  libgoogle-perftools-dev \
  libperl-dev && \
  apt-get autoremove -y && \
  apt-get clean && \
  rm -Rf /tmp/* \
  /var/tmp/* \
  /var/lib/apt/lists/*

RUN mkdir /var/www/ && \
  mkdir /etc/letsencrypt/ && \
  useradd -r -s /usr/sbin/nologin nginx && \
  chown nginx:nginx /var/www/ -R

ADD nginx.conf /etc/nginx/nginx.conf
ADD pagespeed.conf /etc/nginx/pagespeed.conf
ADD conf.d/ /etc/nginx/conf.d/
#ADD dhparam.pem /etc/nginx/dhparam.pem
EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]
