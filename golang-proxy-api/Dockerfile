FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/golang-proxy-api@ccfcbef4d95e0a4c0b220ce12d6785d1bd3e757b

FROM alpine:3.17.3
COPY --from=build /go/bin/golang-proxy-api /golang-proxy-api
CMD ["/golang-proxy-api"]
