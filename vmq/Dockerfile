FROM openjdk:8-jre-alpine
RUN apk add --no-cache openssl ca-certificates wget \
    && wget https://github.com/szvone/Vmq/releases/download/v1.6.1/vmq.war -O /app/vmq.war \
    && apk del openssl ca-certificates wget
WORKDIR /app
EXPOSE 8080
CMD ["java", "-jar", "vmq.war", "--server.port=8080"]
