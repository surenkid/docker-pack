FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/golang-proxy-api@413de4b522ca6d15b934ad054467940d2f5605e5

FROM alpine:3.17.3
COPY --from=build /go/bin/golang-proxy-api /golang-proxy-api
CMD ["/golang-proxy-api"]
