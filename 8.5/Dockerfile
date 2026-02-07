FROM php:8.5-apache
LABEL maintainer="Henry Mohn <henry@toasterlint.com>"

# Enable Apache Rewrite Module
RUN a2enmod rewrite

# Install PHP extensions
RUN apt-get update && apt-get -y dist-upgrade
RUN apt-get install -y --no-install-recommends libpng-dev libjpeg-dev zip unzip m4 libzip-dev libicu-dev imagemagick git autoconf pkg-config && rm -rf /var/lib/apt/lists/*
RUN docker-php-ext-configure gd --with-jpeg=/usr && docker-php-ext-install gd
RUN docker-php-ext-install mysqli && docker-php-ext-install exif && docker-php-ext-install zip && docker-php-ext-install intl

RUN apt-get install -y --no-install-recommends libmagickwand-dev

RUN git clone https://github.com/Imagick/imagick.git --depth 1 /tmp/imagick \
    && cd /tmp/imagick \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && docker-php-ext-enable imagick

RUN apt-get purge -y libmagickwand-dev git autoconf pkg-config && apt-get -y autoremove --purge && rm -rf /var/lib/apt/lists/* /tmp/imagick

RUN echo "error_reporting = E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED & ~E_WARNING" > /usr/local/etc/php/php.ini

VOLUME /var/www/html

CMD ["apache2-foreground"]
