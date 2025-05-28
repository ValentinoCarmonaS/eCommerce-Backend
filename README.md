# eCommerce Backend

## Overview

The eCommerce Backend is a RESTful API built with **Spring Boot 3.5.0** and **Java 17**, designed to power an online retail platform. This project implements core functionalities for an eCommerce system, including inventory management, user authentication with JSON Web Tokens (JWT), and role-based access control for **Administrators** and **Customers**. The system follows the MVC pattern adapted for a backend API, ensuring modularity, scalability, and adherence to industry best practices. It integrates with a **PostgreSQL** database for persistent storage, uses **Swagger/OpenAPI** for API documentation, and includes comprehensive unit and integration tests to ensure reliability and security.

This project serves as a practical demonstration of modern backend development, showcasing skills in building secure, scalable, and well-documented APIs. It provides a production-ready foundation for an eCommerce platform with potential for future enhancements like order management or payment integration.

## Features

- **Inventory Management**: Create, read, update, and delete products with attributes such as ID, name, description, price, and stock quantity.
- **User Management**: Register and authenticate users with role-based access control (Administrator and Customer roles).
- **Authentication and Authorization**: Secure endpoints using JWT, restricting access based on user roles (e.g., only admins can manage products).
- **API Documentation**: Automatically generated documentation via Swagger UI, accessible at `/swagger-ui.html`.
- **Testing**: Unit and integration tests using JUnit and Spring Test to validate functionality, security, and error handling.
- **Modular Architecture**: Organized into controllers, services, and repositories for maintainability and scalability.
- **Database Flexibility**: Supports PostgreSQL, configurable for local or cloud-based instances (e.g., Neon, Supabase, AWS RDS).

## Technologies Used

- **Java 17**: Programming language for robust backend development.
- **Spring Boot 3.5.0**: Framework for building RESTful APIs with minimal configuration.
- **Spring Data JPA**: For database interaction with PostgreSQL.
- **Spring Security**: For JWT-based authentication and role-based authorization.
- **PostgreSQL**: Relational database for persistent storage.
- **Lombok**: To reduce boilerplate code in entity classes.
- **Springdoc OpenAPI (Swagger)**: For automatic API documentation.
- **JUnit & Spring Test**: For unit and integration testing.
- **java-dotenv**: For managing environment variables in a `.env` file.
- **Maven**: Build tool for dependency management and project compilation.

## Prerequisites

To run this project locally, ensure you have the following installed:

- **Java 17** (JDK)
- **Maven** (for dependency management and building the project)
- **PostgreSQL** (version 13 or higher recommended, either locally or via a cloud provider like Neon, Supabase, or AWS RDS)
- **IntelliJ IDEA** (recommended, or another IDE like VS Code or Eclipse)
- A web browser or API client (e.g., Postman) for testing endpoints
- **Git** (optional, for cloning the repository)

## Installation

Follow these steps to set up and run the project locally:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/ecommerce-backend.git
   cd ecommerce-backend
   ```

2. **Set up PostgreSQL**:
    - **Option 1: Local PostgreSQL**:
    - **Option 2: Cloud PostgreSQL (e.g., Neon)**:
3. **Configure environment variables**:
    - Create a `.env` file in the project root (same directory as `pom.xml`) to store database credentials securely.
    - Add the following variables, replacing the values with your PostgreSQL configuration:
      ```plaintext
      DB_HOST=localhost
      DB_NAME=ecommerce
      DB_USERNAME=your_postgres_username
      DB_PASSWORD=your_postgres_password
      ```
4. **Verify application.properties**:
    - The `src/main/resources/application.properties` file is preconfigured to use environment variables:
      ```properties
      spring.application.name=ecommerce-backend
      spring.datasource.url=jdbc:postgresql://${DB_HOST}/${DB_NAME}?sslmode=require
      spring.datasource.username=${DB_USERNAME}
      spring.datasource.password=${DB_PASSWORD}
      spring.jpa.hibernate.ddl-auto=update
      spring.jpa.show-sql=true
      spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
      springdoc.api-docs.path=/api-docs
      springdoc.swagger-ui.path=/swagger-ui.html
      ```
    - Ensure the `.env` variables match the expected format. For local PostgreSQL, remove `?sslmode=require` from the `spring.datasource.url` if SSL is not enabled:
      ```properties
      spring.datasource.url=jdbc:postgresql://${DB_HOST}:5432/${DB_NAME}
      ```

5. **Install dependencies**:
    - Run the following command to download all dependencies specified in the `pom.xml`:
      ```bash
      mvn clean install
      ```

6. **Run the application**:
    - Execute the application using Maven:
      ```bash
      mvn spring-boot:run
      ```
    - Alternatively, run the main class `EcommerceBackendApplication` in IntelliJ IDEA.
    - The application will start on `http://localhost:8080`.

7. **Access the API**:
    - Open `http://localhost:8080/swagger-ui.html` in a browser to access the Swagger UI for interactive API documentation.
    - Use an API client like Postman to test endpoints.

## Usage

The API provides endpoints for managing products and users, with authentication required for protected operations. Below are the main endpoints and their usage, based on the planned implementation (to be updated as endpoints are developed).

### Authentication

- **Register a User**:
    - **Endpoint**: `POST /api/auth/register`
    - **Description**: Creates a new user account with a specified role (e.g., Customer or Administrator).
    - **Request Body**:
      ```json
      {
          "username": "john_doe",
          "password": "securePassword123",
          "email": "john@example.com",
          "role": "CUSTOMER"
      }
      ```
    - **Response** (201 Created):
      ```json
      {
          "id": 1,
          "username": "john_doe",
          "email": "john@example.com",
          "role": "CUSTOMER"
      }
      ```

- **Login**:
    - **Endpoint**: `POST /api/auth/login`
    - **Description**: Authenticates a user and returns a JWT token.
    - **Request Body**:
      ```json
      {
          "username": "john_doe",
          "password": "securePassword123"
      }
      ```
    - **Response** (200 OK):
      ```json
      {
          "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
      }
      ```

### Product Management

- **Get All Products**:
    - **Endpoint**: `GET /api/products`
    - **Description**: Retrieves a list of all products (accessible to authenticated users).
    - **Response** (200 OK):
      ```json
      [
          {
              "id": 1,
              "name": "Smartphone",
              "description": "High-end smartphone",
              "price": 599.99,
              "stockQuantity": 50
          },
          {
              "id": 2,
              "name": "Headphones",
              "description": "Wireless headphones",
              "price": 79.99,
              "stockQuantity": 100
          }
      ]
      ```

- **Create a Product** (Admin only):
    - **Endpoint**: `POST /api/products`
    - **Description**: Adds a new product to the inventory.
    - **Request Body**:
      ```json
      {
          "name": "Laptop",
          "description": "High-performance laptop",
          "price": 1200.00,
          "stockQuantity": 20
      }
      ```
    - **Response** (201 Created):
      ```json
      {
          "id": 3,
          "name": "Laptop",
          "description": "High-performance laptop",
          "price": 1200.00,
          "stockQuantity": 20
      }
      ```

- **Additional Endpoints**:
    - `GET /api/products/{id}`: Retrieve a specific product.
    - `PUT /api/products/{id}`: Update a product (admin only).
    - `DELETE /api/products/{id}`: Delete a product (admin only).
    - `GET /api/users`: Retrieve user list (admin only).
    - These will be documented as implemented.

### Accessing Protected Endpoints
- Include the JWT token in the `Authorization` header for protected endpoints

### Swagger Documentation
- Explore and test all endpoints interactively via Swagger UI at `http://localhost:8080/swagger-ui.html`.

## Project Structure

```plaintext
ecommerce-backend/
├── src/
│   ├── main/
│   │   ├── java/com/valentino/ecommerce/
│   │   │   ├── config/          # Spring Security and JWT configurations
│   │   │   ├── controller/      # REST controllers for API endpoints
│   │   │   ├── model/           # JPA entities (e.g., Product, User)
│   │   │   ├── repository/      # JPA repositories for database access
│   │   │   ├── service/         # Business logic
│   │   │   └── EcommerceBackendApplication.java
│   │   ├── resources/
│   │   │   └── application.properties  # Configuration for database, Swagger, etc.
│   ├── test/
│   │   └── java/com/valentino/ecommerce/  # Unit and integration tests
├── .env                         # Environment variables (not tracked by Git)
├── .gitignore                   # Files to ignore in Git
├── pom.xml                      # Maven dependencies and build configuration
└── README.md                    # Project documentation
```

## Testing

The project includes unit and integration tests using **JUnit** and **Spring Test**. To run tests:
```bash
mvn test
```

Tests cover:
- CRUD operations for products.
- Authentication and authorization logic (e.g., role-based access).
- Error handling for invalid inputs or unauthorized access.

A test report summarizing coverage and results will be provided as part of the project deliverables.

## Security Considerations

- **Environment Variables**: Sensitive credentials are stored in a `.env` file, which is excluded from version control via `.gitignore`.
- **JWT Security**: Endpoints are protected with JWT, ensuring only authenticated users with appropriate roles can access restricted resources.
- **Input Validation**: All user inputs are validated to prevent injection attacks or invalid data (to be implemented).
- **Password Storage**: Passwords will be hashed using bcrypt for secure storage (to be implemented).

## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For issues or questions, please contact [vcarmona@gmail.com](mailto:vcarmona@gmail.com).
