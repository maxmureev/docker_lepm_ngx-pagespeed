ARG ALPINE_VERSION
FROM alpine:${ALPINE_VERSION}

ENV SOURCE_DIR=/tmp/src
ARG MEMCACHED_VERSION
ARG TIMEZONE

RUN apk update && \
  apk add tzdata && \
  cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
  echo "${TIMEZONE}" > /etc/timezone

RUN apk update && \
  apk add \
    --virtual .build-deps \
    gcc \
    libc-dev \
    libevent-dev \
    linux-headers \
    make \
    perl \
    tar

RUN adduser -D memcached && \
  mkdir -p ${SOURCE_DIR} && \
  cd ${SOURCE_DIR} && \
  wget http://memcached.org/files/memcached-${MEMCACHED_VERSION}.tar.gz --quiet && \
  tar -xzf memcached-${MEMCACHED_VERSION}.tar.gz && \
  cd memcached-${MEMCACHED_VERSION} && \
  ./configure && \
  make -j$(getconf _NPROCESSORS_ONLN) && \
  make install && \
  runDeps="$( \
    scanelf --needed --nobanner --recursive /usr/local \
    | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
    | sort -u \
    | xargs -r apk info --installed \
    | sort -u \
    )" && \
  apk add --virtual .memcached-rundeps $runDeps && \
  apk del .build-deps && \
  rm -rf ${SOURCE_DIR}

CMD ["memcached", "-u", "memcached","-m", "512"]
