FROM alpine AS fetchRepo
RUN apk add --no-cache git \
    && git clone https://github.com/acheong08/ChatGPT-to-API.git /tmp/surenkid

FROM golang:1.20.3-alpine as builder
ENV CGO_ENABLED=0
WORKDIR /app
COPY --from=fetchRepo /tmp/surenkid/go.mod /tmp/surenkid/go.sum ./
RUN go mod download
COPY --from=fetchRepo /tmp/surenkid/ .
RUN go build -ldflags="-w -s" -o /app/ChatGPT-To-API .

FROM scratch
WORKDIR /app
COPY --from=builder /app/ChatGPT-To-API /app/ChatGPT-To-API
EXPOSE 8080
CMD [ "./ChatGPT-To-API" ]
