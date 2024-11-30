#syntax=docker/dockerfile:1.4

ARG PHP_VERSION=8.3
FROM ghcr.io/shopware/docker-base:$PHP_VERSION-nginx as base-image
FROM ghcr.io/friendsofshopware/shopware-cli:latest-php-$PHP_VERSION as shopware-cli

FROM shopware-cli as build

ADD . /src
WORKDIR /src

ENV SHOPWARE_PACKAGES_TOKEN=${SHOPWARE_PACKAGES_TOKEN}
# --mount=type=secret,id=composer_auth,env=COMPOSER_AUTH \
RUN \
    --mount=type=cache,target=/root/.composer \
    --mount=type=cache,target=/root/.npm \
    APP_ENV=ci /usr/local/bin/entrypoint.sh shopware-cli project ci /src

FROM base-image

COPY --from=build --chown=82 --link /src /var/www/html
COPY .php/php.ini /usr/local/etc/php/conf.d/

USER root
RUN install-php-extensions grpc
USER www-data
