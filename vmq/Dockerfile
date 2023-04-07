FROM openjdk:8-jre-alpine

RUN apk add --no-cache curl openssl ca-certificates \
    && curl -L -o /app/vmq.war https://github.com/szvone/Vmq/releases/download/v1.6.1/vmq.war \
    && apk del curl openssl ca-certificates

WORKDIR /app

EXPOSE 8080

CMD ["java", "-jar", "vmq.war", "--server.port=8080"]
