FROM php:7.4-apache-bullseye

ARG VERSION=3.5.1

RUN apt-get update; \
    apt-get upgrade; \
    apt-get -qy install cron curl git wget; \
	docker-php-ext-install pdo pdo_mysql sockets; \
	apt-get clean && rm -rf /var/lib/apt/lists/*

ENV PHPMONITOR_URL https://github.com/phpservermon/phpservermon/archive/v$VERSION.tar.gz

RUN set -x; \
	rm -rf /var/www/html/*; \
    wget $PHPMONITOR_URL -O /tmp/phpmonitor.tar.gz; \
	tar -xvf /tmp/phpmonitor.tar.gz --strip-components=1 -C /var/www/html/; \
	cd /var/www/html; \
	touch config.php && chmod 0777 config.php; \
	php composer.phar install; \
	cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini; \
	sed -i 's/;date.timezone =/date.timezone = UTC/' /usr/local/etc/php/php.ini

COPY ["dockerfile/latest/entrypoint.sh", "/"]

RUN chmod +x /entrypoint.sh

ENTRYPOINT [ "/entrypoint.sh" ]
