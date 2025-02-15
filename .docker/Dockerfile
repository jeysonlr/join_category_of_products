FROM  php:7.4.7-apache-buster

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public

RUN apt-get update && apt-get install -y \
    git \
    zlib1g-dev \
    unzip \
    unixodbc \
    unixodbc-dev \
    freetds-dev \
    freetds-bin \
    tdsodbc \
    libxml2-dev \
    libicu-dev \
    locales-all \
    task-brazilian-portuguese \
    ghostscript \
    libaio-dev \
    unixodbc \
    tdsodbc \
    libzip-dev \
    && docker-php-ext-install zip \
    && docker-php-ext-install sockets

# Set timezone
RUN rm /etc/localtime &&\
    ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime &&\
    "date"

# install pecl
RUN curl -O http://pear.php.net/go-pear.phar \
    ; /usr/local/bin/php -d detect_unicode=0 go-pear.phar

# xDEBUG 
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo 'xdebug.remote_enable=on' >> /usr/local/etc/php/conf.d/xdebug.ini 
RUN echo 'xdebug.remote_host=10.0.75.1' >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/conf.d/xdebug.ini
RUN a2enmod rewrite 

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN a2enmod rewrite \
    && a2enmod headers

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Postgres Driver
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libpng-dev \
    zip unzip \
    git \
    iputils-ping && \
    docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql && \
    docker-php-ext-install pdo pdo_pgsql pgsql && \
    docker-php-ext-install gd
