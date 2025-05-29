# Email4.dev Docker Deployment

## Download

```sh
git clone https://github.com/email4-dev/docker.git email4dev-docker
cd email4dev-docker
```

## Development

1. Edit the environment variables in docker-compose.local.yml to your liking.
2. Run ```sh docker compose -f docker-compose.local.yml up```
3. Import the schema:

### Either use the pb_migrations

Copy the files from [pb_migrations](https://github.com/email4-dev/schema/pb_migrations) to `/var/lib/docker/volumes/docker_pocketbase_migrations/_data` and restart pocketbase

```sh
docker compose restart pocketbase
```

### Or do a manual import

Open `https://localhost:8090`, create your super user and import the [pb_schema.json](https://github.com/email4-dev/schema/pb_schema.json) and [provider_data](https://github.com/email4-dev/schema/provider_data).

### Access

Either expose the ports in your `docker-compose.local.yml` and use `localhost:port` or use a tunneling service like [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)

## Production

1. Edit the environment variables in `docker-compose.yml` to your liking.
2. Edit `Caddyfile` with your domains.
3. Run ```sh docker compose -f docker-compose.yml up -d ```

## Environment Variables

| Variable            | Default value          | Description                                                                                                                                                 |
| ------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NATS_HOST           | nats:4222              | The address where NATS runs, only change when running the NATS service on a different host                                                                  |
| POCKETBASE_URL      | http://pocketbase:8090 | The address where Pocketbase runs, only change when running Pocketbase on a different host                                                                  |
| POCKETBASE_EMAIL    | admin@mydomain.com     | The superuser's email for Pocketbase                                                                                                                        |
| POCKETBASE_PASS     | MY_STRONG_PA$$W0rd     | The superuser's password for Pocketbase                                                                                                                     |
| API_URL             | http://api:3000        | The address where the API runs, only change when running the API service on a different host                                                                |
| DEBUG               | FALSE                  | Enables more verbose logging. It defaults to `true` in the local environment                                                                                |
| ATTACHMENT_LIMIT    | 20                     | Attachment size limit per file in MB                                                                                                                        |
| CAPTCHA_EXPIRE      | 60                     | After how many seconds the altcha challenge expires. Note that the widget will request a new challenge by default after every expiry.                       |
| CAPTCHA_COMPLEXITY  | 100000                 | How many hashing iterations should the challenge take. The more the more secure, but it takes longer to verify. This MUST match the setting on your widget. |
| VERIFICATION_EXPIRE | 24                     | How often should it check each domain for the verification CNAME record, in hours                                                                           |
| MAX_MESSAGES        | 10                     | How many messages should the mailer handle per batch                                                                                                        |
| INTERVAL            | 10                     | How many seconds between each mailer's batch request                                                                                                        |


Read more at the [documentation](https://docs.email4.dev)