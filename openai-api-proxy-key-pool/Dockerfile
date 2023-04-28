FROM golang:alpine3.17 AS build
WORKDIR /go/src/app
RUN apk add --no-cache git \
    && go install github.com/surenkid/openai-api-proxy-key-pool@latest

FROM alpine:3.17.3
COPY --from=build /go/bin/openai-api-proxy-key-pool /openai-api-proxy-key-pool
CMD ["/openai-api-proxy-key-pool"]
