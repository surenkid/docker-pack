FROM alpine AS fetchRepo
RUN apk add --no-cache git \
    && git clone -b legacy https://github.com/linweiyuan/go-chatgpt-api.git /tmp/surenkid

FROM golang:alpine AS builder
WORKDIR /app
COPY --from=fetchRepo /tmp/surenkid/ .
RUN go build -ldflags="-w -s" -o go-chatgpt-api main.go

FROM alpine
WORKDIR /app
COPY --from=builder /app/go-chatgpt-api .
RUN apk add --no-cache tzdata
ENV TZ=Asia/Shanghai
EXPOSE 8080
CMD ["/app/go-chatgpt-api"]
