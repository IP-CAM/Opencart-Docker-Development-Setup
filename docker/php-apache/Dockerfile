FROM php:8.0-apache

# Install dependencies
RUN apt-get update && apt-get install -y \
    default-mysql-client \
    nano \
    iputils-ping \
    libxml2-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libmcrypt-dev \
    libzip-dev \
    zip \
    unzip \
    curl


# Cleanup
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
	zip \
	pdo_mysql \
    mysqli \
    xml \
    fileinfo \
    soap \
    && docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype \
	&& docker-php-ext-install -j$(nproc) gd


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# Install Xdebug
ENV XDEBUG_PORT 9001
ENV XDEBUG_IDEKEY docker
RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY xdebug.ini "$PHP_INI_DIR/conf.d/"


# Change userid for www-data user
RUN usermod -u 1000 www-data


# Site host settings
ADD vhost.conf /etc/apache2/sites-enabled/vhost.conf


# Enable url rewrite module
RUN a2enmod rewrite
RUN service apache2 restart
