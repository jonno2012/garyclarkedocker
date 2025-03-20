FROM php:8.2-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    zip \
    unzip \
    libzip-dev

# Install PHP extensions
RUN pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install pdo pdo_mysql pcntl zip

ENV COMPOSER_ALLOW_SUPERUSER=1

COPY ./app/composer.* ./

# WORKDIR /app - this would change the working directory for the rest of the Dockerfile. 
# The working directory is the directory that will be used for the rest of the commands.
# and which you will be on by default when you run bash.

# Install Composer
RUN cd ~
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer
RUN composer self-update

ENV PATH="/root/.composer/vendor/bin:${PATH}"

RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

COPY ./app/ .

RUN composer dump-autoload --optimize