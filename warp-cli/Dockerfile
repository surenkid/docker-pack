FROM ubuntu:22.04

LABEL mantainer="surenkids@gmail.com"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gpg \
        ca-certificates \
        socat \
        supervisor && \
    curl https://pkg.cloudflareclient.com/pubkey.gpg | \
        gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | \
        tee /etc/apt/sources.list.d/cloudflare-client.list && \
    apt-get update && \
    apt-get install -y cloudflare-warp && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo_supervisord_conf > /etc/supervisor/supervisord.conf && \
    echo "\
[program:warp-svc]\n\
command=/usr/bin/warp-svc\n\
stdout_events_enabled=true\n\
stderr_events_enabled=true\n\
autorestart=true\n\
priority=100\n\
\n\
[program:init]\n\
command=/bin/bash /init.sh\n\
stdout_events_enabled=true\n\
stderr_events_enabled=true\n\
autorestart=false\n\
priority=200\n\
\n\
[program:socat]\n\
command=socat TCP-LISTEN:65535,fork TCP:127.0.0.1:40000\n\
stdout_events_enabled=true\n\
stderr_events_enabled=true\n\
autorestart=true\n\
priority=300\n" >> /etc/supervisor/supervisord.conf

RUN echo "#!/bin/bash\n\
set -e\n\
register_and_connect() {\n\
  warp-cli --accept-tos register\n\
  warp-cli --accept-tos set-mode proxy\n\
  warp-cli --accept-tos connect\n\
  warp-cli --accept-tos enable-always-on\n\
  warp-cli --accept-tos account\n\
}\n\
register_and_connect\n\
while true; do\n\
  if [[ \$(warp-cli --accept-tos warp-stats | awk 'NR==3') == *GB ]]; then\n\
    warp-cli --accept-tos delete\n\
    register_and_connect\n\
  fi\n\
  sleep 300\n\
done\n" > /init.sh && chmod +x /init.sh

EXPOSE 65535/tcp

ENTRYPOINT ["supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]

