# Cashly API 💰

A comprehensive personal finance management API built with Spring Boot that provides seamless bank integration and financial tracking capabilities.

## Overview

Cashly is a modern personal finance API designed to give users complete visibility and control over their financial data. The application integrates with GoCardless for secure bank authentication while also supporting manual bank and transaction management.

## Features

### 🏦 Bank Integration
- **GoCardless Integration**: Secure bank authentication and data retrieval
- **Multiple Bank Support**: Connect and manage multiple bank accounts
- **Manual Bank Management**: Add banks and transactions without third-party integration
- **Real-time Data**: Fetch latest account balances and transactions

### 📊 Transaction Management
- **Transaction Categorization**: Automatically categorize transactions with manual override capability
- **Historical Data**: Access complete transaction history
- **Custom Categories**: Create and manage custom spending categories
- **Transaction Search**: Advanced filtering and search capabilities

### 🔒 Security & Authentication
- **OAuth2 Integration**: Secure authentication with GoCardless
- **Spring Security**: Comprehensive security framework integration
- **Data Encryption**: Secure storage of sensitive financial data

### 📈 Financial Insights
- **Spending Analysis**: Track spending patterns across categories
- **Account Summaries**: Real-time account balance tracking
- **Export Capabilities**: Export financial data in various formats

## Architecture

Cashly follows a **Modular Clean Architecture** pattern, ensuring separation of concerns and maintainability:

```
src/
├── main/
│   └── java/
│       └── com/cashly/
│           ├── main/           # Application entry point
│           ├── bank/           # Bank module (Clean Architecture)
│           │   ├── domain/     # Entities, value objects, repositories
│           │   ├── application/ # Use cases, services
│           │   └── infrastructure/ # External integrations, DB
│           └── cash/           # Cash/Transaction module (Clean Architecture)
│               ├── domain/
│               ├── application/
│               └── infrastructure/
```

### Core Modules

- **Bank Module**: Handles bank account management, GoCardless integration, and account data
- **Cash Module**: Manages transactions, categorization, and financial analytics
- **Shared Kernel**: Common utilities, configurations, and cross-cutting concerns

## Tech Stack

- **Framework**: Spring Boot 3.5.5
- **Language**: Java 24
- **Database**: MySQL with Flyway migrations
- **Security**: Spring Security + OAuth2
- **HTTP Client**: OpenFeign for external API calls
- **Testing**: JUnit 5 + Testcontainers
- **Containerization**: Docker & Docker Compose
- **Build Tool**: Maven

## Getting Started

### Prerequisites

- Java 24 or higher
- Maven 3.6+
- Docker and Docker Compose
- MySQL (or use Docker Compose setup)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/cashly.git
   cd cashly
   ```

2. **Set up the database**
   ```bash
   docker-compose up -d
   ```

3. **Configure environment variables**
   ```bash
   cp src/main/resources/application.properties.example src/main/resources/application.properties
   ```
   
   Update the configuration with your GoCardless API credentials:
   ```properties
   spring.application.name=cashly-api
   
   # Database Configuration
   spring.datasource.url=jdbc:mysql://localhost:3306/cashly_db
   spring.datasource.username=cashly_user
   spring.datasource.password=your_password
   
   # GoCardless Configuration
   goCardless.secret-id=your_secret_id
   goCardless.secret-key=your_secret_key
   goCardless.environment=sandbox # or live for production
   ```

4. **Run the application**
   ```bash
   ./mvnw spring-boot:run
   ```

The API will be available at `http://localhost:8080`

## API Documentation

### Bank Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/v1/banks` | List all connected banks |
| `POST` | `/api/v1/banks/connect` | Connect a new bank via GoCardless |
| `POST` | `/api/v1/banks` | Manually add a bank |
| `GET` | `/api/v1/banks/{id}/accounts` | Get accounts for a specific bank |
| `DELETE` | `/api/v1/banks/{id}` | Remove a bank connection |

### Transaction Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/v1/transactions` | List transactions with filtering |
| `POST` | `/api/v1/transactions` | Manually add a transaction |
| `PUT` | `/api/v1/transactions/{id}/categorize` | Update transaction category |
| `GET` | `/api/v1/transactions/categories` | List all categories |
| `POST` | `/api/v1/transactions/categories` | Create a new category |

### Analytics Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/v1/analytics/spending` | Get spending analysis |
| `GET` | `/api/v1/analytics/balances` | Get account balance summary |
| `GET` | `/api/v1/analytics/trends` | Get spending trends over time |

## Configuration

### GoCardless Setup

1. Create a GoCardless developer account at [GoCardless Developer Portal](https://developer.gocardless.com/)
2. Obtain your API credentials (Secret ID and Secret Key)
3. Configure the webhook endpoint for real-time updates
4. Update the application properties with your credentials

### Database Migration

The application uses Flyway for database migrations. Migration files are located in:
```
src/main/resources/db/migration/
```

To run migrations manually:
```bash
./mvnw flyway:migrate
```

## Development

### Running Tests

```bash
# Run all tests
./mvnw test

# Run integration tests
./mvnw test -Dtest="*IntegrationTest"

# Run with Testcontainers
./mvnw test -Dspring.profiles.active=test
```

### Code Style

The project follows standard Java coding conventions. Use the provided EditorConfig and Checkstyle configurations.

### Docker Development

For development with Docker:

```bash
# Build the application image
docker build -t cashly-api .

# Run with Docker Compose
docker-compose -f docker-compose.dev.yml up
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please ensure your code follows the project's coding standards and includes appropriate tests.

## Security

- All sensitive data is encrypted at rest
- API endpoints are secured with Spring Security
- GoCardless integration follows OAuth2 security standards
- Regular security audits and dependency updates

## Roadmap

- [ ] Complete GoCardless integration
- [ ] Implement transaction categorization ML model
- [ ] Add budgeting and goal-setting features
- [ ] Mobile app companion
- [ ] Advanced analytics dashboard
- [ ] Multi-currency support
- [ ] Export to popular finance tools

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support and questions:
- Create an issue on GitHub
- Contact: support@cashly.com
- Documentation: [docs.cashly.com](https://docs.cashly.com)

## Acknowledgments

- [GoCardless](https://gocardless.com/) for banking infrastructure
- [Spring Boot](https://spring.io/projects/spring-boot) for the amazing framework
- [Testcontainers](https://www.testcontainers.org/) for integration testing

---

Made with ❤️ by the Cashly team