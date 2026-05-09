# 🔧 Developer Mode

Run infrastructure with Docker and microservices locally from your IDE. Recommended for development and debugging.

---

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- [Git](https://git-scm.com/) installed
- Java 21
- Maven 3.9+

---

## 1. Install the shared library

The microservices depend on a shared library that must be installed in your local Maven repository before running them.

```bash
git clone https://github.com/Digital-Money-Nicolas-Diez/JwtConverterLib.git
cd JwtConverterLib
mvn install -DskipTests
cd ..
```

---

## 2. Run the infrastructure

```bash
docker compose up postgres rabbitmq keycloak eureka-service
```

---

## 3. Run the microservices

Run `user-service` and `account-service` locally from your IDE.

---

## 4. Access the services

| Service | URL |
|---|---|
| Keycloak Admin Panel | http://localhost:8080/admin |
| Keycloak Login/Register | http://localhost:8080/realms/DH_BACKEND/account |
| Eureka Dashboard | http://localhost:8761 |
| RabbitMQ Management | http://localhost:15672 |
| Users Service | http://localhost:8081 |
| Account Service | http://localhost:8082 |

> **Keycloak credentials:** `admin` / `admin`  
> **RabbitMQ credentials:** `guest` / `guest`  
> Note: In Hybrid mode, Keycloak is accessible via `localhost:8080` instead of `keycloak:8080`.

---

## Ports reference

| Service | Port | Protocol |
|---|---|---|
| Keycloak | 8080 | HTTP |
| user-service | 8081 | HTTP |
| account-service | 8082 | HTTP |
| Eureka | 8761 | HTTP |
| RabbitMQ | 5672 | AMQP |
| RabbitMQ Management | 15672 | HTTP |
| PostgreSQL | 5432 | TCP |

---

## Getting a token

To test authenticated endpoints, obtain a Bearer token from Keycloak:

```http
POST http://localhost:8080/realms/DH_BACKEND/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

| Field | Value |
|---|---|
| `grant_type` | `password` |
| `client_id` | `gateway` |
| `username` | your registered username |
| `password` | your registered password |

Use the `access_token` field from the response as a Bearer token in your requests.

---

## Registering a user

User registration is handled directly by Keycloak.  
Go to http://localhost:8080/realms/DH_BACKEND/account and create an account.  
Once registered, the system automatically creates a bank account for the user.
