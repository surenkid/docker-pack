FROM openjdk:8-jre-alpine
RUN apk update && apk add curl \
    && curl -L -o /app/vmq.war https://github.com/szvone/Vmq/releases/download/v1.6.1/vmq.war \
    && apk del curl \
    && rm -rf /var/cache/apk/*
WORKDIR /app
EXPOSE 8080
CMD ["java", "-jar", "vmq.war", "--server.port=8080"]
