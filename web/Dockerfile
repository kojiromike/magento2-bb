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

# Install composer
RUN php -r "readfile('https://getcomposer.org/installer');" \
  | php -- --filename=composer --install-dir=/usr/local/bin

# Setup Apache dirs and configuration.

# Create a "default" user. Any volume mounted files should be owned by this
# user, so web source files can be shared via a volume mount. Apache will be run
# as this user by `apache_safe_start_perms` to have the necessary permissions
# on any volume mounted files and directories.
RUN adduser --system --uid=1000 default

# Magento will be installed at /var/www/html
ENV MAGENTO_ROOT_DIR /var/www/html
RUN rm -rf $MAGENTO_ROOT_DIR \
 && mkdir -p $MAGENTO_ROOT_DIR \
 && chown -R default $MAGENTO_ROOT_DIR

WORKDIR $MAGENTO_ROOT_DIR
VOLUME $MAGENTO_ROOT_DIR

# Add apache configuration and enable mod_rewrite
RUN a2enmod rewrite \
 && a2ensite 000-default

COPY bin/* /usr/local/bin/

# Add PHP configuration
COPY config/magento.php.ini /usr/local/etc/php/conf.d/magento.php.ini

# `apache_safe_start_perms` will start apache as the "default" user.
CMD ["apache_safe_start_perms", "-DFOREGROUND"]
