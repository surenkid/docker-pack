FROM ubuntu:22.04

LABEL mantainer="surenkids@gmail.com"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gpg \
        ca-certificates \
        tini && \
    curl https://pkg.cloudflareclient.com/pubkey.gpg | \
        gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | \
        tee /etc/apt/sources.list.d/cloudflare-client.list && \
    apt-get update && \
    apt-get install -y cloudflare-warp && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo  "#!/bin/bash\n\
set -e\n\
/usr/bin/warp-svc &\n\
warp_svc_pid=$!\n\
sleep 5\n\
warp-cli --accept-tos register\n\
warp-cli --accept-tos set-mode proxy\n\
warp-cli --accept-tos connect\n\
wait $warp_svc_pid\n" > /init.sh && chmod +x /init.sh

EXPOSE 40000/tcp

ENTRYPOINT ["tini", "--", "bash", "-c", "/init.sh"]
