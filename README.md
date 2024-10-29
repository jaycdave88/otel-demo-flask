# OpenTelemetry Demo Application

This demo application showcases OpenTelemetry integration with a FastAPI application and PostgreSQL database, including traces, metrics, logs, profiling, and eBPF capabilities.

## Prerequisites

- Docker
- Docker Compose
- cURL (for testing)

## Project Structure

```
otel-demo/
├── README.md
├── docker-compose.yml
├── config/
│   └── otel-collector-config.yaml
└── app/
    ├── Dockerfile
    ├── main.py
    └── requirements.txt
```

## Setup Instructions

1. Clone or create the project structure:
```bash
# Create the directory structure
mkdir -p otel-demo/{app,config}
cd otel-demo
```

2. Copy all configuration files to their respective locations:
- `docker-compose.yml` in root directory
- `config/otel-collector-config.yaml` in config directory
- `main.py`, `Dockerfile`, and `requirements.txt` in app directory

3. Start the environment:
```bash
# Build and start all services
docker-compose up -d --build
```

4. Verify all services are running:
```bash
docker-compose ps
```

You should see three services running:
- app (FastAPI application)
- db (PostgreSQL database)
- otel-collector (OpenTelemetry Collector)

## Testing the Application

### Basic API Tests

1. Test the root endpoint:
```bash
curl http://localhost:8000/
```
Expected response:
```json
{"message": "Hello World"}
```

2. Create a test item:
```bash
curl -X POST "http://localhost:8000/items?name=test-item"
```
Expected response:
```json
{"id": 1, "name": "test-item"}
```

3. Retrieve all items:
```bash
curl http://localhost:8000/items
```
Expected response:
```json
[{"id": 1, "name": "test-item"}]
```

## Monitoring and Observability

### Viewing Telemetry Data

1. Check Application Logs:
```bash
docker-compose logs app
```

2. Check OpenTelemetry Collector Logs:
```bash
docker-compose logs otel-collector
```

3. View Metrics:
```bash
curl http://localhost:8888/metrics
```

### Trace and Metrics Endpoints

- OTLP gRPC: localhost:4317
- OTLP HTTP: localhost:4318
- Prometheus Metrics: localhost:8888

## Troubleshooting

If services aren't starting properly:

1. Check service logs:
```bash
# View logs for all services
docker-compose logs

# View logs for a specific service
docker-compose logs app
docker-compose logs db
docker-compose logs otel-collector
```

2. Restart services:
```bash
# Restart all services
docker-compose restart

# Restart a specific service
docker-compose restart app
```

3. Reset the environment:
```bash
# Stop and remove all containers and volumes
docker-compose down -v

# Rebuild and start
docker-compose up -d --build
```

## Common Issues and Solutions

1. If the app fails to start:
- Check the application logs: `docker-compose logs app`
- Verify all required packages are in requirements.txt
- Ensure PostgreSQL is running: `docker-compose ps`

2. If database connections fail:
- Verify PostgreSQL is running: `docker-compose ps`
- Check database logs: `docker-compose logs db`
- Ensure environment variables match in docker-compose.yml

3. If metrics endpoint is not accessible:
- Verify the collector is running: `docker-compose ps`
- Check collector logs: `docker-compose logs otel-collector`
- Ensure ports are properly exposed in docker-compose.yml

## Stopping the Application

To stop all services:
```bash
docker-compose down
```

To stop all services and remove volumes:
```bash
docker-compose down -v
```

## Configuration Customization

### Exporters

The OpenTelemetry Collector is configured to support multiple exporters. To use a specific backend:

1. Edit `config/otel-collector-config.yaml`
2. Uncomment and configure the desired exporter
3. Update the exporters list in the service pipelines section
4. Restart the collector: `docker-compose restart otel-collector`