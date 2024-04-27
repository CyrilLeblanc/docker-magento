FROM php:8.2-apache-buster

ENV UID=1000 \
    GID=1000

# Install required packages
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    libxml2-dev \
    libxslt-dev \
    libzip-dev \
    git \
    msmtp \
    mailutils && \
    apt-get clean  && \
    rm -rf /var/lib/apt/lists/*

# Configure GD extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --prefix=/usr

# Install missing php extensions
RUN docker-php-ext-install \
    bcmath \
    gd \
    intl \
    pdo_mysql \
    soap \
    sockets \
    xsl \
    zip \
    opcache

# Configure PHP
COPY magento.ini /usr/local/etc/php/conf.d/magento.ini

# Install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Install composer
COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

# Configure msmtprc
COPY msmtprc /etc/msmtprc

# Enable apache modules
RUN a2enmod rewrite

# Change www-data UID and GID
RUN usermod -u $UID www-data && groupmod -g $GID www-data

# Fix permissions
RUN chown -R www-data:www-data /var/www

WORKDIR /var/www/html

USER www-data

CMD ["apache2-foreground", "-D", "FOREGROUND"]
