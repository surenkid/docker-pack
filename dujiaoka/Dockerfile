FROM webdevops/php-nginx:7.4
RUN apt-get update && apt-get install -y git && \
    git clone https://github.com/assimon/dujiaoka.git /tmp/dujiaoka && \
    rm -rf /app/* && \
    cp -r /tmp/dujiaoka/* /app && \
    chmod -R 777 /app && \
    rm -rf /tmp/dujiaoka && \
    apt-get clean && \
    apt-get purge -y git && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /app
RUN [ "sh", "-c", "composer install --ignore-platform-reqs" ]
RUN [ "sh", "-c", "chmod -R 777 /app" ]
