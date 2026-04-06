# 🛒 E-Commerce Microservices Platform

A production-grade microservices e-commerce system built with
Java 17, Spring Boot, Spring Cloud, Apache Kafka, Redis, and Docker.

## Architecture

```
Client → API Gateway → [User | Product | Order | Payment | Notification]
                ↕ Eureka Service Discovery
         MySQL | Redis | Apache Kafka
```

## Tech Stack

| Layer       | Technology                     |
|-------------|-------------------------------|
| Language    | Java 17                        |
| Framework   | Spring Boot 3.x, Spring Cloud  |
| Database    | MySQL 8                        |
| Cache       | Redis 7                        |
| Messaging   | Apache Kafka                   |
| Container   | Docker, Docker Compose         |
| Auth        | JWT (JJWT 0.12)                |
| API Docs    | Swagger / SpringDoc OpenAPI    |

## Services

| Service              | Port | Description                         |
|----------------------|------|-------------------------------------|
| service-discovery    | 8761 | Eureka Server                       |
| api-gateway          | 8080 | Gateway + JWT Auth + Rate Limiting  |
| user-service         | 8081 | Auth, Registration, Profiles        |
| product-service      | 8082 | Products, Search, Redis Cache       |
| order-service        | 8083 | Orders, Kafka Producer              |
| payment-service      | 8084 | Payments, Kafka Consumer/Producer   |
| notification-service | 8085 | Email Notifications via Kafka       |

## Getting Started

### Prerequisites
- Java 17+, Maven 3.9+, Docker Desktop

### Run with Docker Compose
```bash
git clone https://github.com/yourusername/ecommerce-microservices
cd ecommerce-microservices
mvn clean package -DskipTests
docker-compose up --build
```

### Access
- **Eureka Dashboard**: http://localhost:8761
- **API Gateway**: http://localhost:8080
- **Swagger UI**: http://localhost:8081/swagger-ui.html

## API Quick Start
```bash
# Register
curl -X POST http://localhost:8080/api/auth/register \
  -H 'Content-Type: application/json' \
  -d '{"name":"Alice","email":"alice@test.com","password":"Pass123"}'

# Login → get token
curl -X POST http://localhost:8080/api/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"email":"alice@test.com","password":"Pass123"}'

# Browse products
curl http://localhost:8080/api/products \
  -H 'Authorization: Bearer <token>'
```

## Kafka Event Flow
```
Order Placed → order.created → Payment Processing
             → payment.processed → Order Confirmed
             → order.confirmed → Email Notification
```
