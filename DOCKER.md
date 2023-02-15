# Docker Deployment Guide

## Prerequisites
- Docker 20.10+
- Docker Compose 2.0+

## Quick Start with Docker
```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

## Service Dependencies
Services start in the following order:
1. RabbitMQ (Message Bus)
2. Zipkin (Tracing)
3. Eureka Naming Server
4. Config Server
5. Currency Exchange Service
6. Currency Conversion Service
7. API Gateway

## Environment Variables
Each service can be configured via environment variables:

### Currency Exchange Service
- `EUREKA_CLIENT_SERVICEURL_DEFAULTZONE`
- `SPRING_ZIPKIN_BASEURL`
- `RABBIT_URI`

### Currency Conversion Service
- `EUREKA_CLIENT_SERVICEURL_DEFAULTZONE`
- `SPRING_ZIPKIN_BASEURL`
- `RABBIT_URI`

## Health Checks
All services expose health endpoints:
- `http://localhost:<port>/actuator/health`

## Monitoring URLs (Docker)
- Eureka: http://localhost:8761
- Zipkin: http://localhost:9411
- API Gateway: http://localhost:8765

## Scaling Services
```bash
# Scale currency exchange service
docker-compose up -d --scale currency-exchange=3

# Scale currency conversion service  
docker-compose up -d --scale currency-conversion=2
```

## Production Considerations
- Use external databases (PostgreSQL/MySQL)
- Configure proper resource limits
- Set up log aggregation
- Implement secrets management
- Configure reverse proxy (nginx/traefik)
