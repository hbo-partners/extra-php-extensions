# extra-php-extensions


    FROM bref/php-74-fpm-dev AS dev

        WORKDIR /var/task/www

        COPY ./src /var/task/www

        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/psr-php-74:latest /opt/bref-extra /opt/bref-extra
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/psr-php-74:latest /opt/bref/ /opt/bref
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/phalcon-php-74:latest /opt/bref-extra /opt/bref-extra
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/phalcon-php-74:latest /opt/bref/ /opt/bref

        # bind mount Composer /user/bin
        COPY --from=composer /usr/bin/composer /usr/bin/composer
        RUN composer install --no-dev --no-interaction --optimize-autoloader


    FROM bref/php-74-fpm AS prod


        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/psr-php-74:latest /opt/bref-extra /opt/bref-extra
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/psr-php-74:latest /opt/bref/ /opt/bref
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/phalcon-php-74:latest /opt/bref-extra /opt/bref-extra
        COPY --from=ghcr.io/hbo-partners/extra-php-extensions/phalcon-php-74:latest /opt/bref/ /opt/bref

        COPY --from=dev /var/task /var/task/www
        COPY --from=dev /var/task/vendor /var/task/vendor

        # Configure the handler file (the entrypoint that receives all HTTP requests)
