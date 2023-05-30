ARG APP_NAME=openai-api-proxy-key-pool

FROM alpine AS fetchRepo
ARG APP_NAME
RUN apk add --no-cache git \
    && git clone https://github.com/surenkid/${APP_NAME}.git /tmp/${APP_NAME}

FROM golang:alpine3.17 AS build
ARG APP_NAME
WORKDIR /app
COPY --from=fetchRepo /tmp/${APP_NAME}/ .
RUN go build -ldflags="-w -s" -o ${APP_NAME} main.go

FROM alpine:3.17.3
ARG APP_NAME
WORKDIR /app
COPY --from=build /app/${APP_NAME} .
RUN apk add --no-cache ca-certificates tzdata \
    && update-ca-certificates
ENV TZ=Asia/Shanghai
EXPOSE 8124
CMD ["/app/${APP_NAME}"]
