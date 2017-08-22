FROM php:7.0.21-apache

RUN apt-get update \
   && docker-php-ext-install mbstring \
   && docker-php-ext-install pdo_mysql \
   && apt-get install -y libxml2-dev \
       && docker-php-ext-install soap \
   && apt-get install -y libmcrypt4 libmcrypt-dev \
       && docker-php-ext-install mcrypt \
   && apt-get install -y libxslt-dev \
       && docker-php-ext-install xsl \
   && apt-get install -y libicu-dev \
       && docker-php-ext-install intl \
   && apt-get install -y libpng12-dev libjpeg-dev \
       && docker-php-ext-configure gd --with-jpeg-dir=/usr/lib \
       && docker-php-ext-install gd \
   && apt-get install -y zlib1g-dev \
       && docker-php-ext-install zip \
   && apt-get install -y libfreetype6-dev \
	&& docker-php-ext-install bcmath


RUN cd /tmp \
	&& curl -o ioncube.tar.gz http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
    && tar -xvvzf ioncube.tar.gz \
    && mv ioncube/ioncube_loader_lin_7.0.so /usr/local/lib/php/extensions/* \
    && rm -Rf ioncube.tar.gz ioncube \
    && echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20151012/ioncube_loader_lin_7.0.so" > /usr/local/etc/php/conf.d/00_docker-php-ext-ioncube_loader_lin_7.0.ini


RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini
  
RUN apt-get update && \
      apt-get -y install sudo

RUN apt-get update && \
      apt-get -y install git

RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
RUN echo 'sendmail_path = /usr/bin/mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/php.ini


RUN a2enmod rewrite
RUN a2enmod ssl
RUN chown -R www-data:www-data /var/www
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
