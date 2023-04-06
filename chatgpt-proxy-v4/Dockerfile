FROM golang AS build
WORKDIR /go/src/app
RUN go get -d github.com/acheong08/ChatGPT-Proxy-V4@latest
RUN go build -o ChatGPT-Proxy-V4 github.com/acheong08/ChatGPT-Proxy-V4@latest

FROM scratch
COPY --from=build /go/src/app/ChatGPT-Proxy-V4 /ChatGPT-Proxy-V4
CMD ["ChatGPT-Proxy-V4"]
