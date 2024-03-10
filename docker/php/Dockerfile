FROM php:8.1-apache
COPY site.conf /etc/apache2/sites-enabled/site.conf
RUN apt-get update
RUN a2enmod rewrite
#opcache
RUN docker-php-ext-install opcache
#gd
RUN apt-get install -y \
libfreetype-dev \
libjpeg62-turbo-dev \
libpng-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
&& docker-php-ext-install -j$(nproc) gd
#data base
RUN docker-php-ext-install mysqli pdo pdo_mysql
#magento requirement
RUN docker-php-ext-install bcmath
RUN apt-get install -y libicu-dev \
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl
RUN apt-get install -y libxml2-dev \
&& docker-php-ext-install soap
RUN docker-php-ext-install sockets
RUN apt-get install -y libxslt-dev \
&& docker-php-ext-install xsl
RUN apt-get install -y \
libzip-dev \
zip \
&& docker-php-ext-install zip
#php.ini configuration
RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN sed -i 's/;realpath_cache_size = 4096k/realpath_cache_size = 10M/g' /usr/local/etc/php/php.ini
RUN sed -i 's/;realpath_cache_ttl = 120/realpath_cache_ttl = 7200/g' /usr/local/etc/php/php.ini
RUN sed -i 's/;date\.timezone =/date\.timezone = "Asia\/Hong_Kong"/g' /usr/local/etc/php/php.ini
RUN sed -i 's/memory_limit = 128M/memory_limit = 2G/g' /usr/local/etc/php/php.ini
#magento setup
# RUN cd /var/www/html
# RUN find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
# RUN find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
# RUN bin/magento cache:clean
# RUN bin/magento cache:flush
EXPOSE 80