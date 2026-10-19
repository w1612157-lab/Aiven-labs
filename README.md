# aiven-runtime-n8n

Self-hosted [n8n](https://n8n.io) running behind [Traefik](https://traefik.io) with automatic TLS, deployed via Docker Compose.

## Services

- **traefik** — reverse proxy and TLS termination (Let's Encrypt via `tlschallenge`)
- **n8n** — workflow automation server

## Configuration

Set the following environment variables (e.g. in a `.env` file):

| Variable | Description |
| --- | --- |
| `SSL_EMAIL` | Email used for Let's Encrypt certificate registration |
| `SUBDOMAIN` | Subdomain n8n will be served on |
| `DOMAIN_NAME` | Root domain |
| `GENERIC_TIMEZONE` | Timezone for n8n (e.g. `America/New_York`) |

## Running

```sh
docker compose up -d --build
```

n8n will be available at `https://${SUBDOMAIN}.${DOMAIN_NAME}`.

## Adding tools to n8n

The `n8n` image (`docker/n8n/Dockerfile`) is minimal by default. Any extra tool or language runtime you want available inside n8n (for the `Execute Command` node, custom scripts, etc.) has to be added manually to that Dockerfile — it is not included out of the box.

For example, to add Python:

```dockerfile
FROM n8nio/n8n

USER root
RUN apk add --no-cache python3 py3-pip
USER node
```

Rebuild the image after editing the Dockerfile:

```sh
docker compose up -d --build n8n
```
