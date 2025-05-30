FROM oven/bun:alpine

WORKDIR /mailer

RUN apk add --no-cache git

RUN git clone https://github.com/email4-dev/mailer.git .

RUN bun install

ENV NODE_ENV=production

ENTRYPOINT ["bun","/mailer/index.ts"]