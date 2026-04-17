# 📘 1. Spring Boot Annotations – Study Material

## What is an Annotation?

An annotation in Spring Boot is a **marker** that tells Spring **what a class, method, or variable does**.

Instead of writing long configuration code, we use annotations.

Example:

```java
@RestController
public class UserController {}
```

👉 This tells Spring:

> “This class handles HTTP requests and returns JSON responses.”

---

## 🔷 Core Annotation Categories

| Category             | Purpose                       |
| -------------------- | ----------------------------- |
| Application Setup    | Start & configure Spring Boot |
| Controller Layer     | Handle HTTP requests          |
| Service Layer        | Business logic                |
| Repository Layer     | Database operations           |
| Dependency Injection | Manage object creation        |
| JPA / Entity         | Database mapping              |
| Validation           | Input validation              |
| Exception Handling   | Error control                 |

---

## 🚀 Application-Level Annotations

### `@SpringBootApplication`

📌 **Entry point of Spring Boot app**

```java
@SpringBootApplication
public class BackendApplication {
    public static void main(String[] args) {
        SpringApplication.run(BackendApplication.class, args);
    }
}
```

🔹 Internally combines:

* `@Configuration`
* `@EnableAutoConfiguration`
* `@ComponentScan`

---

## 🌐 Controller Layer Annotations

### `@RestController`

📌 Marks class as REST controller
📌 Returns JSON by default

```java
@RestController
public class UserController {}
```

---

### `@RequestMapping`

📌 Base URL mapping

```java
@RequestMapping("/api/users")
```

---

### HTTP Method Annotations

| Annotation       | HTTP Method | Use            |
| ---------------- | ----------- | -------------- |
| `@GetMapping`    | GET         | Fetch data     |
| `@PostMapping`   | POST        | Create data    |
| `@PutMapping`    | PUT         | Update full    |
| `@PatchMapping`  | PATCH       | Partial update |
| `@DeleteMapping` | DELETE      | Remove data    |

Example:

```java
@GetMapping("/{id}")
public User getUser(@PathVariable Long id) {}
```

---

### Request Handling Annotations

| Annotation      | Purpose              |
| --------------- | -------------------- |
| `@PathVariable` | Read value from URL  |
| `@RequestBody`  | Read JSON body       |
| `@RequestParam` | Read query parameter |

Example:

```java
@PostMapping
public User create(@RequestBody User user) {}
```

---

## 🧠 Service Layer Annotations

### `@Service`

📌 Marks business logic class

```java
@Service
public class UserService {}
```

✔️ Keeps controller clean
✔️ Reusable logic

---

## 🗄️ Repository Layer Annotations

### `@Repository`

📌 Marks database access layer

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {}
```

✔️ Handles database exceptions automatically

---

## 🔁 Dependency Injection Annotations

### `@Autowired`

📌 Injects required dependency

```java
@Autowired
private UserService userService;
```

⚠️ Best Practice:

```java
// Constructor Injection (Recommended)
public UserController(UserService userService) {
    this.userService = userService;
}
```

---

## 🧩 JPA / Entity Annotations

### `@Entity`

📌 Marks class as DB table

```java
@Entity
public class User {}
```

---

### Common JPA Annotations

| Annotation        | Purpose       |
| ----------------- | ------------- |
| `@Id`             | Primary key   |
| `@GeneratedValue` | Auto ID       |
| `@Column`         | Column config |
| `@Table`          | Table name    |
| `@ManyToOne`      | Relationship  |
| `@OneToMany`      | Relationship  |

---

## ✅ Validation Annotations

| Annotation  | Meaning         |
| ----------- | --------------- |
| `@NotNull`  | Cannot be null  |
| `@NotBlank` | Cannot be empty |
| `@Email`    | Valid email     |
| `@Min`      | Minimum value   |
| `@Max`      | Maximum value   |

Example:

```java
@NotBlank
@Email
private String email;
```

Controller:

```java
@PostMapping
public User create(@Valid @RequestBody User user) {}
```

---

## 🚨 Exception Handling Annotations

### `@RestControllerAdvice`

📌 Global error handler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {}
```

---

### `@ExceptionHandler`

📌 Handles specific exception

```java
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<?> handleNotFound() {}
```

---

# 📄 2. Spring Boot Annotations – Documentation (Layer-wise)

### Controller Layer

* `@RestController`
* `@RequestMapping`
* `@GetMapping`
* `@PostMapping`
* `@PathVariable`
* `@RequestBody`

### Service Layer

* `@Service`

### Repository Layer

* `@Repository`
* `JpaRepository`

### Entity Layer

* `@Entity`
* `@Id`
* `@GeneratedValue`
* `@Column`

### Validation

* `@Valid`
* `@NotNull`
* `@Email`

### Error Handling

* `@RestControllerAdvice`
* `@ExceptionHandler`

---

# ⚡ 3. Spring Boot Annotations – CHEAT SHEET (One Page)

```
@SpringBootApplication → Start app

@RestController → REST API
@RequestMapping → Base URL
@GetMapping → Fetch
@PostMapping → Create
@PutMapping → Update
@DeleteMapping → Delete

@PathVariable → URL data
@RequestBody → JSON body
@RequestParam → Query param

@Service → Business logic
@Repository → DB layer

@Entity → Table
@Id → Primary key
@GeneratedValue → Auto ID

@Autowired → Inject dependency

@Valid → Enable validation
@NotBlank → Not empty
@Email → Valid email

@RestControllerAdvice → Global errors
@ExceptionHandler → Handle exception
```

---

