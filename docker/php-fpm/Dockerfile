FROM php:8.1-fpm

#update packages list & upgrade them
RUN apt-get update

#install PHP intl
RUN apt-get install -y zlib1g-dev libicu-dev g++
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl

#install PHP zip
RUN apt-get install -y libzip-dev zip \
    && docker-php-ext-install zip

#install php mysql extension
RUN docker-php-ext-install pdo pdo_mysql mysqli

#install redis extension
RUN pecl install redis \
	&& docker-php-ext-enable redis

#set php memory_limit
RUN echo 'memory_limit = 512M' > /usr/local/etc/php/conf.d/docker-php-memlimit.ini;

#install git & wget & zip
RUN apt-get update && apt-get install -y git
RUN apt-get install wget
RUN apt-get install -y zip

#install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '$(wget -q -O - https://composer.github.io/installer.sig)') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

#install yarn
RUN apt-get install -y gnupg
RUN curl https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn

#update node to the latest stable version
RUN apt-get install -y npm && npm cache clean -f \
 && npm install -g n \
 #need to open another termianl for this
 && n stable

WORKDIR /app

RUN chown -R 1000:1000 /app
RUN mkdir /.composer \
    && chown -R 1000:1000 /.composer
RUN mkdir /.cache \
    && chown -R 1000:1000 /.cache
RUN mkdir /.yarn \
    && chown -R 1000:1000 /.yarn
RUN mkdir -p /.config/psysh \
    && chown -R 1000:1000 /.config
USER 1000
