FROM php:7.4-apache

RUN apt-get update && apt-get install -y --no-install-recommends \
    zlib1g-dev \
    libpng-dev \
    libxslt-dev \
    libpq-dev \
    libmemcached-dev \
    libmagickwand-dev \
    libgmp-dev \
    curl \
    git \
    wget \
    unzip \
    nodejs \
    npm \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libzip-dev \
#    zlibc \
    openssh-server

RUN wget https://getcomposer.org/installer -O - -q \
    | php -- --install-dir=/bin --filename=composer --quiet

RUN pecl install xdebug-3.0.2
RUN docker-php-ext-enable xdebug

RUN docker-php-ext-configure zip
RUN docker-php-ext-configure gd --with-freetype --with-jpeg=/usr/include --enable-gd

# Enable default PHP extensions
RUN docker-php-ext-install mysqli pdo_mysql pcntl bcmath soap intl exif gmp xsl sockets gd zip

RUN echo 'xdebug.mode=debug' >> /usr/local/etc/php/php.ini-development
RUN echo 'xdebug.client_port=9003' >> /usr/local/etc/php/php.ini-development
RUN echo 'xdebug.discover_client_host=true' >> /usr/local/etc/php/php.ini-development

RUN echo 'xdebug.mode=debug' >> /usr/local/etc/php/php.ini-production
RUN echo 'xdebug.client_port=9003' >> /usr/local/etc/php/php.ini-production
RUN echo 'xdebug.discover_client_host=true' >> /usr/local/etc/php/php.ini-production

RUN echo 'xdebug.mode=debug' >> /usr/local/etc/php/conf.d/docker-php-ext-bcmath.ini
RUN echo 'xdebug.client_port=9003' >> /usr/local/etc/php/conf.d/docker-php-ext-bcmath.ini
RUN echo 'xdebug.discover_client_host=true' >> /usr/local/etc/php/conf.d/docker-php-ext-bcmath.ini

#ADD ./php.ini /usr/local/etc/php/php.ini

#RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/php.ini
#RUN echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/php.ini
#RUN echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini

COPY /vhosts/default-vhost.conf /etc/apache2/sites-available/
RUN a2dissite 000-default.conf
RUN a2ensite default-vhost.conf
RUN a2enmod rewrite

WORKDIR /var/www/html
