# 🚀 Building a Professional Todo App: Best Practices & Guide

This guide is designed to help you learn and teach Spring Boot by building a Todo application. It covers code organization, best practices, and a cheat sheet for common tasks.

---

## 1. 📂 Code Organization (Standard Layered Architecture)

In professional Spring Boot applications, we separate concerns into different layers. Do not put everything in one file!

### Recommended Package Structure
For a project named `startSpring`, your package `com.example.startSpring` should look like this:

```text
com.example.startSpring
│
├── StartSpringApplication.java  (Entry point)
│
├── model/                       (Database Entities)
│   └── Todo.java
│
├── repository/                  (Data Access Layer)
│   └── TodoRepository.java
│
├── service/                     (Business Logic Layer)
│   └── TodoService.java
│
├── controller/                  (API Layer - Handles HTTP requests)
│   └── TodoController.java
│
├── dto/                         (Data Transfer Objects - What clients see)
│   ├── TodoDTO.java
│   └── ApiResponse.java         (Standard Response Wrapper)
│
└── exception/                   (Global Error Handling)
    └── GlobalExceptionHandler.java
```

### Why this structure?
1.  **Controller**: Only talks to the Service. It receives the request and returns the response. It should **not** contain business logic.
2.  **Service**: Contains the business logic (validations, calculations). It talks to the Repository.
3.  **Repository**: Talks to the database.
4.  **Model/Entity**: Represents the database table.

---

## 2. ✅ Best Practices & Tips

### 1. Use DTOs (Data Transfer Objects)
**Don't expose your Entity directly.**
*   **Bad**: Returning the `User` entity which contains a `password` field directly to the frontend.
*   **Good**: Create a `UserDTO` that only contains `username` and `email`, and return that.

### 2. Standardize API Responses
Don't return different shapes of data. Use a wrapper like `ApiResponse<T>`:
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... }
}
```

### 3. Dependency Injection
Always use **Constructor Injection** instead of `@Autowired` on fields. It makes testing easier and ensures required dependencies are present.

**Preferred Way:**
```java
@Service
@RequiredArgsConstructor // Lombok annotation to generate constructor
public class TodoService {
    private final TodoRepository repository;
}
```

### 4. Proper HTTP Status Codes
Don't just return 200 OK for everything.
*   `200 OK`: Successful GET, PUT.
*   `201 Created`: Successful POST (creation).
*   `204 No Content`: Successful DELETE.
*   `400 Bad Request`: Validation failed.
*   `404 Not Found`: Resource not found.

### 5. Global Exception Handling
Instead of `try-catch` blocks in every controller, use a `@ControllerAdvice` to handle errors globally. This keeps your controllers clean.

---

## 3. 📜 Spring Boot Cheat Sheet

### Common Annotations

| Annotation | Layer | Purpose |
| :--- | :--- | :--- |
| `@SpringBootApplication` | Main | Starts the application. |
| `@RestController` | Controller | Marks a class as a controller where methods return JSON. |
| `@Service` | Service | Marks a class as holding business logic. |
| `@Repository` | Repository | Marks a class as a database accessor. |
| `@Component` | General | Generic Spring-managed bean. |
| `@Configuration` | Config | Class that defines beans. |

### Request Mapping

| Annotation | HTTP Method | Usage |
| :--- | :--- | :--- |
| `@GetMapping("/todos")` | GET | Read data. |
| `@PostMapping("/todos")` | POST | Create data. |
| `@PutMapping("/todos/{id}")` | PUT | Update data (replace). |
| `@DeleteMapping("/todos/{id}")` | DELETE | Delete data. |
| `@PathVariable` | - | Extract values from URL (e.g., `/{id}`). |
| `@RequestBody` | - | Extract JSON from request body. |
| `@RequestParam` | - | Extract query params (e.g., `?name=foo`). |

---

## 4. 🛠️ Tips for Teaching

1.  **Start Simple**: Start with an in-memory list (using `ArrayList`) before connecting to a real database (H2 or MySQL). This isolates the logic learning from database complexity.
2.  **Debug Mode**: Show students how to use breakpoints in the IDE to trace the flow from Controller -> Service -> Repository.
3.  **Postman**: Teach them to use Postman or cURL to test APIs, as browsers can easily only test GET requests.

---

## 5. Next Steps for Your Todo App

1.  Create the **Model** (`Todo.java`).
2.  Create a **Repository** interface.
3.  Create a **Service** to handle logic.
4.  Create a **Controller** to expose endpoints.
