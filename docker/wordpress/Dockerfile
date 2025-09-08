FROM wordpress:php8.2-apache

# Install system deps, composer, and Xdebug
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    zip \
    libzip-dev \
    && docker-php-ext-install zip

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install Xdebug (for debugging)
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Configure Xdebug for simple remote debugging (adjust as needed)
RUN { \
    echo "xdebug.mode=develop,debug"; \
    echo "xdebug.start_with_request=yes"; \
    echo "xdebug.client_host=host.docker.internal"; \
    echo "xdebug.client_port=9003"; \
    echo "xdebug.log=/tmp/xdebug.log"; \
} > /usr/local/etc/php/conf.d/xdebug.ini

# Set recommended permissions (optional)
RUN chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

# Expose port already defined by base image (80)
