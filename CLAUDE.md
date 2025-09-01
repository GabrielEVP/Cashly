# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build and Run
```bash
# Run the application
./mvnw spring-boot:run

# Build the application
./mvnw clean compile

# Package the application
./mvnw clean package
```

### Testing
```bash
# Run all tests
./mvnw test

# Run integration tests
./mvnw test -Dtest="*IntegrationTest"

# Run tests with Testcontainers
./mvnw test -Dspring.profiles.active=test
```

### Database Operations
```bash
# Run Flyway migrations
./mvnw flyway:migrate

# Start database with Docker Compose
docker-compose up -d

# Clean and rebuild database
docker-compose down -v && docker-compose up -d
```

## Architecture Overview

Cashly follows a **Modular Clean Architecture** pattern with clear separation of concerns:

### Module Structure
- **Bank Module** (`com.cashly.bank`): Handles bank account management and GoCardless integration
  - `domain/`: Entities, value objects, and repository interfaces
  - `application/`: Use cases and service interfaces  
  - `infrastructure/`: External integrations, database implementations
- **Cash Module** (`com.cashly.cash`): Manages transactions, categorization, and analytics
  - Same layered structure as Bank module
- **Main Module** (`com.cashly.main`): Application entry point and shared configurations

### Key Technologies
- **Framework**: Spring Boot 3.5.5 with Java 24
- **Database**: MySQL with Flyway migrations
- **Security**: Spring Security + OAuth2 for GoCardless integration
- **HTTP Client**: OpenFeign for external API calls
- **Testing**: JUnit 5 + Testcontainers for integration tests

### Configuration
- Main config: `src/main/resources/application.properties.example`
- Database migrations: `src/main/resources/db/migration/`
- Server runs on port 8080 with context path `/api/v1`

### Integration Points
- **GoCardless API**: Bank data retrieval and authentication
- **MySQL Database**: Primary data storage
- **OAuth2 Flow**: Secure bank connection authorization

## Development Notes

### Database Setup
1. Copy `application.properties.example` to `application.properties`
2. Configure database credentials and GoCardless API keys
3. Run `docker-compose up -d` to start MySQL
4. Migrations run automatically on application startup

### Testing Strategy
- Unit tests for domain logic
- Integration tests with Testcontainers for database operations
- Security tests for OAuth2 flows
- Use `@SpringBootTest` for full application context testing

### API Structure
All endpoints are prefixed with `/api/v1` and include:
- Bank management and connection endpoints
- Transaction CRUD and categorization
- Analytics and reporting endpoints