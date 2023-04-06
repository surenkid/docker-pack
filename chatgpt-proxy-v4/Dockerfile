FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/acheong08/ChatGPT-Proxy-V4@latest

FROM alpine:3.17.3
COPY --from=build /go/bin/ChatGPT-Proxy-V4 /ChatGPT-Proxy-V4
CMD ["/ChatGPT-Proxy-V4"]
