FROM mileschou/phalcon:7.4-fpm-alpine AS ext

RUN apk --update add --virtual build-dependencies build-base openssl-dev autoconf \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && apk del build-dependencies build-base openssl-dev autoconf \
    && rm -rf /var/cache/apk/*

RUN cp `php-config --extension-dir`/mongodb.so /tmp/mongodb.so
RUN echo 'extension=mongodb' > /tmp/ext.ini

# Build the final image with just the files we need
FROM scratch

# Copy things we installed to the final image
COPY --from=ext /tmp/mongodb.so /opt/phalcon-extra/mongodb.so
COPY --from=ext /tmp/ext.ini /opt/phalcon/etc/php/conf.d/ext-mongo.ini
