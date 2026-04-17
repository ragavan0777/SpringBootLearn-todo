# 🎓 Student Learning Path: Spring Boot Todo API

Welcome to the Spring Boot Todo Backend! This project is designed to teach you how to build a production-ready, secure, and distributed API. Follow this path to understand how all the parts fit together.

---

### Step 1: The Foundation (Data Models)
Start here to see how we define our data using Java and JPA (Java Persistence API).
- **File**: `src/main/java/com/example/startSpring/model/Todo.java`
- **What to learn**: How `@Entity` transforms a Java class into a Database table.

### Step 2: The Data Layer (Repositories)
See how we talk to the database without writing complex SQL queries.
- **File**: `src/main/java/com/example/startSpring/repository/TodoRepository.java`
- **What to learn**: The power of `JpaRepository` and how custom methods like `findByUserId` work.

### Step 3: The Brain (Services)
Most of the "logic" happens here. This is where we decide who can see what.
- **File**: `src/main/java/com/example/startSpring/service/TodoService.java`
- **What to learn**: How to isolate data so users can only see their own todos (Multi-tenancy).

### Step 4: The Doorway (Controllers)
This is the API layer that the outside world (like a mobile app or website) talks to.
- **File**: `src/main/java/com/example/startSpring/controller/TodoController.java`
- **What to learn**: RESTful annotations like `@GetMapping`, `@PostMapping`, and Response entities.

### Step 5: The Shield (Security & JWT)
The most advanced part. Learn how we protect our API using JSON Web Tokens.
- **Files**: 
  - `config/SecurityConfig.java`: Defining public vs. private routes.
  - `security/JwtService.java`: Creating and validating secure tokens.
- **What to learn**: Stateless authentication and protected endpoints.

---

### 🛠️ Hands-on Exercises for Students:
1. **Easy**: Change the "Title" validation to require at least 5 characters.
2. **Medium**: Add a "due date" field to the Todo model and update the database.
3. **Hard**: Implement a feature where an Admin can see the total count of Todos for all users.
