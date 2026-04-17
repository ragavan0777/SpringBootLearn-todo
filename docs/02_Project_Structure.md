# 📂 Project Structure Explained

This document explains every file and folder in your Spring Boot project. Use this to teach your students how a professional application is organized.

---

## 1. Root Directory

### `pom.xml` (Project Object Model)
*   **What is it?**: The "shopping list" for your project.
*   **What it does**: It tells Maven (the build tool) which libraries to download.
*   **Example**: We added `spring-boot-starter-security` here so we could use login features.

### `src/main/resources/application.properties`
*   **What is it?**: The configuration center.
*   **What it does**: Stores settings like database passwords, server port, etc.
*   **Example**: `server.port=5000` tells the app to run on port 5000 instead of the default 8080.

---

## 2. Java Source Code (`src/main/java/com/example/startSpring`)

We organize code by **function** (what it does), not by random choice.

### `config/` (Configuration)
*   **Purpose**: Setup files that run once when the app starts.
*   **Files**:
    *   `SecurityConfig.java`: Defines who can access what (e.g., "Login is public, everything else is private").
    *   `ApplicationConfig.java`: Creates tools we need, like the PasswordEncoder.

### `controller/` (The Waiter)
*   **Purpose**: The entry point for API requests. It talks to the outside world.
*   **Analogy**: Like a waiter in a restaurant. It takes the order (HTTP Request) and brings back the food (HTTP Response).
*   **Files**:
    *   `TodoController.java`: Handles `/api/v1/todos` requests.
    *   `AuthenticationController.java`: Handles `/api/auth/login` and `/register`.
*   **Rule**: Controllers should **never** have complex logic. They just pass data to the Service.

### `service/` (The Chef)
*   **Purpose**: The business logic. This is where the actual work happens.
*   **Analogy**: The chef in the kitchen. They take the raw ingredients (data) and cook the meal.
*   **Files**:
    *   `TodoService.java`: Contains logic like "Check if todo exists before deleting" and "Only show users their own data".
    *   `AuthenticationService.java`: Handles registering users and generating tokens.

### `repository/` (The Pantry)
*   **Purpose**: Talks to the database.
*   **Analogy**: The pantry or fridge where ingredients are stored.
*   **Files**:
    *   `TodoRepository.java`: Methods to `save()`, `findAll()`, `delete()` Todos.
    *   `UserRepository.java`: Methods to find Users.
*   **Magic**: We extend `JpaRepository`, so we don't have to write SQL queries manually!

### `model/` (The Ingredients)
*   **Purpose**: Defines the shape of our data (Database Tables).
*   **Files**:
    *   `Todo.java`: Defines that a Todo has an `id`, `title`, and `completed` status.
    *   `User.java`: Defines that a User has a `username`, `password`, and `role`.

### `dto/` (The Menu)
*   **Purpose**: Data Transfer Objects. Simple objects used to send data back and forth.
*   **Why?**: We don't want to expose our full database entity (like passwords) to the user.
*   **Files**:
    *   `RegisterRequest.java`: Only contains `username`, `password`, and `role`.
    *   `ApiResponse.java`: A standard wrapper for all responses (`success`, `message`, `data`).

### `exception/` (The Complaint Department)
*   **Purpose**: Handles errors globally.
*   **Files**:
    *   `GlobalExceptionHandler.java`: Catches errors (like "User not found" or "Validation failed") and returns a nice JSON response.

### `security/` (The Bouncer)
*   **Purpose**: Handles security logic.
*   **Files**:
    *   `JwtService.java`: Creates and verifies the digital "ID cards" (Tokens).
    *   `JwtAuthenticationFilter.java`: Checks every request to see if they have a valid ID card.

---

## 3. The Flow of a Request

When a user wants to **Create a Todo**:

1.  **Request**: User sends `POST /api/v1/todos` with JSON data.
2.  **Security**: `JwtAuthenticationFilter` checks if the user is logged in.
3.  **Controller**: `TodoController` receives the request and checks validation (`@Valid`).
4.  **Service**: `TodoService` links the Todo to the current User.
5.  **Repository**: `TodoRepository` saves it to the MySQL database.
6.  **Response**: The saved Todo is wrapped in `ApiResponse` and returned to the user.

---

## 4. Summary for Students

| Package | Role | Analogy |
| :--- | :--- | :--- |
| `controller` | Receives requests | Waiter |
| `service` | Business Logic | Chef |
| `repository` | Database Access | Pantry |
| `model` | Database Tables | Ingredients |
| `dto` | API Data Format | Menu |
| `config` | App Setup | Restaurant Manager |
| `exception` | Error Handling | Complaint Dept |
