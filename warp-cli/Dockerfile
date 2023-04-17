FROM ubuntu:22.10

LABEL mantainer="galtjay<galtjay@foxmail.com>"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gpg \
        supervisor && \
    curl https://pkg.cloudflareclient.com/pubkey.gpg | \
        gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | \
        tee /etc/apt/sources.list.d/cloudflare-client.list && \
    apt-get update && \
    apt-get install cloudflare-warp -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY warp.conf /etc/supervisor/conf.d/warp.conf

COPY init.sh /init.sh
RUN chmod +x /init.sh

EXPOSE 40000/tcp

CMD ["bash", "-c", "/init.sh"]
