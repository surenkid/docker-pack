FROM alpine AS fetchRepo
RUN apk add --no-cache git \
    && git clone -b master https://github.com/assimon/dujiaoka.git /tmp/surenkid

FROM webdevops/php-nginx:7.4-alpine
COPY --from=fetchRepo /tmp/surenkid/ /app
WORKDIR /app
RUN [ "sh", "-c", "composer install --ignore-platform-reqs" ]
RUN [ "sh", "-c", "chmod -R 777 /app" ]
