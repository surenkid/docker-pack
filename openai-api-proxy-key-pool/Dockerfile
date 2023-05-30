FROM alpine AS fetchRepo
RUN apk add --no-cache git \
    && git clone https://github.com/surenkid/openai-api-proxy-key-pool.git /tmp/surenkid

FROM golang:alpine3.17 AS build
WORKDIR /app
COPY --from=fetchRepo /tmp/surenkid/ .
RUN go build -ldflags="-w -s" -o openai-api-proxy-key-pool main.go

FROM alpine:3.17.3
WORKDIR /app
COPY --from=build /app/openai-api-proxy-key-pool .
RUN apk add --no-cache ca-certificates tzdata \
    && update-ca-certificates
ENV TZ=Asia/Shanghai
EXPOSE 8124
CMD ["/app/openai-api-proxy-key-pool"]
