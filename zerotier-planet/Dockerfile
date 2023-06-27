FROM node:14-buster as builder

ARG ZT_PORT=22083

ENV TZ=Asia/Shanghai

WORKDIR /app

RUN apt-get update \
    && apt-get install -y git wget \
    && git clone https://github.com/xubiaolin/docker-zerotier-planet.git /app

RUN apt-get update \
    && mkdir -p /usr/include/nlohmann/ && cd /usr/include/nlohmann/ && wget https://github.com/nlohmann/json/releases/download/v3.10.5/json.hpp \
    && apt-get install -y git python3 npm make g++ zerotier-one linux-headers \
    && mkdir /app -p &&  cd /app && git clone --progress https://github.com/key-networks/ztncui.git \
    && cd /app/ztncui/src \
    && cp /app/patch/binding.gyp . \
    && echo "Configuring the NPM environment" \
    && npm install -g --progress --verbose node-gyp \
    && npm install \
    && echo 'HTTP_PORT=3443' >.env \
    && echo 'NODE_ENV=production' >>.env \
    && echo 'HTTP_ALL_INTERFACES=true' >>.env \
    && echo "ZT_ADDR=localhost:${ZT_PORT}" >>.env \
    && echo "${ZT_PORT}" >/app/zerotier-one.port \
    && cp -v etc/default.passwd etc/passwd

RUN cd /app && git clone --progress https://github.com/zerotier/ZeroTierOne.git --depth 1 \
    && zerotier-one -d && sleep 5s && ps -ef |grep zerotier-one |grep -v grep |awk '{print $1}' |xargs kill -9 \
    && cd /var/lib/zerotier-one && zerotier-idtool initmoon identity.public >moon.json \
    && cd /app/patch && python3 patch.py \
    && cd /var/lib/zerotier-one && zerotier-idtool genmoon moon.json && mkdir moons.d && cp ./*.moon ./moons.d \
    && cd /app/ZeroTierOne/attic/world/ && sh build.sh \
    && sleep 5s \
    && cd /app/ZeroTierOne/attic/world/ && ./mkworld \
    && mkdir /app/bin -p && cp world.bin /app/bin/planet \
    && TOKEN=$(cat /var/lib/zerotier-one/authtoken.secret) \
    && echo "ZT_TOKEN=$TOKEN" >> /app/ztncui/src/.env

FROM alpine:3.17
WORKDIR /app

COPY --from=builder /app/ztncui /app/ztncui
COPY --from=builder /app/bin /app/bin
COPY --from=builder /app/zerotier-one.port /app/zerotier-one.port
COPY --from=builder /var/lib/zerotier-one /var/lib/zerotier-one

RUN apk update\
    && apk add --no-cache npm zerotier-one

VOLUME [ "/app","/var/lib/zerotier-one" ]

CMD /bin/sh -c "cd /var/lib/zerotier-one && ./zerotier-one -p`cat /app/zerotier-one.port` -d; cd /app/ztncui/src;npm start"
