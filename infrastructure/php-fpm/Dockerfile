FROM tiendang/madolche-php-fpm:latest-v7.3

LABEL maintainer="TienDang <tiendang.memo@gmail.com>"

ARG TIMEZONE

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"

RUN apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends

###########################################################################
# xDebug:
###########################################################################
RUN pecl install xdebug  && \
      docker-php-ext-enable xdebug

# Copy xdebug configuration for remote debugging
COPY ./conf.d/xdebug.ini /usr/local/etc/php/conf.d/php-ext-xdebug.ini

# Install Opcache extension
RUN docker-php-ext-install opcache

###########################################################################
# Human Language and Character Encoding Support:
###########################################################################

ARG INSTALL_INTL=true

RUN if [ ${INSTALL_INTL} = true ]; then \
    # Install intl and requirements
    apt-get install -yqq zlib1g-dev libicu-dev g++ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl \
;fi

###########################################################################
# ImageMagick:
###########################################################################

#USER root
#
#RUN apt-get install -yqq libmagickwand-dev imagemagick && \
#    pecl install imagick && \
#        docker-php-ext-enable imagick

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure non-root user.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000