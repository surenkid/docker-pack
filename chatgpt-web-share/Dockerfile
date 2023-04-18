FROM golang:alpine3.17 AS fetchRepo
WORKDIR /tmp/chatgptwebshare
RUN apk add --no-cache git \
    && git clone https://github.com/moeakwak/chatgpt-web-share.git /tmp/chatgptwebshare

FROM python:3.10-alpine

ARG PIP_CACHE_DIR=/pip_cache
ARG TARGETARCH

COPY --from=fetchRepo /tmp/chatgptwebshare/ /repo/

RUN mkdir -p /app/backend

RUN apk add --update caddy
RUN if [ "${TARGETARCH}" = "arm64" ] ; then \
        apk add --no-cache gcc musl-dev libffi-dev \
    ; fi

COPY /repo/backend/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

COPY /repo/Caddyfile /app/Caddyfile
COPY /repo/backend /app/backend
COPY /repo/frontend/dist /app/dist

WORKDIR /app

EXPOSE 80

COPY /repo/startup.sh /app/startup.sh
RUN chmod +x /app/startup.sh; mkdir /data
CMD ["/app/startup.sh"]
