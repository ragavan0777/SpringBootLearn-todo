# 🎓 Spring Boot Ultimate Cheat Sheet

This cheat sheet covers the most important annotations and concepts for your Spring Boot course.

---

## 1. Core Annotations

| Annotation | Meaning | Where to use? |
| :--- | :--- | :--- |
| `@SpringBootApplication` | "Start here!" - The main entry point. | Main Class |
| `@RestController` | "I handle API requests and return JSON." | Controller Class |
| `@Service` | "I contain business logic." | Service Class |
| `@Repository` | "I talk to the database." | Repository Interface |
| `@Entity` | "I am a database table." | Model Class |
| `@Configuration` | "I set things up (beans)." | Config Class |
| `@Bean` | "I am a tool/object that Spring manages." | Method in Config |

---

## 2. Handling HTTP Requests

| Annotation | Method | Example URL | Usage |
| :--- | :--- | :--- | :--- |
| `@GetMapping` | GET | `/todos` | Fetch data |
| `@PostMapping` | POST | `/todos` | Create new data |
| `@PutMapping` | PUT | `/todos/1` | Update (Replace) |
| `@PatchMapping` | PATCH | `/todos/1` | Update (Partial) |
| `@DeleteMapping` | DELETE | `/todos/1` | Remove data |

### Getting Data from the Request

*   **`@RequestBody`**: "Take the JSON sent by the user and turn it into a Java Object."
    ```java
    public void create(@RequestBody Todo todo)
    ```
*   **`@PathVariable`**: "Take the ID from the URL."
    ```java
    @GetMapping("/{id}")
    public void get(@PathVariable Long id)
    ```
*   **`@RequestParam`**: "Take the query parameter (after the ?)."
    ```java
    // URL: /search?name=john
    public void search(@RequestParam String name)
    ```

---

## 3. Validation (Protecting your API)

| Annotation | Meaning |
| :--- | :--- |
| `@Valid` | "Check this object for errors!" (Put on Controller parameter) |
| `@NotBlank` | String cannot be null or empty. |
| `@Size(min=3)` | String must be at least 3 chars long. |
| `@Email` | Must be a valid email format. |
| `@Min(18)` | Number must be 18 or higher. |

---

## 4. Security (The Bouncer)

| Annotation | Meaning |
| :--- | :--- |
| `@EnableWebSecurity` | Turns on the security system. |
| `@PreAuthorize("hasRole('ADMIN')")` | "Only Admins allowed!" |
| `@AuthenticationPrincipal` | "Give me the current logged-in user." |

---

## 5. Database (JPA)

We extend `JpaRepository<Todo, Long>` to get these methods for free:

*   `save(entity)`: Create or Update.
*   `findById(id)`: Find one by ID.
*   `findAll()`: Find all.
*   `deleteById(id)`: Delete one.
*   `count()`: Count total rows.

---

## 6. Common Status Codes

| Code | Meaning | When to use |
| :--- | :--- | :--- |
| **200 OK** | Success | GET, PUT, DELETE |
| **201 Created** | Created | POST (New item made) |
| **400 Bad Request** | User Error | Invalid input (e.g., empty title) |
| **401 Unauthorized** | Who are you? | Missing/Invalid Token |
| **403 Forbidden** | Not Allowed | User tries to do Admin task |
| **404 Not Found** | Missing | ID doesn't exist |
| **500 Server Error** | Our Fault | Code crashed |

---

## 7. Testing Checklist

1.  **Register**: Create a user (`POST /api/auth/register`).
2.  **Login**: Get a token (`POST /api/auth/login`).
3.  **Use Token**: Add header `Authorization: Bearer <token>`.
4.  **Happy Path**: Send valid data -> Expect 200/201.
5.  **Sad Path**: Send invalid data (empty title) -> Expect 400.
6.  **Security Path**: Try Admin action as User -> Expect 403.
