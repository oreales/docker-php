ARG PHP_TAG
FROM ghcr.io/cvarela-usesi/php-fpm:${PHP_TAG}

RUN apt update -y \
    && apt upgrade -y

# Install and enable SourceGuardian and ionCube loaders
RUN install-php-extensions sourceguardian ioncube_loader
