FROM node:alpine

WORKDIR /app

RUN apk update && apk add --no-cache git
RUN git clone https://github.com/surenkid/chatgpt-wework-robot.git .

RUN npm install

ENV PORT=8080
ENV BASE_PATH=https://api.openai.com/v1
EXPOSE $PORT

CMD [ "npm", "run", "dev" ]
