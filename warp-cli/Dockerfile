FROM ubuntu:22.04

LABEL mantainer="galtjay<galtjay@foxmail.com>"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gpg \
        ca-certificates \
        supervisor && \
    curl https://pkg.cloudflareclient.com/pubkey.gpg | \
        gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | \
        tee /etc/apt/sources.list.d/cloudflare-client.list && \
    apt-get update && \
    apt-get install -y cloudflare-warp && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo "[program:warp-svc]\n\
command=/bin/bash -c /usr/bin/warp-svc\n\
autostart=true\n\
autorestart=true\n\
startretries=3\n\
stderr_logfile=/var/log/warp.log\n\
stdout_logfile=/var/log/warp.log\n" > /etc/supervisor/conf.d/warp.conf

RUN echo  "#!/bin/bash\n\
set -e\n\
supervisord &\n\
sleep 5\n\
warp-cli --accept-tos register\n\
warp-cli --accept-tos set-mode proxy\n\
warp-cli --accept-tos connect\n\
tail -f /var/log/warp.log\n" > /init.sh && chmod +x /init.sh

EXPOSE 40000/tcp

CMD ["bash", "-c", "/init.sh"]
