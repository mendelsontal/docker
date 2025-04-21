### Docker tasks

Summary Practices from Docker Shallow Dive by Alex M. Schapelle VAioLabs.io

## Task One - Networking
- Clean all containers and image from your host
- Create docker network named vaio-net
- Run alpine container on custom docker network named vaio-net
- Run nginx container on custom docker network named vaio-net
- Run ubuntu/apache2 container on default docker network
- Run from alpine container ping command to test connectivity with ubuntu/apache2
- BONUS: connect already running ubuntu/apache2 container to vaio-net
- Delete all the containers images and network

## Task Two - Volumes
- Let’s create a Docker volume and mount it to persist MySQL data:
    - Create volume name mysql-data
    - Run mysql container in the background and mount it with storage
    - Kill mysql container
    - Verify that data still exists
    - Run new mysql container background and mount it with the same storage
    - Test that data is the same and not corrupted (accessing remote folder should be enough)

## Task Three - Dockerfile
- Describe what the next dockerfile does:
```# Download base image ubuntu 20.04
FROM ubuntu:20.04
LABEL maintainer="admin@sysadminjournal.com"
LABEL version="0.1"
LABEL description="This is custom Docker Image."
ARG DEBIAN_FRONTEND=noninteractive
RUN apt update
RUN apt install -y nginx php-fpm supervisor && \
rm -rf /var/lib/apt/lists/* && \
apt clean
ENV nginx_vhost /etc/nginx/sites-available/default
ENV php_conf /etc/php/7.4/fpm/php.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
echo "\ndaemon off;" >> ${nginx_conf}
COPY supervisord.conf ${supervisor_conf}
RUN mkdir -p /run/php && \
chown -R www-data:www-data /var/www/html && \
chown -R www-data:www-data /run/php
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"
COPY start.sh /start.sh
CMD ["./start.sh"]
EXPOSE 80 443```



[Link](https://gitlab.com/vaiolabs-io/docker-shallow-dive)