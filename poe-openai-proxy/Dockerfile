FROM golang:1.20 AS golang-builder
RUN apt-get update && \
    apt-get install -y git
WORKDIR /app
RUN git clone https://github.com/juzeon/poe-openai-proxy.git .
RUN go build

FROM python:3.9
COPY --from=golang-builder /app/poe-openai-proxy /app/
COPY --from=golang-builder /app/external /app/external
COPY --from=golang-builder /app/config.example.toml /app/config.toml
RUN pip install --no-cache-dir -r /app/external/requirements.txt
EXPOSE 3700
CMD ["/bin/sh", "-c", "python /app/external/api.py & /app/poe-openai-proxy"]
