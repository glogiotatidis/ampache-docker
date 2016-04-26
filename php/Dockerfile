FROM php:apache

RUN echo "deb http://www.deb-multimedia.org jessie main non-free" > /etc/apt/sources.list.d/multimedia.list
RUN apt-get update && apt-get install -y -q --force-yes \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libmysqlclient-dev \
        git \
        ffmpeg

RUN a2enmod rewrite

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql gettext

RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer

WORKDIR /var/www
RUN curl -s https://codeload.github.com/ampache/ampache/tar.gz/master | tar zxC html --strip-components=1

WORKDIR /var/www/html
RUN composer install --prefer-source --no-interaction
RUN chown www-data.www-data -R .

