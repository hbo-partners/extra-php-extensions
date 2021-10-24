ARG PHP_VERSION
FROM bref/build-php-$PHP_VERSION:latest AS ext

RUN pecl install --force phalcon-4.1.2
RUN cp `php-config --extension-dir`/phalcon.so /tmp/phalcon.so
RUN echo 'extension=/opt/bref-extra/phalcon.so' > /tmp/ext.ini

# Build the final image with just the files we need
FROM scratch

# Copy things we installed to the final image
COPY --from=ext /tmp/phalcon.so /opt/bref-extra/phalcon.so
COPY --from=ext /tmp/ext.ini /opt/bref/etc/php/conf.d/ext-phalcon.ini
