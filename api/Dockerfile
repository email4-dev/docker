FROM oven/bun:latest

WORKDIR /api

RUN git clone https://github.com/email4-dev/api.git .

RUN bun install

EXPOSE 3000

ENV NODE_ENV=production

ENTRYPOINT ["bun","/api/index.ts"]