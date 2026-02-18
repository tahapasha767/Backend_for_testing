# E-Commerce Backend API

A comprehensive **Spring Boot REST API** for e-commerce applications, providing product catalog management, order processing, customer management, and checkout functionality with MySQL database integration.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [Installation & Setup](#installation--setup)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Docker Support](#docker-support)
- [API Usage Examples](#api-usage-examples)
- [Contributing](#contributing)
- [Contact](#contact)

---

## 🎯 Overview

This is a production-ready Spring Boot backend application designed for e-commerce platforms. It provides RESTful APIs for managing products, customers, orders, and checkout processes. The application uses Spring Data REST for automatic REST endpoint generation and Spring Data JPA for database operations.

**Key Capabilities:**
- Product catalog management with categories
- Customer order processing and tracking
- Checkout workflow with shipping and billing addresses
- Location management (countries and states)
- Order tracking with unique UUID-based tracking numbers
- Automatic REST API generation via Spring Data REST

---

## 🛠 Tech Stack

| Technology | Version/Purpose |
|------------|----------------|
| **Java** | 17 |
| **Spring Boot** | 3.1.3 |
| **Spring Data JPA** | Database abstraction layer |
| **Spring Data REST** | Automatic REST API generation |
| **MySQL** | Relational database |
| **Lombok** | Boilerplate code reduction |
| **Maven** | Dependency management and build tool |
| **Docker** | Containerization support |

### Dependencies

- `spring-boot-starter-data-jpa` - JPA and Hibernate support
- `spring-boot-starter-data-rest` - REST API generation
- `spring-boot-devtools` - Development tools (hot reload)
- `mysql-connector-j` - MySQL database driver
- `lombok` - Code generation annotations
- `spring-boot-starter-test` - Testing framework

---

## ✨ Features

### Core Features

- ✅ **Product Management**
  - Product catalog with categories
  - Product search and filtering
  - SKU management
  - Inventory tracking (units in stock)
  - Product images support

- ✅ **Order Management**
  - Create and process orders
  - Order tracking with unique UUID tracking numbers
  - Order status management
  - Order history per customer

- ✅ **Customer Management**
  - Customer registration and profiles
  - Email-based customer identification
  - Order history tracking
  - Customer address management

- ✅ **Checkout System**
  - Complete checkout workflow
  - Separate shipping and billing addresses
  - Order item management
  - Automatic order tracking number generation

- ✅ **Location Management**
  - Country and state/region data
  - Address validation support
  - Geographic data for shipping

- ✅ **Security & Configuration**
  - CORS configuration for frontend integration
  - Read-only endpoints for product/category data
  - Entity ID exposure for API responses
  - Configurable allowed origins

---

## 📁 Project Structure

```
ecommerce-backend-master/
├── src/
│   ├── main/
│   │   ├── java/com/shittu24/ecommerce/
│   │   │   ├── config/              # Configuration classes
│   │   │   │   └── MyDataRestConfig.java
│   │   │   ├── controller/          # REST Controllers
│   │   │   └── CheckoutController.java
│   │   │   ├── dao/                  # Data Access Objects (Repositories)
│   │   │   ├── ├── CustomerRepository.java
│   │   │   ├── ├── ProductRepository.java
│   │   │   ├── ├── ProductCategoryRepository.java
│   │   │   ├── ├── CountryRepository.java
│   │   │   └── └── StateRepository.java
│   │   │   ├── dto/                  # Data Transfer Objects
│   │   │   ├── ├── Purchase.java
│   │   │   └── └── PurchaseResponse.java
│   │   │   ├── entity/               # JPA Entities
│   │   │   ├── ├── Product.java
│   │   │   ├── ├── ProductCategory.java
│   │   │   ├── ├── Order.java
│   │   │   ├── ├── OrderItem.java
│   │   │   ├── ├── Customer.java
│   │   │   ├── ├── Address.java
│   │   │   ├── ├── Country.java
│   │   │   └── └── State.java
│   │   │   ├── service/              # Business Logic
│   │   │   ├── ├── CheckoutService.java
│   │   │   └── └── CheckoutServiceImplementation.java
│   │   │   └── SpringBootEcommerceApplication.java
│   │   └── resources/
│   │       └── application.properties (to be created)
│   └── test/
│       └── java/com/shittu24/ecommerce/
│           └── SpringBootEcommerceApplicationTests.java
├── pom.xml                           # Maven dependencies
├── Dockerfile                        # Docker configuration
├── mvnw / mvnw.cmd                  # Maven wrapper scripts
└── README.md
```

---

## 🗄 Database Schema

### Entity Relationships

```
ProductCategory (1) ────< (Many) Product
Product (1) ────< (Many) OrderItem
Customer (1) ────< (Many) Order
Order (1) ────< (Many) OrderItem
Order (1) ──── (1) Address (Shipping)
Order (1) ──── (1) Address (Billing)
Country (1) ────< (Many) State
```

### Entities Overview

| Entity | Description | Key Fields |
|--------|-------------|------------|
| **Product** | Product catalog items | `id`, `sku`, `name`, `description`, `unitPrice`, `imageUrl`, `unitsInStock`, `category` |
| **ProductCategory** | Product categories | `id`, `categoryName` |
| **Order** | Customer orders | `id`, `orderTrackingNumber`, `totalPrice`, `totalQuantity`, `status`, `customer`, `orderItems` |
| **OrderItem** | Individual items in an order | `id`, `productId`, `quantity`, `unitPrice`, `imageUrl` |
| **Customer** | Customer information | `id`, `firstName`, `lastName`, `email`, `orders` |
| **Address** | Shipping/billing addresses | `id`, `street`, `city`, `state`, `country`, `zipCode` |
| **Country** | Country data | `id`, `code`, `name` |
| **State** | State/region data | `id`, `name`, `country` |

---

## 🌐 API Endpoints

### Base Path
All endpoints are prefixed with `/api` (configurable via `spring.data.rest.base-path`)

### Product Endpoints (Read-Only)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/products` | Get all products (paginated) |
| `GET` | `/api/products/{id}` | Get product by ID |
| `GET` | `/api/products/search/findByCategoryId?id={categoryId}` | Get products by category |
| `GET` | `/api/products/search/findByNameContaining?name={name}` | Search products by name |

### Product Category Endpoints (Read-Only)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/product-category` | Get all product categories |
| `GET` | `/api/product-category/{id}` | Get category by ID |

### Country Endpoints (Read-Only)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/countries` | Get all countries |
| `GET` | `/api/countries/{id}` | Get country by ID |

### State Endpoints (Read-Only)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/states` | Get all states |
| `GET` | `/api/states/{id}` | Get state by ID |
| `GET` | `/api/states/search/findByCountryCode?code={countryCode}` | Get states by country code |

### Checkout Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/checkout/purchase` | Place a new order (checkout) |

**Note:** Product, ProductCategory, Country, and State endpoints are read-only (POST, PUT, DELETE, PATCH are disabled via configuration).

---

## 🚀 Installation & Setup

### Prerequisites

- **JDK 17** or higher
- **Maven 3.6+** (or use Maven wrapper included)
- **MySQL 8.0+** installed and running
- **Git** (for cloning)

### Step 1: Clone the Repository

```bash
git clone https://github.com/Shittu24/ecommerce-backend.git
cd ecommerce-backend
```

### Step 2: Create MySQL Database

```sql
CREATE DATABASE ecommerce_db;
-- Or use your preferred database name
```

### Step 3: Configure Database Connection

Create `src/main/resources/application.properties` with the following content:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/ecommerce_db
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# Server Configuration
server.port=8080

# Spring Data REST Configuration
spring.data.rest.base-path=/api

# CORS Configuration
allowed.origins=http://localhost:4200,http://localhost:3000
```

**Important:** Replace `your_username` and `your_password` with your MySQL credentials, and `ecommerce_db` with your database name.

### Step 4: Build the Project

**Using Maven Wrapper (Windows):**
```bash
mvnw.cmd clean install
```

**Using Maven Wrapper (Linux/macOS):**
```bash
./mvnw clean install
```

**Using Maven (if installed):**
```bash
mvn clean install
```

---

## ⚙️ Configuration

### Application Properties

Key configuration options:

- **`spring.datasource.*`** - Database connection settings
- **`spring.jpa.hibernate.ddl-auto`** - Database schema management (`update`, `create`, `validate`, `none`)
- **`server.port`** - Server port (default: 8080)
- **`spring.data.rest.base-path`** - Base path for REST APIs (default: `/api`)
- **`allowed.origins`** - Comma-separated list of allowed CORS origins

### CORS Configuration

The application is configured to allow cross-origin requests from specified origins. Update `allowed.origins` in `application.properties` to include your frontend URLs.

---

## 🏃 Running the Application

### Option 1: Using Maven Wrapper

**Windows:**
```bash
mvnw.cmd spring-boot:run
```

**Linux/macOS:**
```bash
./mvnw spring-boot:run
```

### Option 2: Using Maven

```bash
mvn spring-boot:run
```

### Option 3: Running JAR File

After building:
```bash
java -jar target/spring-boot-ecommerce-0.0.1-SNAPSHOT.jar
```

### Option 4: Using IDE

Run `SpringBootEcommerceApplication.java` as a Java application.

### Verify Application is Running

Once started, you can verify by accessing:
- **Health Check:** `http://localhost:8080/api/products` (should return product list or empty array)
- **API Base:** `http://localhost:8080/api`

---

## 🐳 Docker Support

### Build Docker Image

```bash
cd ecommerce-backend-master
docker build -t ecommerce-backend .
```

### Run Docker Container

```bash
docker run -p 8000:8000 \
  -e SPRING_DATASOURCE_URL=jdbc:mysql://host.docker.internal:3306/ecommerce_db \
  -e SPRING_DATASOURCE_USERNAME=your_username \
  -e SPRING_DATASOURCE_PASSWORD=your_password \
  ecommerce-backend
```

**Note:** The Dockerfile exposes port 8000. Ensure your `application.properties` includes `server.port=8000` or set it via environment variable.

### Docker Compose (Optional)

Create `docker-compose.yml` for complete setup:

```yaml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: ecommerce_db
    ports:
      - "3306:3306"
  
  app:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - mysql
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/ecommerce_db
      SPRING_DATASOURCE_USERNAME: root
      SPRING_DATASOURCE_PASSWORD: rootpassword
```

---

## 📝 API Usage Examples

### 1. Get All Products

```bash
curl -X GET http://localhost:8080/api/products
```

**Response:**
```json
{
  "_embedded": {
    "products": [
      {
        "id": 1,
        "sku": "BOOK-TECH-1000",
        "name": "JavaScript - The Fun Parts",
        "description": "Learn JavaScript",
        "unitPrice": 19.99,
        "imageUrl": "assets/images/products/books/book-luv2code-1000.png",
        "active": true,
        "unitsInStock": 100,
        "dateCreated": "2024-01-01T00:00:00",
        "lastUpdated": "2024-01-01T00:00:00",
        "_links": {
          "self": { "href": "http://localhost:8080/api/products/1" },
          "category": { "href": "http://localhost:8080/api/products/1/category" }
        }
      }
    ]
  },
  "_links": {
    "self": { "href": "http://localhost:8080/api/products" }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```

### 2. Get Products by Category

```bash
curl -X GET "http://localhost:8080/api/products/search/findByCategoryId?id=1"
```

### 3. Search Products by Name

```bash
curl -X GET "http://localhost:8080/api/products/search/findByNameContaining?name=JavaScript"
```

### 4. Get All Countries

```bash
curl -X GET http://localhost:8080/api/countries
```

### 5. Get States by Country Code

```bash
curl -X GET "http://localhost:8080/api/states/search/findByCountryCode?code=US"
```

### 6. Place an Order (Checkout)

```bash
curl -X POST http://localhost:8080/api/checkout/purchase \
  -H "Content-Type: application/json" \
  -d '{
    "customer": {
      "firstName": "John",
      "lastName": "Doe",
      "email": "john.doe@example.com"
    },
    "shippingAddress": {
      "street": "123 Main St",
      "city": "New York",
      "state": "New York",
      "country": "United States",
      "zipCode": "10001"
    },
    "billingAddress": {
      "street": "123 Main St",
      "city": "New York",
      "state": "New York",
      "country": "United States",
      "zipCode": "10001"
    },
    "order": {
      "totalPrice": 36.98,
      "totalQuantity": 2
    },
    "orderItems": [
      {
        "imageUrl": "assets/images/products/coffeemugs/coffeemug-luv2code-1000.png",
        "quantity": 1,
        "unitPrice": 18.99,
        "productId": 26
      },
      {
        "imageUrl": "assets/images/products/mousepads/mousepad-luv2code-1000.png",
        "quantity": 1,
        "unitPrice": 17.99,
        "productId": 51
      }
    ]
  }'
```

**Response:**
```json
{
  "orderTrackingNumber": "997e311f-be72-4c45-98ac-86e5541f4c5e"
}
```

**Note:** The order tracking number is a UUID v4 generated automatically for each order.

---

## 🔧 Development

### Project Architecture

- **Layered Architecture:** Controller → Service → Repository → Entity
- **DTO Pattern:** Data Transfer Objects for API requests/responses
- **Repository Pattern:** Spring Data JPA repositories for data access
- **RESTful Design:** Follows REST principles and conventions

### Key Design Decisions

1. **Spring Data REST:** Automatic REST endpoint generation reduces boilerplate code
2. **Read-Only Endpoints:** Product, Category, Country, and State endpoints are read-only for data integrity
3. **UUID Tracking:** Orders use UUID v4 for tracking numbers (globally unique)
4. **Customer Deduplication:** Customers are identified by email to prevent duplicates
5. **Cascade Operations:** Order creation cascades to order items and addresses

### Testing

Run tests with:
```bash
mvnw test
# or
./mvnw test
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
4. **Commit your changes**
   ```bash
   git commit -m "Add: your feature description"
   ```
5. **Push to the branch**
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Open a Pull Request**

### Contribution Guidelines

- Follow Java coding conventions
- Write meaningful commit messages
- Add tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting

---

## 📧 Contact

For questions, support, or collaboration opportunities:

- **Developer:** Ibrahim Shittu
- **Email:** ibrahimshittu007@gmail.com
- **GitHub:** [Shittu24](https://github.com/Shittu24)

---

## 📄 License

This project is open source and available for educational and commercial use.

---

## 🙏 Acknowledgments

- Spring Boot team for the excellent framework
- MySQL team for the robust database system
- Lombok project for reducing boilerplate code
- All contributors and users of this project

---

## 📚 Additional Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA Documentation](https://spring.io/projects/spring-data-jpa)
- [Spring Data REST Documentation](https://spring.io/projects/spring-data-rest)
- [MySQL Documentation](https://dev.mysql.com/doc/)

---

**Last Updated:** February 2026

**Version:** 0.0.1-SNAPSHOT
