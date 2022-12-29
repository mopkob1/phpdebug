FROM php:8.2-fpm-alpine

# Install xdebug
RUN apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS --update linux-headers \
  && pecl install xdebug \
  && docker-php-ext-enable xdebug \
  && apk del .phpize-deps

RUN mkdir -p /home/xdebug

COPY ./docker/php/xdebug-debug.ini /home/xdebug/xdebug-debug.ini
COPY ./docker/php/xdebug-default.ini /home/xdebug/xdebug-default.ini
COPY ./docker/php/xdebug-off.ini /home/xdebug/xdebug-off.ini
COPY ./docker/php/xdebug-profile.ini /home/xdebug/xdebug-profile.ini
COPY ./docker/php/xdebug-trace.ini /home/xdebug/xdebug-trace.ini

ARG XDEBUG_MODES
ARG REMOTE_HOST
ARG REMOTE_PORT
ARG IDE_KEY

ENV MODES=$XDEBUG_MODES
ENV CLIENT_HOST=$REMOTE_HOST
ENV CLIENT_PORT=$REMOTE_PORT
ENV IDEKEY=$IDE_KEY

COPY ./docker/php/fpm-entrypoint.sh /home/fpm-entrypoint
RUN chmod +x /home/fpm-entrypoint

WORKDIR /var/www/html

ENTRYPOINT ["/home/fpm-entrypoint"]

