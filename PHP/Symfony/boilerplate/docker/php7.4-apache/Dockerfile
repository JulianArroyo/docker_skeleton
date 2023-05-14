FROM php:7.4.33-apache

ARG UID

# Create user and some useful stuff
RUN adduser -u ${UID} --disabled-password --gecos "" appuser
RUN mkdir /home/appuser/.ssh
RUN chown -R appuser:appuser /home/appuser/
RUN echo "StrictHostKeyChecking no" >> /home/appuser/.ssh/config
RUN echo "export COLUMNS=300" >> /home/appuser/.bashrc
# RUN echo "alias sf=/var/www/boilerplate/bin/console" >> /home/appuser/.bashrc

COPY ./php.ini /usr/local/etc/php/php.ini

# Install packages and PHP extensions
RUN apt-get update \
    && apt upgrade -y \
    # common libraries and extensions
    && apt-get install -y vim ca-certificates apt-transport-https software-properties-common lsb-release \
    && apt-get install -y curl bzip2 zip unzip xz-utils gnupg git acl openssl openssh-client wget \
    && apt-get install -y libpng-dev zlib1g-dev libzip-dev libbz2-dev libgmp-dev libxml2-dev libtidy-dev libxslt1-dev libicu-dev \
    && docker-php-ext-install -j "$(nproc)" bcmath bz2 calendar gettext gmp intl soap tidy xsl zip pdo gd \
    # for MySQL
    && docker-php-ext-install -j "$(nproc)" pdo_mysql \
    # for Postgres
    && apt-get install -y libpq-dev \
    && docker-php-ext-install -j "$(nproc)" pdo_pgsql \
    # for RabbitMQ and XDEBUG
    && docker-php-ext-install -j "$(nproc)" sockets \
    && apt-get install -y librabbitmq-dev libssh-dev \
    # https://chrisshennan.com/blog/fixing-pecl-xdebug-requires-php-version-8-installed-version-7-4
    # https://stackoverflow.com/questions/75029280/install-xdebug-for-php-7-4-on-m1-apple
    # https://xdebug.org/docs/compat
    && pecl install xdebug-3.1.6 amqp-1.11.0 \
    # enable Docker extensions
    && docker-php-ext-enable --ini-name 05-opcache.ini opcache xdebug amqp bcmath bz2 calendar gettext gmp intl soap tidy xsl zip pdo_mysql pdo_pgsql gd \
    && echo "Done."

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install PHP-CS-FIXER
RUN wget https://cs.symfony.com/download/php-cs-fixer-v3.phar -O php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

## Install Symfony binary
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt-get update && apt install -y symfony-cli

# Config XDEBUG
#COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN a2enmod rewrite

COPY ./boilerplate.conf /etc/apache2/sites-available/boilerplate.conf

RUN a2ensite boilerplate
RUN a2dissite 000-default

USER appuser
WORKDIR /var/www/boilerplate/
