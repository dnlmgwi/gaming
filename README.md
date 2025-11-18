# SpacetimeDB Docker Setup

This repository contains Docker configuration for running SpacetimeDB standalone.

## Prerequisites

- Docker (version 20.10+)
- Docker Compose (version 2.0+)

## Quick Start

### Development: Using Docker Compose (Recommended)

Start SpacetimeDB:

```bash
docker-compose up -d
```

View logs:

```bash
docker-compose logs -f
```

Stop SpacetimeDB:

```bash
docker-compose down
```

Stop and remove volumes (WARNING: This will delete all data):

```bash
docker-compose down -v
```

### Production: Using Traefik Reverse Proxy

For production deployments with Traefik, use the production compose file:

```bash
docker-compose -f docker-compose.prod.yml up -d
```

**Prerequisites for production:**
- Traefik must be running with a network named `traefik`
- DNS record for `spacetime.newwave.mw` pointing to your server
- (Optional) Configure Let's Encrypt for HTTPS by uncommenting the secure router labels

**Production deployment steps:**

1. Ensure Traefik network exists:
```bash
docker network create traefik
```

2. Start SpacetimeDB:
```bash
docker-compose -f docker-compose.prod.yml up -d
```

3. Check logs:
```bash
docker-compose -f docker-compose.prod.yml logs -f spacetimedb
```

4. Access SpacetimeDB at: `http://spacetime.newwave.mw`

**Enable HTTPS (recommended for production):**

Edit `docker-compose.prod.yml` and uncomment the HTTPS labels:
```yaml
- "traefik.http.routers.spacetime-secure.rule=Host(`spacetime.newwave.mw`)"
- "traefik.http.routers.spacetime-secure.entrypoints=websecure"
- "traefik.http.routers.spacetime-secure.tls.certresolver=letsencrypt"
```

### Using Docker Run

Alternatively, you can run SpacetimeDB directly with Docker:

```bash
docker run --rm --pull always -p 3000:3000 clockworklabs/spacetime start
```

## Configuration

### Ports

- **3000**: SpacetimeDB HTTP API and WebSocket connections

### Volumes

The docker-compose setup persists data in a Docker volume named `spacetimedb-data`. This ensures your database survives container restarts.

### Environment Variables

You can customize SpacetimeDB behavior by uncommenting and modifying environment variables in `docker-compose.yml`:

```yaml
environment:
  SPACETIME_LOG_LEVEL: info  # Options: trace, debug, info, warn, error
```

## Accessing SpacetimeDB

**Development (docker-compose.yml):**
- HTTP API: `http://localhost:3000`
- WebSocket: `ws://localhost:3000`

**Production (docker-compose.prod.yml with Traefik):**
- HTTP API: `http://spacetime.newwave.mw` (or `https://spacetime.newwave.mw` if HTTPS enabled)
- WebSocket: `ws://spacetime.newwave.mw` (or `wss://spacetime.newwave.mw` if HTTPS enabled)

## Health Check

The docker-compose setup includes a health check that monitors the SpacetimeDB service. You can check the health status:

```bash
docker-compose ps
```

## Troubleshooting

### Check logs

```bash
docker-compose logs -f spacetimedb
```

### Restart the service

```bash
docker-compose restart
```

### Reset everything

If you need to start fresh:

```bash
docker-compose down -v
docker-compose up -d
```

## Documentation

For more information, visit the [SpacetimeDB Documentation](https://spacetimedb.com/docs).

## License

See SpacetimeDB official documentation for licensing information.
