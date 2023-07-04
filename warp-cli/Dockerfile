FROM ubuntu:22.04

LABEL mantainer="surenkids@gmail.com"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gpg \
        ca-certificates \
        socat \
        tini && \
    curl https://pkg.cloudflareclient.com/pubkey.gpg | \
        gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | \
        tee /etc/apt/sources.list.d/cloudflare-client.list && \
    apt-get update && \
    apt-get install -y cloudflare-warp && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo '#!/bin/bash\n\
set -e\n\
\n\
function start_warp_svc {\n\
  /usr/bin/warp-svc 2>&1 | tee -a warp-svc.log | grep "ERROR" &\n\
  echo $!  # 返回warp-svc的进程ID\n\
}\n\
\n\
function start_socat {\n\
  socat TCP-LISTEN:65535,fork TCP:127.0.0.1:40000 &\n\
  echo $!  # 返回socat的进程ID\n\
}\n\
\n\
register_and_connect() {\n\
  warp-cli --accept-tos register\n\
  warp-cli --accept-tos set-mode proxy\n\
  warp-cli --accept-tos connect\n\
  warp-cli --accept-tos enable-always-on\n\
  warp-cli --accept-tos account\n\
}\n\
\n\
warp_svc_pid=$(start_warp_svc)\n\
socat_pid=$(start_socat)\n\
sleep 5\n\
\n\
register_and_connect\n\
\n\
while true; do\n\
  if ! kill -0 $warp_svc_pid 2>/dev/null; then\n\
    echo "warp_svc has exited, restarting it."\n\
    warp_svc_pid=$(start_warp_svc)\n\
  fi\n\
  if [[ $(warp-cli --accept-tos warp-stats | awk "NR==3") == *GB ]]; then\n\
    warp-cli --accept-tos delete\n\
    register_and_connect\n\
  fi\n\
  sleep 300\n\
done\n\
\n\
wait -n $warp_svc_pid $socat_pid\n' > /init.sh && chmod +x /init.sh

EXPOSE 65535/tcp

ENTRYPOINT ["tini", "--", "bash", "-c", "/init.sh"]
