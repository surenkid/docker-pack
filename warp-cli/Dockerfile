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
stdout_logfile=/dev/stdout\n\
stdout_logfile_maxbytes=0\n\
stderr_logfile=/dev/stderr\n\
stderr_logfile_maxbytes=0\n\
autorestart=false\n\
priority=200\n\
\n\
[program:check]\n\
command=/bin/bash /check.sh\n\
stdout_events_enabled=true\n\
stderr_events_enabled=true\n\
stdout_logfile=/dev/stdout\n\
stdout_logfile_maxbytes=0\n\
stderr_logfile=/dev/stderr\n\
stderr_logfile_maxbytes=0\n\
autorestart=true\n\
priority=300\n\
\n\
[program:socat]\n\
command=socat TCP-LISTEN:65535,fork TCP:127.0.0.1:40000\n\
stdout_events_enabled=true\n\
stderr_events_enabled=true\n\
autorestart=true\n\
priority=400\n" >> /etc/supervisor/supervisord.conf

RUN echo "#!/bin/bash\n\
set -e\n\
warp-cli --accept-tos register\n\
warp-cli --accept-tos set-mode proxy\n\
warp-cli --accept-tos connect\n\
warp-cli --accept-tos enable-always-on\n\
warp-cli --accept-tos account\n" > /init.sh && chmod +x /init.sh

RUN echo "#!/bin/bash\n\
set -e\n\
warp_stats=\$(warp-cli --accept-tos warp-stats | awk 'NR==3')\n\
echo \$warp_stats\n\
if [[ \$warp_stats == *GB ]]; then\n\
  warp-cli --accept-tos delete\n\
  warp-cli --accept-tos register\n\
  warp-cli --accept-tos set-mode proxy\n\
  warp-cli --accept-tos connect\n\
  warp-cli --accept-tos enable-always-on\n\
  warp-cli --accept-tos account\n\
fi\n\
sleep 60\n" > /check.sh && chmod +x /check.sh

EXPOSE 65535/tcp

ENTRYPOINT ["supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]

