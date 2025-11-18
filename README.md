# SpacetimeDB Docker Setup

This repository contains Docker configuration for running SpacetimeDB standalone.

## Prerequisites

- Docker (version 20.10+)
- Docker Compose (version 2.0+)

## Quick Start

### Using Docker Compose (Recommended)

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

Once running, SpacetimeDB will be available at:

- HTTP API: `http://localhost:3000`
- WebSocket: `ws://localhost:3000`

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
