#syntax=docker/dockerfile:1.4

ARG PHP_VERSION=8.3
FROM ghcr.io/shopware/docker-base:$PHP_VERSION-nginx as base-image

USER root
RUN install-php-extensions grpc protobuf
USER www-data
