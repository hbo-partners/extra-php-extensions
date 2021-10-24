ARG PHP_VERSION
FROM bref/build-php-$PHP_VERSION:latest AS ext

RUN pecl install --force psr-1.1.0
RUN cp `php-config --extension-dir`/psr.so /tmp/psr.so
RUN echo 'extension=/opt/bref-extra/psr.so' > /tmp/ext.ini

# Build the final image with just the files we need
FROM scratch

# Copy things we installed to the final image
COPY --from=ext /tmp/psr.so /opt/bref-extra/psr.so
COPY --from=ext /tmp/ext.ini /opt/bref/etc/php/conf.d/ext-psr.ini
