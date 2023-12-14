# PHP 8.3.0
FROM php:8.3.0-apache

# Habilitando mod-rewrite
RUN a2enmod rewrite

# Habilitando php.ini dev cmd php --ini
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Configurando SSL
RUN a2enmod ssl
RUN mkdir -p /etc/apache2/ssl
COPY ./docker/php-apache/apache/ssl/*.pem /etc/apache2/ssl/
COPY ./docker/php-apache/apache/000-default.conf /etc/apache2/sites-available/000-default.conf
# COPY ./docker/php-apache/apache/directory.conf /etc/apache2/sites-enabled/directory.conf

# Composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Update e instalação de zip unzip vim
RUN apt-get update && apt-get install -y zip unzip vim

# PHP pdo pdo_mysql gd exif
RUN docker-php-ext-install pdo pdo_mysql
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev && \
docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && \
docker-php-ext-install gd
RUN docker-php-ext-install exif

# Publicando portas
EXPOSE 80
EXPOSE 443

# Restartando serviços
RUN service apache2 restart

# Pasta de trabalho
WORKDIR /var/www/html