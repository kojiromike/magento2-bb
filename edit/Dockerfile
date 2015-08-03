FROM php:5.6-cli
MAINTAINER Michael A. Smith <michael@smith-li.com>

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
                         vim-nox \
                         wget \
 && pecl install -o -f xdebug \
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
 && until rm -rf /var/lib/apt/lists; do sleep 1; done \
 && useradd -m editor
WORKDIR /home/editor
COPY vimrunner /usr/local/bin/vimrunner
ENTRYPOINT ["/usr/local/bin/vimrunner"]

##
# Install Pathogen
ADD https://tpo.pe/pathogen.vim \
    /home/editor/.vim/autoload/pathogen.vim
COPY vimrc /home/editor/.vimrc
RUN mkdir -p .vim/bundle \
 && cd .vim/bundle \
 && git clone https://github.com/tpope/vim-sensible.git \
 && git clone https://github.com/joonty/vdebug.git \
 && chown -R editor:editor /home/editor
EXPOSE 9000
