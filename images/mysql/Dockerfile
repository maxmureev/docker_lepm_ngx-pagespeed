ARG ALPINE_VERSION
FROM alpine:${ALPINE_VERSION}

ARG TIMEZONE

RUN apk update && \
  apk add tzdata && \
  cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
  echo "${TIMEZONE}" > /etc/timezone

RUN apk update && \
  apk add \
    mariadb \
    mariadb-client && \
  mkdir -p /var/lib/mysql && \
  mkdir /run/mysqld/ && \
  chmod 770 /var/lib/mysql && \
  chown mysql:mysql /var/lib/mysql -R

COPY my.cnf /etc/mysql/my.cnf
COPY my.cnf /etc/my.cnf
CMD ["mysqld", "--user=root", "--skip-grant-tables"]
