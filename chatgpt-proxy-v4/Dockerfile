FROM golang AS build
WORKDIR /go/src/app
RUN go install github.com/acheong08/ChatGPT-Proxy-V4@latest

FROM scratch
COPY --from=build /go/bin/ChatGPT-Proxy-V4 /ChatGPT-Proxy-V4
CMD ["/ChatGPT-Proxy-V4"]
