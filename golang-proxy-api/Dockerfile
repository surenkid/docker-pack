FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/golang-proxy-api@e56f82a27dab24155a2265f91194fcff9f86378c

FROM alpine:3.17.3
COPY --from=build /go/bin/golang-proxy-api /golang-proxy-api
CMD ["/golang-proxy-api"]
