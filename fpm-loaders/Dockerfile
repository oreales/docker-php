ARG PHP_TAG
FROM ghcr.io/cvarela-usesi/php-fpm:${PHP_TAG}

RUN apt update -y \
    && apt upgrade -y

# Install and enable SourceGuardian and ionCube loaders
RUN OS_ARCH=$(uname -m) \
    && PHP_VERSION=$(php -v | head -n1 | cut -d' ' -f2 | cut -d. -f1-2) \
    && mkdir -p /tmp/sourceguardian \
    && cd /tmp/sourceguardian \
    && if [[ ${OS_ARCH} != "armv7l" ]]; then \
        curl -kOs https://www.sourceguardian.com/loaders/download/loaders.linux-x86_64.tar.gz \
        && tar xzf loaders.linux-x86_64.tar.gz; \
    else \
        curl -kOs https://www.sourceguardian.com/loaders/download/loaders.linux-armhf.tar.gz \
        && tar xzf loaders.linux-armhf.tar.gz; \
    fi \
    && SOURCEGUARDIAN_LOADER_PATH=ixed.${PHP_VERSION}.lin \
    && if [[ -f ${SOURCEGUARDIAN_LOADER_PATH} ]]; then \
        cp ${SOURCEGUARDIAN_LOADER_PATH} "$(php -i | grep '^extension_dir =' | cut -d' ' -f3)/sourceguardian.so" \
            && echo "extension=sourceguardian.so" > /usr/local/etc/php/conf.d/15-sourceguardian.ini; \
    else \
        >&2 printf "\033[33mWARNING\033[0m: Source Gaurdian loaders for PHP_VERSION %s could not be found at %s\n" \
            "${PHP_VERSION}" "${SOURCEGUARDIAN_LOADER_PATH}"; \
    fi \
    && rm -rf /tmp/sourceguardian

# Install and enable IonCube loader
RUN OS_ARCH=$(uname -m) \
    && PHP_VERSION=$(php -v | head -n1 | cut -d' ' -f2 | cut -d. -f1-2) \
    && mkdir -p /tmp/ioncube \
    && cd /tmp/ioncube \
    && if [[ ${OS_ARCH} != "armv7l" ]]; then \
        curl -kOs https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
        && tar xzf ioncube_loaders_lin_x86-64.tar.gz; \
    else \
        curl -kOs https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_armv7l.tar.gz \
        && tar xzf ioncube_loaders_lin_armv7l.tar.gz; \
    fi \
    && IONCUBE_LOADER_PATH=ioncube/ioncube_loader_lin_${PHP_VERSION}.so \
    && if [[ -f ${IONCUBE_LOADER_PATH} ]]; then \
        cp ${IONCUBE_LOADER_PATH} "$(php -i | grep '^extension_dir =' | cut -d' ' -f3)/ioncube_loader.so" \
            && echo "zend_extension=ioncube_loader.so" > /usr/local/etc/php/conf.d/01-ioncube-loader.ini; \
    else \
        >&2 printf "\033[33mWARNING\033[0m: IonCube loaders for PHP_VERSION %s could not be found at %s\n" \
            "${PHP_VERSION}" "${IONCUBE_LOADER_PATH}"; \
    fi \
    && rm -rf /tmp/ioncube