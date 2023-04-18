FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install -ldflags="-w -s" github.com/linweiyuan/go-chatgpt-api@latest

FROM alpine:3.17.3
COPY --from=build /go/bin/go-chatgpt-api /go-chatgpt-api
CMD ["/go-chatgpt-api"]
