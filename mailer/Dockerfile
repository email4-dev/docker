FROM oven/bun:latest

WORKDIR /mailer

RUN git clone https://github.com/email4-dev/mailer.git .

RUN bun install

ENV NODE_ENV=production

ENTRYPOINT ["bun","/mailer/index.ts"]