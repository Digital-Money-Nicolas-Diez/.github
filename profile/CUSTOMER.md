# 🐳 Customer Mode

Run the entire application with Docker. No local Java or Maven setup required.

---

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- [Git](https://git-scm.com/) installed

---

## 1. Configure the hosts file

This project uses a custom Keycloak hostname. Add the following line to your system's hosts file:

**Linux/Mac:**
```bash
echo "127.0.0.1 keycloak" | sudo tee -a /etc/hosts
```

**Windows** (run Notepad as Administrator and open the file):
```
C:\Windows\System32\drivers\etc\hosts
```
Add the following line at the end:
```
127.0.0.1 keycloak
```

---

## 2. Run the application

```bash
docker compose -f docker-compose.yml up --build
```

---

## 3. Access the services

| Service | URL |
|---|---|
| Keycloak Admin Panel | http://keycloak:8080/admin |
| Keycloak Login/Register | http://keycloak:8080/realms/DH_BACKEND/account |
| Eureka Dashboard | http://localhost:8761 |
| RabbitMQ Management | http://localhost:15672 |
| Users Service | http://localhost:8081 |
| Account Service | http://localhost:8082 |

> **Keycloak credentials:** `admin` / `admin`  
> **RabbitMQ credentials:** `guest` / `guest`

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
POST http://keycloak:8080/realms/DH_BACKEND/protocol/openid-connect/token
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
Go to http://keycloak:8080/realms/DH_BACKEND/account and create an account.  
Once registered, the system automatically creates a bank account for the user.
