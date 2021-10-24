# extra-php-extensions


    FROM bref/php-74-fpm-dev AS dev

        COPY --from=bref/extra-mongodb-php-74:0.11.15 /opt/bref-extra /opt/bref-extra
        COPY --from=bref/extra-mongodb-php-74:0.11.15 /opt/bref/ /opt/bref

        COPY --from=psr:local /opt/bref-extra /opt/bref-extra
        COPY --from=psr:local /opt/bref/ /opt/bref

        COPY --from=phalcon:local /opt/bref-extra /opt/bref-extra
        COPY --from=phalcon:local /opt/bref/ /opt/bref

        COPY ./src /var/task

        # bind mount Composer /user/bin
        COPY --from=composer /usr/bin/composer /usr/bin/composer
        RUN composer install --no-dev --no-interaction --optimize-autoloader


    FROM bref/php-74-fpm AS prod

        COPY --from=bref/extra-mongodb-php-74:0.11.15 /opt/bref-extra /opt/bref-extra
        COPY --from=bref/extra-mongodb-php-74:0.11.15 /opt/bref/ /opt/bref
        COPY ./src /var/task

        # Configure the handler file (the entrypoint that receives all HTTP requests)
        CMD ["public/index.php"]
