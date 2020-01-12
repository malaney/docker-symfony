# See https://github.com/docker-library/php/blob/master/7.1/fpm/Dockerfile
FROM php:7.1-fpm
ARG TIMEZONE

MAINTAINER Maxence POUTORD <maxence.poutord@gmail.com>

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    telnet \
    unzip \
    ssh \
    vim \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev
    node \
    uglifyjs

# RUN systemctl enable ssh 
# RUN service ssh status
# RUN mkdir /var/run/sshd
# CMD ['/usr/sbin/sshd', '-D']

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
&& composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql mysqli


# install xdebug
# RUN pecl install xdebug \
# && docker-php-ext-enable xdebug \
# && echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# install apc
# Install APCu and APC backward compatibility
RUN pecl install apcu \
    && pecl install apcu_bc-1.0.5 \
    && docker-php-ext-enable apcu --ini-name 10-apcu.ini \
    && docker-php-ext-enable apc  --ini-name 20-apc.ini

# Install GD
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN echo 'alias sf="php app/console"' >> ~/.bashrc \
&& echo 'alias sf3="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony

RUN service ssh start 
