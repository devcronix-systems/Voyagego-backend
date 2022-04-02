FROM php:apache
#php:8.1.x with apache

# Set workdir
WORKDIR /var/www/voyagego

# Args
ARG user
ARG uid

# Install dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
        zip \
        unzip \
        curl \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        libicu-dev \
        zlib1g-dev \
        openssl \
        #extras
        g++ \
        cron \
        wget \
        vim \
        git \
        jpegoptim \
        pngquant \
        optipng \
        gifsicle \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libwebp-dev \
        libpng-dev \
        libssl-dev \
        libpq-dev \
        libwebp-dev \
        libmcrypt-dev 

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install and configure extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
docker-php-ext-install pdo pdo_pgsql opcache soap zip exif intl gd && \
pecl install xdebug && docker-php-ext-enable xdebug opcache

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer


# Create user to run composer
RUN mkdir -p /home/$user/.composer && \
chown -R $user:$user /home/$user && \
chown -R $user:$user /home/$user/.composer

#apache enable mods, make certs and logs
RUN a2enmod rewrite proxy_fcgi proxy deflate ssl && \
mkdir -p /etc/apache2/ssl && \
mkdir -p /var/log/php && \
touch /var/log/php/errors.log && chmod 777 /var/log/php/errors.log && \
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/cert-key.pem -out /etc/apache2/ssl/cert.pem -subj "/C=PL/ST=Warsaw/L=Warsaw/O=Security/OU=Development/CN=example.com" && \
chown -R $user:$user /etc/apache2/ssl/
#RUN php artisan config:cache && \
#    php artisan route:cache && \
# Change current user to www
USER $user
