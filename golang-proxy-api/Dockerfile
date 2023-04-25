FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/golang-proxy-api@72245995c94ab1d4f5a874559ff176b680ce6d1d

FROM alpine:3.17.3
COPY --from=build /go/bin/golang-proxy-api /golang-proxy-api
CMD ["/golang-proxy-api"]
