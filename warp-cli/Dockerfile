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

RUN echo "#!/bin/bash\n\
set -e\n\
/usr/bin/warp-svc &\n\
warp_svc_pid=$!\n\
sleep 10\n\
register_and_connect() {\n\
  warp-cli --accept-tos register\n\
  warp-cli --accept-tos set-mode proxy\n\
  warp-cli --accept-tos connect\n\
  warp-cli --accept-tos enable-always-on\n\
}\n\
register_and_connect\n\
socat TCP-LISTEN:65535,fork TCP:127.0.0.1:40000 &\n\
socat_pid=$!\n\
while true; do\n\
  if [[ $(warp-cli --accept-tos warp-stats | awk 'NR==3') == *GB ]]; then\n\
    warp-cli --accept-tos delete\n\
    register_and_connect\n\
  fi\n\
  sleep 300\n\
done\n\
wait -n $warp_svc_pid $socat_pid\n" > /init.sh && chmod +x /init.sh

EXPOSE 65535/tcp

ENTRYPOINT ["tini", "--", "bash", "-c", "/init.sh"]
