FROM node:alpine

WORKDIR /app

RUN apk update && apk add --no-cache git
RUN git clone https://github.com/sytpb/chatgpt-wework-robot.git .

RUN npm install

ENV PORT=8080
EXPOSE $PORT

CMD [ "npm", "run", "dev" ]
