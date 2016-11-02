FROM codeception/codeception

MAINTAINER Kentaro Ohkouchi <nanasess@fsm.ne.jp>

### see also https://hub.docker.com/r/codeception/codeception/~/dockerfile/

RUN apt-get update && apt-get install --no-install-recommends -y \
        git curl wget sudo libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libmcrypt-dev libxml2-dev libpq-dev libpq5 postgresql-client mysql-client \
        && docker-php-ext-configure \
        gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) \
        mbstring zip gd xml pdo pdo_pgsql pdo_mysql soap mcrypt \
        && rm -r /var/lib/apt/lists/*

RUN echo "date.timezone = Asia/Tokyo" >> /usr/local/etc/php/php.ini

## Clone EC-CUBE3

ENV ECCUBE_PATH /var/www

ARG ECCUBE_REPOS
ARG ECCUBE_BRANCH
ARG DBTYPE

RUN cd /var && \
    git clone --depth=50 -b ${ECCUBE_BRANCH} ${ECCUBE_REPOS} ${ECCUBE_PATH}

WORKDIR ${ECCUBE_PATH}
RUN composer install
ENV DBSERVER db
ENV DBPASS password
ENV AUTH_MAGIC XjosAXOzO1B3mE0egwQA

## create config yaml files
RUN php /var/www/eccube_install.php ${DBTYPE} none --skip-createdb --skip-initdb --verbose
WORKDIR /project

## create dummy directories
RUN mkdir /project/html
RUN mkdir /project/app

COPY codecept /repo/codecept
RUN chmod +x /repo/codecept