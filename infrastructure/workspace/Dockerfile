ARG PHP_VERSION

FROM tiendang/madolche-workplace:latest-v${PHP_VERSION}

LABEL maintainer="TienDang <tiendang.memo@gmail.com>"

RUN apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends

ARG PHP_VERSION
###########################################################################
# Set Timezone
###########################################################################

#ARG TIMEZONE
#ENV TZ ${TIMEZONE}
#
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

###########################################################################
# ImageMagick:
###########################################################################

#RUN apt-get install -y libmagickwand-dev imagemagick && \
#    pecl install imagick && \
#    echo "extension=imagick.so" >> /etc/php/${PHP_VERSION}/mods-available/imagick.ini && \
#        ln -s /etc/php/${PHP_VERSION}/mods-available/imagick.ini /etc/php/${PHP_VERSION}/cli/conf.d/20-imagick.ini

###########################################################################
# xDebug:
###########################################################################

RUN pecl install xdebug && \
    echo "zend_extension=xdebug.so" >> /etc/php/${PHP_VERSION}/cli/conf.d/20-xdebug.ini

# ADD for REMOTE debugging
COPY ./conf.d/xdebug.ini /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/*

# Set default work directory
WORKDIR /var/www
