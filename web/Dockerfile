FROM php:5.6-apache
MAINTAINER Scott van Brug <svanbrug@ebay.com>

# Install requirements for Magento 2, composer, and some helpful tools.
RUN apt-get update \
 && apt-get install -qqy apt-utils \
                         g++ \
                         git \
                         libfreetype6-dev \
                         libicu-dev \
                         libjpeg62-turbo-dev \
                         libmcrypt-dev \
                         libpng12-dev \
                         libxslt-dev \
                         mysql-client \
                         re2c \
                         wget \
 && docker-php-ext-configure gd \
                             --with-freetype-dir=/usr/include/ \
                             --with-jpeg-dir=/usr/include/ \
 && docker-php-ext-install bcmath \
                           gd \
                           intl \
                           mbstring \
                           mcrypt \
                           mysql \
                           pdo_mysql \
                           pcntl \
                           xsl \
                           zip \
 && apt-get autoremove -qqy \
 && until rm -rf /var/lib/apt/lists; do sleep 1; done
RUN pecl install -o -f xdebug \
 && cd /usr/local/etc/php/conf.d \
 && printf 'zend_extension=%s\n' /usr/local/lib/php/extensions/no-debug-non-zts-*/xdebug.so >> xdebug.ini \
 && echo 'xdebug.remote_enable=On' >> xdebug.ini \
 && echo 'xdebug.remote_connect_back=On' >> xdebug.ini

# Install composer
RUN php -r "readfile('https://getcomposer.org/installer');" \
  | php -- --filename=composer --install-dir=/usr/local/bin

# Setup Apache dirs and configuration.

ENV APACHE_RUN_USER www-data
ENV MAGENTO_ROOT_DIR /var/www/html

# Create the apache user with uid 1000 so permissions
# and ownership will be similar across containers
RUN userdel $APACHE_RUN_USER \
 && useradd -U \
            -d $MAGENTO_ROOT_DIR \
            -r \
            -s /bin/false \
            -u 1000 \
            $APACHE_RUN_USER

# Magento will be installed at /var/www/html
RUN rm -rf $MAGENTO_ROOT_DIR \
 && mkdir -p $MAGENTO_ROOT_DIR \
 && chown -R $APACHE_RUN_USER $MAGENTO_ROOT_DIR

WORKDIR $MAGENTO_ROOT_DIR
VOLUME $MAGENTO_ROOT_DIR

# Add apache configuration and enable mod_rewrite
RUN a2enmod rewrite \
 && a2ensite 000-default

COPY bin/* /usr/local/bin/

# Add PHP configuration
COPY config/magento.php.ini /usr/local/etc/php/conf.d/magento.php.ini

# `apache_safe_start_perms` will start apache as the "default" user.
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]
EXPOSE 9000
