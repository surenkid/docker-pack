ARG tag_name=v2.11.10

FROM alpine/git as builder
RUN git clone --depth 1 --branch $tag_name https://github.com/Kerwin1202/chatgpt-web.git
WORKDIR /chatgpt-web

# build front-end
FROM node:lts-alpine AS frontend
RUN npm install pnpm -g
WORKDIR /app
COPY --from=builder /chatgpt-web/package.json /app
COPY --from=builder /chatgpt-web/pnpm-lock.yaml /app
RUN pnpm install
COPY --from=builder /chatgpt-web/ /app
RUN pnpm run build

# build backend
FROM node:lts-alpine as backend
RUN npm install pnpm -g
WORKDIR /app
COPY --from=builder /chatgpt-web/service/package.json /app
COPY --from=builder /chatgpt-web/service/pnpm-lock.yaml /app
RUN pnpm install
COPY --from=builder /chatgpt-web/service /app
RUN pnpm build

# service
FROM node:lts-alpine
RUN npm install pnpm -g
WORKDIR /app
COPY --from=builder /chatgpt-web/service/package.json /app
COPY --from=builder /chatgpt-web/service/pnpm-lock.yaml /app
RUN pnpm install --production && rm -rf /root/.npm /root/.pnpm-store /usr/local/share/.cache /tmp/*
COPY --from=builder /chatgpt-web/service /app
COPY --from=frontend /app/replace-title.sh /app
RUN chmod +x /app/replace-title.sh
COPY --from=frontend /app/dist /app/public
COPY --from=backend /app/build /app/build
COPY --from=backend /app/src/utils/templates /app/build/templates
EXPOSE 3002
CMD ["sh", "-c", "./replace-title.sh && pnpm run prod"]
