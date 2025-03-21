FROM php:8.2-fpm as app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    zip \
    unzip \
    libzip-dev

# Install PHP extensions
RUN pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install pdo pdo_mysql pcntl zip


    # Useful PHP extension installer image, copy binary into your container
#  COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install php extensions
 # exit on errors, exit on unset variables, print every command as it is executed
#  RUN set -eux; \
#      install-php-extensions pdo pdo_mysql;

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


FROM app as app_dev
# everything under app_dev will be used to build the app_dev image, as called in the docker-compose.dev.yaml file

# RUN set -eux; \
    # install-php-extensions xdebug;
