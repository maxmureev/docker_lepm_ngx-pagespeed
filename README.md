# docker_lepm_ngx-pagespeed
Nginx, ngx-pagespeed module, php-fpm, mariadb, memcached and phpmyadmin in Docker containers

# Prepare
Install docker-compose:
```
pip install docker-compose
```
Clone repo:
```
git clone git@github.com:maxmureev/docker_lepm_ngx-pagespeed.git
```
Copy sites files to `volumes/sites/[site_dir]`.

Start containers:
```
cd docker_lepm_ngx-pagespeed
docker-compose build && docker-compose up -d
```

Create database:
```
# for example.ru
docker exec -it mysql-example.ru mysql -e 'CREATE DATABASE `base_for_example.ru` CHARACTER SET utf8 COLLATE utf8_general_ci;'
# for site.com
docker exec -it mysql-site.com mysql -e 'CREATE DATABASE `base_for_site.com` CHARACTER SET utf8 COLLATE utf8_general_ci;'
```
Or create via phpmyadmin

# Mysql
The MySQL is configured to work without password with a parameter `--skip-grant-tables`

# phpMyAdmin
Connect to database:   
Server: mysql `container_name` (mysql-example.ru or mysql-site.com)   
Username: root   
Password: without password

The phpMyAdmin are intentionally commented out in the docker-compose.yml file, since its inclusion is required only on request   
phpMyAdmin config is taken [here](https://github.com/phpmyadmin/docker)

# Nginx

> Why Ubuntu, not Alpine?

It's easier. Alpine [ should be patched](https://github.com/wernight/docker-alpine-nginx-pagespeed/blob/master/Dockerfile) to build the ngx-pagespeed module and I do not like it, because I do not want to do Alpine for the sake of Alpine

***

Description in Russian [here](https://notessysadmin.com/lemp-ngx_pagespeed-memcached-and-phpmyadmin-in-docker-containers)
