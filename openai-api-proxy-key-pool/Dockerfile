ARG APP_NAME=openai-api-proxy-key-pool

FROM golang:alpine3.17 AS build
ARG APP_NAME
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/${APP_NAME}@latest

FROM alpine:3.17.3
ARG APP_NAME
COPY --from=build /go/bin/${APP_NAME}/${APP_NAME}
CMD ["/${APP_NAME}"]
