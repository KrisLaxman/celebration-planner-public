# Celebration Planner

A production-ready, full-stack event management and productivity application engineered to help users organize complex celebrations, manage task workflows, and track execution progress. Built with an emphasis on multi-tier security architectures, modern state management, and containerized enterprise deployment.

---

## Key Features

* Multi-Factor Authentication (MFA): A two-stage login pipeline utilizing BCrypt password hashing and automated, short-lived email One-Time Passwords (OTPs) via SMTP.
* Stateless Session Traffic: Secured by cryptographically signed JSON Web Tokens (JWT) generated dynamically upon successful MFA validation.
* Comprehensive Event & Task Workflows: Full CRUD operations supporting cascading deletion logic, soft deletes, future-date domain validations, and responsive completion toggles.
* Printable Analytics & Reporting: Custom printer-friendly layout engines providing detailed progress statistics and master planning task sheets.
* Automated Testing & API Docs: Fully integrated Swagger UI (OpenAPI specification) for sandbox endpoint testing and comprehensive Maven unit testing suites.

---

## Technology Stack

### Backend Architecture
* Core: Java 21, Spring Boot 4.0.1
* Security: Spring Security, JJWT (JSON Web Token) 0.13.0
* Data Layer: Spring Data JPA, Hibernate, MySQL 8.0, Lombok
* Build Tool: Maven

### Frontend Engineering
* Core: Angular 20.0.0, TypeScript 5.8.2
* UI/UX: Angular Material 20.2.14, Bootstrap 5.3.8
* Reactive Streams: RxJS 7.8.0

### DevOps & Infrastructure
* Containerization: Docker, Docker Compose (Multi-container orchestration)
* Routing & Security: Nginx (Reverse Proxy), Cloudflare (Edge Network & DNS Management)
* CI/CD: GitLab CI/CD pipelines

---

## System Architecture

The application is structured around a decoupled, layered architecture to maintain a strict separation of concerns, ensuring high scalability and maintainability:
+-------------------------------------------------------------+
|                        Client Browser                       |
|                   Angular SPA (Port 4200)                   |
+-------------------------------------------------------------+
|
v
+-------------------------------------------------------------+
|                     Nginx Reverse Proxy                     |
|                    (Port 80 and 443)                        |
+-------------------------------------------------------------+
|
v
+-------------------------------------------------------------+
|                       Spring Boot API                       |
|  +---------+     +---------+     +---------+     +-------+  |
|  |Controller|--> | Service |-->  |Repository|--> |  JPA  |  |
|  |  Layer   |    |  Layer  |     |  Layer  |     |Entity |  |
|  +---------+     +---------+     +---------+     +-------+  |
|                                                             |
|                    JWT and MFA Security                     |
+-------------------------------------------------------------+
|
v
+-------------------------------------------------------------+
|                        MySQL Database                       |
|               (Port 3306 - Docker Container)                |
+-------------------------------------------------------------+

### Applied Architectural & Design Patterns
* Model-View-Controller (MVC): Decoupled request management, UI views, and backend domain logic.
* Repository Pattern: Complete data-access abstraction layer separating business capabilities from persistent store configurations.
* Data Transfer Object (DTO) Pattern: Decoupling internal schema entities from payload exposure over HTTP networks.
* Observer Pattern (RxJS): Reactive frontend stream binding to deliver smooth, asynchronous interface updates without triggering browser refreshes.

---

## Multi-Step Authentication Workflow
[ Step 1: Login ]
User submits credentials -> Password verified (BCrypt) -> 6-Digit OTP generated & emailed -> Client receives { mfaRequired: true }

[ Step 2: Verification ]
User submits 6-Digit OTP -> Backend authenticates code -> Stateless JWT token signed & issued -> Client intercepts & attaches token to Authorization header

---

## Quick Start

### Local Development Environment
1. Clone the Repository:
    ```bash
    git clone [https://github.com/yourusername/celebration-planner.git](https://github.com/yourusername/celebration-planner.git)
    cd celebration-planner
    ```
2. Spin up Backend Ecosystem:
    ```bash
    cd backend
    ./mvnw clean install
    ./mvnw spring-boot:run
    ```
3. Boot Frontend Single Page App:
    ```bash
    cd ../frontend
    npm install
    npm start
    ```

### Production Orchestration (Docker Compose)
To launch the entire platform, including database layers and reverse proxy paths in a sandboxed, isolated environment:
```bash
docker compose up -d --build

Once deployed, access the interactive API ecosystem at http://localhost:8080/swagger-ui/index.html.
