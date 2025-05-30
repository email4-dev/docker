FROM oven/bun:alpine

WORKDIR /api

RUN apk add --no-cache git

RUN git clone https://github.com/email4-dev/api.git .

RUN bun install

EXPOSE 3000

ENV NODE_ENV=production

ENTRYPOINT ["bun","/api/index.ts"]