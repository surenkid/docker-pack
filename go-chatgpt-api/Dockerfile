FROM alpine AS fetchRepo
WORKDIR /tmp/go-chatgpt-api
RUN apk add --no-cache git \
    && git clone -b legacy https://github.com/linweiyuan/go-chatgpt-api.git /tmp/go-chatgpt-api

FROM golang:alpine AS builder
WORKDIR /app
COPY --from=fetchRepo /tmp/go-chatgpt-api/ .
RUN go build -ldflags="-w -s" -o go-chatgpt-api main.go

FROM alpine
WORKDIR /app
COPY --from=builder /app/go-chatgpt-api .
RUN apk add --no-cache tzdata
ENV TZ=Asia/Shanghai
EXPOSE 8080
CMD ["/app/go-chatgpt-api"]
