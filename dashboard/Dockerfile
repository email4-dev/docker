FROM oven/bun:latest AS builder

WORKDIR /dashboard

RUN git clone https://github.com/email4-dev/dashboard.git .

RUN bun install && bun run build

# production
FROM oven/bun:latest

COPY --from=builder /dashboard/package*.json ./
COPY --from=builder /dashboard/build ./

EXPOSE 3000

ENV NODE_ENV=production

ENTRYPOINT ["bun","/dashboard/index.js"]