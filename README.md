# Spring Boot JWT Authentication Demo

This is a Spring Boot application that demonstrates JWT (JSON Web Token) based authentication and authorization. The application provides secure endpoints for user registration, login, and accessing user details.

## Features

- User registration and login
- JWT token-based authentication
- Password encryption using BCrypt
- Protected endpoints with JWT validation
- MySQL database integration
- Cross-Origin Resource Sharing (CORS) support

## Prerequisites

- Java 17 or higher
- Maven
- MySQL Server

## Project Structure

```
src/main/java/com/jwtsecurity/demo/
├── config/
│   └── SecurityConfig.java
├── controller/
│   ├── AuthController.java
│   └── UserController.java
├── dto/
│   ├── JwtResponse.java
│   ├── LoginRequest.java
│   └── RegisterRequest.java
├── model/
│   └── User.java
├── repository/
│   └── UserRepository.java
├── security/
│   ├── JwtAuthenticationFilter.java
│   └── JwtTokenProvider.java
└── service/
    └── CustomUserDetailsService.java
```

## Setup Instructions

1. Clone the repository
2. Configure MySQL database:
   - Create a database named `jwt_security_db`
   - Update database credentials in `application.properties` if needed
3. Update JWT secret in `application.properties`:
   ```properties
   app.jwt.secret=your-secret-key-should-be-very-long-and-secure-at-least-32-characters
   ```
4. Build the project:
   ```bash
   mvn clean install
   ```
5. Run the application:
   ```bash
   mvn spring-boot:run
   ```

## API Endpoints

### 1. Register User
- **URL**: `/api/auth/register`
- **Method**: `POST`
- **Auth Required**: No
- **Body**:
  ```json
  {
    "username": "testuser",
    "password": "password123",
    "email": "test@example.com",
    "fullName": "Test User"
  }
  ```
- **Response**: Success message or error if username/email exists

### 2. Login
- **URL**: `/api/auth/login`
- **Method**: `POST`
- **Auth Required**: No
- **Body**:
  ```json
  {
    "username": "testuser",
    "password": "password123"
  }
  ```
- **Response**:
  ```json
  {
    "token": "jwt_token_here",
    "type": "Bearer",
    "username": "testuser"
  }
  ```

### 3. Get User Details
- **URL**: `/api/user`
- **Method**: `GET`
- **Auth Required**: Yes (Bearer Token)
- **Headers**:
  ```
  Authorization: Bearer jwt_token_here
  ```
- **Response**:
  ```json
  {
    "id": 1,
    "username": "testuser",
    "email": "test@example.com",
    "fullName": "Test User"
  }
  ```

## Example Usage with cURL

### Register a new user:
```bash
curl -X POST http://localhost:8080/api/auth/register \
-H "Content-Type: application/json" \
-d '{
  "username": "testuser",
  "password": "password123",
  "email": "test@example.com",
  "fullName": "Test User"
}'
```

### Login to get JWT token:
```bash
curl -X POST http://localhost:8080/api/auth/login \
-H "Content-Type: application/json" \
-d '{
  "username": "testuser",
  "password": "password123"
}'
```

### Get user details (replace {token} with the JWT token):
```bash
curl http://localhost:8080/api/user \
-H "Authorization: Bearer {token}"
```

## Security Features

1. **Password Encryption**: All passwords are encrypted using BCrypt before storing in the database
2. **JWT Token**: Secure token-based authentication with expiration
3. **Protected Endpoints**: All endpoints except registration and login require valid JWT token
4. **CORS Support**: Configured to allow cross-origin requests
5. **Input Validation**: Basic validation for user registration

## Error Handling

The application handles various error scenarios:
- Duplicate username/email during registration
- Invalid credentials during login
- Invalid or expired JWT tokens
- Missing authentication token
- User not found

## Dependencies

- Spring Boot 3.5.0
- Spring Security
- Spring Data JPA
- MySQL Connector
- JWT (jjwt-api, jjwt-impl, jjwt-jackson)
- Lombok
- Spring Web