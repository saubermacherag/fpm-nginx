# FPM/Nginx - [Fork Origin](https://github.com/bkuhl/fpm-nginx)

This is a fork repo which builds a 2in1 Docker Image containing php-fpm and a preconfigured nginx
for serving CakePHP applications.

## Example Dockerfile (building a react/cakephp app)

```
# This part is intended to build react and php parts of the application
FROM node:8.16.0-jessie-slim as builder
MAINTAINER devops@wastebox.biz

WORKDIR /app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
COPY . .
RUN npm ci
RUN npm run build

FROM <repo-name>:<version-name>

WORKDIR /var/www/html

# copy built app to nginx
COPY --chown=www-data:www-data --from=builder /app /var/www/html

USER www-data

RUN ls -la && composer install --verbose --no-interaction --optimize-autoloader --no-dev --prefer-dist && \
  rm -rf /home/www-data/.composer/cache

USER root
```
