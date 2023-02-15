# Spring Cloud Currency Microservices

A comprehensive microservices architecture built with Spring Boot and Spring Cloud, demonstrating enterprise-grade patterns for building and deploying cloud-native applications.

## 🏗️ Architecture Overview

This project showcases a complete microservices ecosystem with the following components:

### Core Services
- **Currency Exchange Service** - Provides real-time currency exchange rates with H2 database
- **Currency Conversion Service** - Performs currency conversions using Feign client communication
- **API Gateway** - Central entry point with routing and load balancing
- **Eureka Naming Server** - Service discovery and registration
- **Config Server** - Centralized configuration management
- **Limit Service** - Demonstrates externalized configuration patterns

### 🚀 Key Features Implemented
- **Service Discovery** - Netflix Eureka for automatic service registration and discovery
- **Load Balancing** - Client-side load balancing with Spring Cloud LoadBalancer
- **API Gateway** - Spring Cloud Gateway for routing and cross-cutting concerns
- **Circuit Breaker** - Fault tolerance using Resilience4j
- **Distributed Tracing** - Request tracing with Spring Cloud Sleuth and Zipkin
- **Centralized Configuration** - Spring Cloud Config Server with Git repository
- **Inter-Service Communication** - OpenFeign for declarative REST clients
- **Database Integration** - JPA with H2 for data persistence
- **Containerization** - Docker Compose for easy deployment

## 🛠️ Technology Stack

- **Framework**: Spring Boot 2.x, Spring Cloud 2021.x
- **Service Discovery**: Netflix Eureka
- **API Gateway**: Spring Cloud Gateway
- **Circuit Breaker**: Resilience4j
- **HTTP Client**: OpenFeign
- **Tracing**: Spring Cloud Sleuth + Zipkin
- **Database**: H2 (in-memory)
- **Build Tool**: Maven
- **Containerization**: Docker & Docker Compose

## 📋 Service Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   API Gateway   │───▶│ Currency Exch.  │───▶│   H2 Database   │
│    Port 8765    │    │    Port 8000    │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        ▲
         │                        │
         ▼                        │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│Currency Convert │───▶│  Feign Client   │    │ Eureka Server   │
│    Port 8100    │    │                 │    │    Port 8761    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                                              ▲
         ▼                                              │
┌─────────────────┐    ┌─────────────────┐             │
│  Config Server  │    │ Zipkin Tracing  │             │
│    Port 8888    │    │    Port 9411    │             │
└─────────────────┘    └─────────────────┘             │
         │                                              │
         ▼                                              │
┌─────────────────┐                                     │
│ Git Config Repo │                                     │
│  (External)     │────────────────────────────────────┘
└─────────────────┘
```

## 🚦 Service Ports

|     Service Name          |     Port(s)       |     Description                    |
|---------------------------|-------------------|------------------------------------|
| Currency Exchange Service| 8000, 8001, 8002 | Exchange rate provider             |
| Currency Conversion Service| 8100, 8101, 8102| Currency conversion calculator     |
| API Gateway               | 8765              | Central routing and load balancing |
| Eureka Naming Server      | 8761              | Service discovery registry         |
| Config Server             | 8888              | Centralized configuration          |
| Limit Service             | 8080, 8081        | Configuration demonstration        |
| Zipkin Tracing Server     | 9411              | Distributed tracing UI             |

## 🔗 API Endpoints

### Direct Service Access
| Endpoint | URL | Description |
|----------|-----|-------------|
| Currency Exchange | `http://localhost:8000/currency-exchange/from/USD/to/INR` | Get exchange rate |
| Currency Conversion (Direct) | `http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10` | Convert using RestTemplate |
| Currency Conversion (Feign) | `http://localhost:8100/currency-conversion-feign/from/EUR/to/INR/quantity/10000` | Convert using Feign client |
| Eureka Dashboard | `http://localhost:8761/` | Service registry UI |
| Config Server | `http://localhost:8888/limits-service/default` | Configuration endpoint |
| Zipkin Tracing | `http://localhost:9411/zipkin/` | Distributed tracing UI |

### Through API Gateway
| Endpoint | URL | Description |
|----------|-----|-------------|
| Exchange via Gateway | `http://localhost:8765/currency-exchange-service/currency-exchange/from/EUR/to/INR` | Routed exchange service |
| Conversion via Gateway | `http://localhost:8765/currency-conversion-service/currency-converter-feign/from/USD/to/INR/quantity/10` | Routed conversion service |

### Configuration & Monitoring
| Endpoint | URL | Method | Description |
|----------|-----|--------|-------------|
| Refresh Config | `http://localhost:8080/actuator/refresh` | POST | Refresh service configuration |
| Bus Refresh | `http://localhost:8080/actuator/bus-refresh` | POST | Refresh all services config |
| Service Health | `http://localhost:8080/actuator/health` | GET | Health check endpoint |

## 🚀 Quick Start

### Prerequisites
- Java 11 or higher
- Maven 3.6+
- Docker & Docker Compose (optional)

### Running with Maven
1. **Start Eureka Server**
   ```bash
   cd naming-server
   mvn spring-boot:run
   ```

2. **Start Config Server**
   ```bash
   cd limit-service
   mvn spring-boot:run
   ```

3. **Start Currency Exchange Service**
   ```bash
   cd currency-exchange-service
   mvn spring-boot:run
   ```

4. **Start Currency Conversion Service**
   ```bash
   cd currency-conversion-service
   mvn spring-boot:run
   ```

5. **Start API Gateway**
   ```bash
   cd api-gateway
   mvn spring-boot:run
   ```

### Running with Docker Compose
```bash
docker-compose up -d
```

This will start all services with proper dependencies and networking.

## 🧪 Testing the Services

### Test Currency Exchange
```bash
curl http://localhost:8000/currency-exchange/from/USD/to/INR
```

### Test Currency Conversion
```bash
curl http://localhost:8100/currency-conversion-feign/from/USD/to/INR/quantity/100
```

### Test via API Gateway
```bash
curl http://localhost:8765/currency-conversion-service/currency-conversion-feign/from/USD/to/INR/quantity/100
```

## 🏛️ Microservices Patterns Demonstrated

- **Service Registry Pattern** - Eureka for service discovery
- **API Gateway Pattern** - Single entry point for all services
- **Circuit Breaker Pattern** - Fault tolerance with Resilience4j
- **Configuration Server Pattern** - Externalized configuration
- **Load Balancer Pattern** - Client-side load balancing
- **Distributed Tracing Pattern** - Request tracking across services

## 📊 Monitoring & Observability

- **Service Discovery**: Monitor registered services at `http://localhost:8761`
- **Distributed Tracing**: View request traces at `http://localhost:9411`
- **Health Checks**: Each service exposes `/actuator/health` endpoint
- **Metrics**: Available through Spring Boot Actuator endpoints

## 🔧 Configuration

Services are configured through:
- **application.properties** - Local configuration
- **Git Config Repository** - External configuration for different environments
- **Environment Variables** - Docker/cloud deployment configuration

## 🚀 Deployment

### Local Development
Use Maven to run individual services for development.

### Docker Environment
Use the provided docker-compose.yml for containerized deployment.

### Cloud Deployment
Services are designed to be cloud-native and can be deployed to:
- Kubernetes
- AWS ECS/EKS
- Google Cloud Run
- Azure Container Instances

## 📝 Learning Outcomes

This project demonstrates practical implementation of:
- Microservices architecture patterns
- Spring Cloud ecosystem components
- Service-to-service communication
- Fault tolerance and resilience
- Distributed system observability
- Configuration management
- Container orchestration

---

## 🤝 Contributing

This project serves as a learning reference for Spring Cloud microservices patterns. Feel free to fork and experiment with different configurations.

## 📄 License

This project is created for educational purposes and demonstrates industry-standard microservices patterns using Spring Cloud.

---

## 🔄 Recent Updates

**August 25, 2025**: Updated documentation and project description for better clarity and modern practices.

---
**Built with ❤️ using Spring Boot & Spring Cloud**
