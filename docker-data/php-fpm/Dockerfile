FROM php:7.4-fpm

WORKDIR "/var/www/public"
ENV ACCEPT_EULA=Y

RUN apt-get update && apt-get install -y \
    wget \
    curl \
    apt-utils \
    gnupg \
    software-properties-common \
    apt-transport-https \
    libxml2-dev \
    unixodbc-dev \
    build-essential

RUN wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update
RUN apt-get install --no-install-recommends -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        libicu-dev \
        libzip-dev
RUN apt-get install --no-install-recommends -y msodbcsql17 mssql-tools


COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/install-php-extensions

RUN chmod uga+x /usr/bin/install-php-extensions
RUN sync
RUN pecl install pdo_sqlsrv-5.10.1 sqlsrv-5.10.1 zip mcrypt
RUN docker-php-ext-install \
        gd \
        iconv \
        sockets \
        pdo \
    && docker-php-ext-enable \
        sqlsrv \
        pdo_sqlsrv \
        zip \
        mcrypt