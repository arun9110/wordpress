# docker/wordpress/Dockerfile
FROM wordpress:php8.2-fpm

# Install system deps, composer, and Xdebug
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    zip \
    libzip-dev \
    && docker-php-ext-install zip \
    && rm -rf /var/lib/apt/lists/*

# Install Composer (copy from official composer image)
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install Xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Configure Xdebug (adjust client_host for your environment)
RUN { \
    echo "xdebug.mode=develop,debug"; \
    echo "xdebug.start_with_request=yes"; \
    echo "xdebug.client_host=host.docker.internal"; \
    echo "xdebug.client_port=9003"; \
    echo "xdebug.log=/tmp/xdebug.log"; \
} > /usr/local/etc/php/conf.d/xdebug.ini

# Recommended permission fix for mounted volumes
RUN chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

# Expose FPM port for internal docker network
EXPOSE 9000
