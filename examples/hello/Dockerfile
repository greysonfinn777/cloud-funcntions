FROM gcr.io/gae-runtimes/php74:php74_20210216_7_4_15_RC00@sha256:b7d4aef9e57c17152c2ea9a3fbd04c23687fc93c7254608bdf6e19f14a7c10f3

WORKDIR /srv/

# NOTE: The entrypoint "/start", which starts up NGINX and PHP-FPM,
# is configured by creating a `.googleconfig/app_start.json` file with the
# contents:
#
#     {"entrypointContents": "CUSTOM_ENTRYPOINT"}
#
# We configure it to use the `router.php` file included in this package.
RUN mkdir .googleconfig && \
    echo '{"entrypointContents": "serve vendor/google/cloud-functions-framework/router.php"}' > .googleconfig/app_start.json

# Copy over composer files and run "composer install"
COPY composer.* ./
COPY --from=composer:1 /usr/bin/composer /usr/local/bin
RUN composer install --no-dev

# Copy over all application files
COPY . .

# Set a runtime name (required by the base image)
ENV GAE_RUNTIME php74
