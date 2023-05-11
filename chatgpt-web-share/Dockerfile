FROM golang:alpine3.17 AS fetchRepo
RUN apk add --no-cache git \
    && git clone https://github.com/moeakwak/chatgpt-web-share.git /tmp/surenkid


FROM python:3.10-alpine

ARG PIP_CACHE_DIR=/pip_cache
ARG TARGETARCH

RUN mkdir -p /app/backend

RUN apk add --update caddy
RUN if [ "${TARGETARCH}" = "arm64" ] ; then \
        apk add --no-cache gcc musl-dev libffi-dev \
    ; fi

COPY --from=fetchRepo /tmp/surenkid/backend/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

COPY --from=fetchRepo /tmp/surenkid/Caddyfile /app/Caddyfile
COPY --from=fetchRepo /tmp/surenkid/backend /app/backend

WORKDIR /app

EXPOSE 80

COPY --from=fetchRepo /tmp/surenkid/startup.sh /app/startup.sh
RUN chmod +x /app/startup.sh; mkdir /data
CMD ["/app/startup.sh"]
