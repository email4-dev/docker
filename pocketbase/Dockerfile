FROM alpine:latest AS setup

ARG PB_VERSION=latest
ARG PB_OS=linux
ARG PB_ARCH=amd64

WORKDIR /setup

RUN apk add --no-cache unzip ca-certificates
ADD https://github.com/pocketbase/pocketbase/releases/download/v${PB_VERSION}/pocketbase_${PB_VERSION}_${PB_OS}_${PB_ARCH}.zip /tmp/pb.zip
ADD https://github.com/email4-dev/schema/archive/refs/heads/main.zip /tmp/schema.zip
RUN unzip /tmp/pb.zip -d ./
RUN unzip /tmp/schema.zip -d /tmp

FROM alpine:latest AS app

WORKDIR /app
COPY --from=setup /setup/pocketbase ./pocketbase
COPY --from=setup /tmp/schema-main/pb_migrations /pb_migrations

RUN apk add curl
RUN addgroup -S pocketbase && adduser -SG pocketbase pocketbase
RUN chmod +x ./pocketbase
RUN chown -R pocketbase:pocketbase ./
RUN mkdir /pb_data /pb_public /pb_hooks
RUN chown -R pocketbase:pocketbase /pb_migrations /pb_data /pb_public /pb_hooks
USER pocketbase

EXPOSE 8090

ENTRYPOINT ["/app/pocketbase", "serve", "--http=0.0.0.0:8090", "--dir=/pb_data", "--publicDir=/pb_public", "--hooksDir=/pb_hooks", "--migrationsDir=/pb_migrations"]