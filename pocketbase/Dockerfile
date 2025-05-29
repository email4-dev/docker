ARG PB_VERSION=0.28.2

FROM alpine:latest AS setup

ARG PB_VERSION
WORKDIR /setup

RUN apk add --no-cache unzip ca-certificates
ADD https://github.com/pocketbase/pocketbase/releases/download/v${PB_VERSION}/pocketbase_${PB_VERSION}_linux_amd64.zip /tmp/pb.zip
ADD https://github.com/email4-dev/schema/archive/refs/heads/main.zip /tmp/schema.zip
RUN unzip /tmp/pb.zip -d ./
RUN unzip /tmp/schema.zip -d /

FROM alpine:latest AS app

ARG PB_VERSION

WORKDIR /app
COPY --from=setup /setup/pocketbase ./pocketbase
COPY --from=setup /schema-main/pb_migrations /
COPY entrypoint.sh ./entrypoint.sh

RUN addgroup -S pocketbase && adduser -SG pocketbase pocketbase
RUN chmod +x ./pocketbase
RUN chown -R pocketbase:pocketbase ./
RUN chown -R pocketbase:pocketbase /pb_migrations
USER pocketbase

EXPOSE 8090

ENTRYPOINT ["/app/pocketbase", "serve", "--http=0.0.0.0:8090", "--dir=/pb_data", "--publicDir=/pb_public", "--hooksDir=/pb_hooks", "--migrationsDir=/pb_migrations"]