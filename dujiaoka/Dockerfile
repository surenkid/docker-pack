FROM alpine AS fetchRepo
WORKDIR /tmp/go-chatgpt-api
RUN apk add --no-cache git \
    && git clone -b master https://github.com/assimon/dujiaoka.git /tmp/surenkid

FROM webdevops/php-nginx:7.4
COPY --from=fetchRepo /tmp/surenkid/ /app
WORKDIR /app
RUN [ "sh", "-c", "composer install --ignore-platform-reqs" ]
RUN [ "sh", "-c", "chmod -R 777 /app" ]
