# Backend Development with Spring Boot - Complete Guide

## 📚 Course Overview

This guide covers fundamental backend concepts and their implementation in Spring Boot, designed for students learning web application development.

---

## 1. Client-Server Architecture

### What is Client-Server Architecture?

Client-Server architecture is a computing model where tasks are divided between two entities:

- **Client:** The requester of services (e.g., web browser, mobile app)
- **Server:** The provider of services (e.g., your Spring Boot application)

**How it works:**

1. Client sends a request to the server
2. Server processes the request
3. Server sends back a response
4. Client receives and displays the response

### Visual Example

```
Client (Browser)  ←→  Server (Spring Boot App)  ←→  Database
     |                        |                          |
  [Request]              [Process]                   [Store Data]
     |                        |                          |
  [Response]             [Send Back]                [Retrieve Data]

```

---

## 2. What is an API?

### API (Application Programming Interface)

An API is a set of rules that allows different software applications to communicate with each other.

**Think of it like a restaurant:**

- You (client) don't go to the kitchen directly
- You tell the waiter (API) what you want
- The waiter takes your order to the kitchen (server)
- The kitchen prepares your food
- The waiter brings it back to you

### Types of APIs

- **REST API:** Most common, uses HTTP methods (what we'll focus on)
- **SOAP API:** Older, XML-based protocol
- **GraphQL:** Flexible query language for APIs

---

## 3. REST (REpresentational State Transfer)

### What is REST?

REST is an architectural style for designing networked applications. It uses HTTP methods to perform operations on resources.

### REST Principles

- **Stateless:** Each request contains all information needed (no session data stored on server)
- **Client-Server separation:** Client and server are independent
- **Uniform Interface:** Standard HTTP methods for all operations
- **Resource-based:** Everything is a resource (users, products, orders)

### Resources and Endpoints

A **resource** is any data or object (like a User, Product, Order).

An **endpoint** is a URL where the resource can be accessed.

Example:

```
Resource: User
Endpoint: /api/users
Full URL: http://localhost:8080/api/users

```

---

## 4. HTTP Request Methods

HTTP methods tell the server what action to perform on a resource.

### GET - Retrieve Data

**Purpose:** Fetch data from the server (Read operation)

**Characteristics:** Safe, Idempotent (calling it multiple times has the same effect)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // Get all users
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    // Get user by ID
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.findById(id);
    }
}

```

**Example Request:**

```
GET http://localhost:8080/api/users/1

```

### POST - Create New Data

**Purpose:** Create a new resource on the server

**Characteristics:** Not idempotent (calling multiple times creates multiple resources)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
}

```

**Example Request:**

```json
POST http://localhost:8080/api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}

```

### PUT - Update Existing Data (Complete Update)

**Purpose:** Replace an entire resource

**Characteristics:** Idempotent (same result no matter how many times called)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        User updatedUser = userService.update(id, user);
        return ResponseEntity.ok(updatedUser);
    }
}

```

**Example Request:**

```json
PUT http://localhost:8080/api/users/1
Content-Type: application/json

{
  "name": "John Smith",
  "email": "johnsmith@example.com"
}

```

### PATCH - Update Existing Data (Partial Update)

**Purpose:** Update only specific fields of a resource

**Difference from PUT:** PATCH updates only provided fields, PUT replaces entire resource

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PatchMapping("/{id}")
    public ResponseEntity<User> partialUpdateUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
        User updatedUser = userService.partialUpdate(id, updates);
        return ResponseEntity.ok(updatedUser);
    }
}

```

**Example Request:**

```json
PATCH http://localhost:8080/api/users/1
Content-Type: application/json

{
  "email": "newemail@example.com"
}

```

### DELETE - Remove Data

**Purpose:** Delete a resource from the server

**Characteristics:** Idempotent (deleting same resource multiple times has same effect)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}

```

**Example Request:**

```
DELETE http://localhost:8080/api/users/1

```

### Quick Reference Table

| **Method** | **Purpose** | **Example** | **Idempotent** |
| --- | --- | --- | --- |
| GET | Read/Retrieve | Get user list | Yes |
| POST | Create | Create new user | No |
| PUT | Update (full) | Replace user data | Yes |
| PATCH | Update (partial) | Update user email | Yes |
| DELETE | Delete | Remove user | Yes |

---

## 5. HTTP Response Status Codes

Status codes tell the client whether the request was successful and what happened.

### 2xx - Success

- **200 OK:** Request succeeded (GET, PUT, PATCH)
- **201 Created:** New resource created successfully (POST)
- **204 No Content:** Request succeeded but no content to return (DELETE)

### 4xx - Client Errors

- **400 Bad Request:** Invalid request format or data
- **401 Unauthorized:** Authentication required
- **403 Forbidden:** Authenticated but not authorized
- **404 Not Found:** Resource doesn't exist

### 5xx - Server Errors

- **500 Internal Server Error:** Something went wrong on server
- **503 Service Unavailable:** Server temporarily unavailable

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        try {
            User user = userService.findById(id);
            if (user == null) {
                return ResponseEntity.status(HttpStatus.NOT_FOUND).build(); // 404
            }
            return ResponseEntity.ok(user); // 200
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build(); // 500
        }
    }
}

```

---

## 6. Request & Response Structure

### HTTP Request Components

- **Request Line:** Method + URL + HTTP Version
- **Headers:** Metadata about the request (Content-Type, Authorization)
- **Body:** Data being sent (for POST, PUT, PATCH)

**Example HTTP Request:**

```
POST /api/users HTTP/1.1
Host: localhost:8080
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "age": 25
}

```

### HTTP Response Components

- **Status Line:** HTTP Version + Status Code + Status Text
- **Headers:** Metadata about the response (Content-Type, Date)
- **Body:** Data being returned

**Example HTTP Response:**

```
HTTP/1.1 201 Created
Content-Type: application/json
Date: Mon, 05 Jan 2026 05:19:00 GMT

{
  "id": 123,
  "name": "Jane Doe",
  "email": "jane@example.com",
  "age": 25,
  "createdAt": "2026-01-05T05:19:00Z"
}

```

---

## 7. Spring Boot Implementation

### Complete REST Controller Example

```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController // Marks this class as a REST controller
@RequestMapping("/api/users") // Base URL for all endpoints
public class UserController {
    
    @Autowired
    private UserService userService;
    
    // GET /api/users - Get all users
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.findAll();
        return ResponseEntity.ok(users);
    }
    
    // GET /api/users/{id} - Get user by ID
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userService.findById(id);
        if (user == null) {
            return ResponseEntity.notFound().build();
        }
        return ResponseEntity.ok(user);
    }
    
    // POST /api/users - Create new user
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
    
    // PUT /api/users/{id} - Update user
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        User updatedUser = userService.update(id, user);
        if (updatedUser == null) {
            return ResponseEntity.notFound().build();
        }
        return ResponseEntity.ok(updatedUser);
    }
    
    // DELETE /api/users/{id} - Delete user
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        boolean deleted = userService.delete(id);
        if (!deleted) {
            return ResponseEntity.notFound().build();
        }
        return ResponseEntity.noContent().build();
    }
}

```

### User Model Example

```java
package com.example.demo.model;

import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity // Marks this as a database entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    private Integer age;
    
    private LocalDateTime createdAt;
    
    // Constructors
    public User() {
        this.createdAt = LocalDateTime.now();
    }
    
    public User(String name, String email, Integer age) {
        this.name = name;
        this.email = email;
        this.age = age;
        this.createdAt = LocalDateTime.now();
    }
    
    // Getters and Setters
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
    
    public Integer getAge() {
        return age;
    }
    
    public void setAge(Integer age) {
        this.age = age;
    }
    
    public LocalDateTime getCreatedAt() {
        return createdAt;
    }
    
    public void setCreatedAt(LocalDateTime createdAt) {
        this.createdAt = createdAt;
    }
}

```

### Service Layer Example

```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public List<User> findAll() {
        return userRepository.findAll();
    }
    
    public User findById(Long id) {
        Optional<User> user = userRepository.findById(id);
        return user.orElse(null);
    }
    
    public User save(User user) {
        return userRepository.save(user);
    }
    
    public User update(Long id, User userDetails) {
        Optional<User> existingUser = userRepository.findById(id);
        if (existingUser.isPresent()) {
            User user = existingUser.get();
            user.setName(userDetails.getName());
            user.setEmail(userDetails.getEmail());
            user.setAge(userDetails.getAge());
            return userRepository.save(user);
        }
        return null;
    }
    
    public boolean delete(Long id) {
        if (userRepository.existsById(id)) {
            userRepository.deleteById(id);
            return true;
        }
        return false;
    }
}

```

### Repository Example

```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // JpaRepository provides built-in methods:
    // findAll(), findById(), save(), deleteById(), etc.
    
    // You can add custom query methods here
    User findByEmail(String email);
}

```

---

## 8. Testing Your API

### Using Postman or cURL

**GET Request - Fetch all users:**

```bash
curl -X GET http://localhost:8080/api/users

```

**GET Request - Fetch user by ID:**

```bash
curl -X GET http://localhost:8080/api/users/1

```

**POST Request - Create new user:**

```bash
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
  }'

```

**PUT Request - Update user:**

```bash
curl -X PUT http://localhost:8080/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Smith",
    "email": "johnsmith@example.com",
    "age": 31
  }'

```

**DELETE Request - Delete user:**

```bash
curl -X DELETE http://localhost:8080/api/users/1

```

---

## 9. Key Spring Boot Annotations

| **Annotation** | **Purpose** | **Used On** |
| --- | --- | --- |
| @RestController | Marks class as REST controller | Class |
| @RequestMapping | Base URL mapping | Class/Method |
| @GetMapping | Handle GET requests | Method |
| @PostMapping | Handle POST requests | Method |
| @PutMapping | Handle PUT requests | Method |
| @PatchMapping | Handle PATCH requests | Method |
| @DeleteMapping | Handle DELETE requests | Method |
| @PathVariable | Extract value from URL path | Parameter |
| @RequestBody | Convert JSON to Java object | Parameter |
| @RequestParam | Extract query parameters | Parameter |
| @Service | Marks class as service layer | Class |
| @Repository | Marks class as data access layer | Class |
| @Entity | Marks class as database entity | Class |
| @Autowired | Dependency injection | Field/Constructor |

---

## 10. Project Structure

```
src/main/java/com/example/demo/
│
├── DemoApplication.java          # Main application class
│
├── controller/                   # REST Controllers
│   └── UserController.java
│
├── service/                      # Business logic
│   └── UserService.java
│
├── repository/                   # Database access
│   └── UserRepository.java
│
├── model/                        # Entity classes
│   └── User.java
│
└── dto/                          # Data Transfer Objects (optional)
    └── UserDTO.java

src/main/resources/
│
├── application.properties        # Configuration
└── data.sql                      # Initial data (optional)

```

---

## 11. Application Configuration

### application.properties

```
# Server Configuration
server.port=8080

# Database Configuration (H2 for development)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# H2 Console (for testing)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

```

---

## 12. Practice Exercises

### Exercise 1: Basic CRUD API

Create a REST API for managing products with the following:

- Product entity with fields: id, name, price, description, stock
- All CRUD operations (GET, POST, PUT, DELETE)
- Proper status codes

### Exercise 2: Query Parameters

Add a search endpoint to find products:

```java
@GetMapping("/search")
public List<Product> searchProducts(
    @RequestParam(required = false) String name,
    @RequestParam(required = false) Double minPrice,
    @RequestParam(required = false) Double maxPrice
) {
    // Implement search logic
}

```

### Exercise 3: Error Handling

Create a global exception handler:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(404, ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}

```

### Exercise 4: Validation

Add validation to your entities:

```java
import jakarta.validation.constraints.*;

@Entity
public class User {
    @NotBlank(message = "Name is required")
    private String name;
    
    @Email(message = "Email should be valid")
    @NotBlank(message = "Email is required")
    private String email;
    
    @Min(value = 0, message = "Age must be positive")
    @Max(value = 150, message = "Age must be realistic")
    private Integer age;
}

// In controller:
@PostMapping
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    // Spring automatically validates
}

```

---

## 13. Common Pitfalls & Best Practices

### ❌ Common Mistakes

- Not using proper HTTP status codes
- Missing @RequestBody or @PathVariable annotations
- Not handling exceptions properly
- Exposing entity classes directly (should use DTOs)
- Not validating input data

### ✅ Best Practices

- Use meaningful endpoint names (nouns, not verbs)
- Return appropriate status codes
- Use DTOs to control data exposure
- Implement proper error handling
- Add input validation
- Use service layer for business logic
- Keep controllers thin (delegate to services)
- Use proper naming conventions

---

## 14. Additional Resources

- **Spring Boot Documentation:** [https://spring.io/projects/spring-boot](https://spring.io/projects/spring-boot)
- **REST API Tutorial:** [https://restfulapi.net/](https://restfulapi.net/)
- **HTTP Status Codes:** [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- **Testing Tools:** Postman, Insomnia, Thunder Client (VS Code)

---

## 📝 Summary Checklist

- [ ]  Understand Client-Server Architecture
- [ ]  Know what APIs and REST are
- [ ]  Master all HTTP methods (GET, POST, PUT, PATCH, DELETE)
- [ ]  Understand HTTP status codes
- [ ]  Learn Request/Response structure
- [ ]  Implement basic CRUD operations in Spring Boot
- [ ]  Use proper Spring Boot annotations
- [ ]  Test APIs using Postman or cURL
- [ ]  Add validation and error handling
- [ ]  Follow REST API best practices