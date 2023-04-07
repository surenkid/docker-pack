FROM openjdk:8-jre-slim
    
RUN apt-get update && apt-get install -y curl \
    && mkdir /app \
    && curl -L -o /app/vmq.war https://github.com/szvone/Vmq/releases/download/v1.6.1/vmq.war \
    && apt-get remove -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

EXPOSE 8080

CMD ["java", "-jar", "vmq.war", "--server.port=8080"]
