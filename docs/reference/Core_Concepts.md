# Spring Boot Core Concepts

## Explained Using a TODO Application

*(Easy to Learn • Easy to Teach • Real-World)*

---

## 🎯 What You’ll Learn Using This TODO App

By the end of this document, you will clearly understand:

* What is **Dependency Injection (DI)**
* What is **Inversion of Control (IoC)**
* What is a **Spring Bean**
* What are **Presentation, Service, Persistence layers**
* Why `@Component`, `@Service`, `@Repository` exist
* How **Plain Java vs Spring Boot** differ

---

# 🧠 PART 1: The Problem (Plain Java – NO Spring)

## 📝 Scenario

We want a simple TODO app:

* Add a todo
* Get all todos

---

## ❌ Plain Java Approach (Tightly Coupled)

```java
public class TodoService {
    public void addTodo(String task) {
        System.out.println("Todo added: " + task);
    }
}

public class TodoController {

    private TodoService todoService = new TodoService();

    public void createTodo(String task) {
        todoService.addTodo(task);
    }
}
```

---

## ❌ What’s Wrong Here?

* `TodoController` **creates** `TodoService`
* Controller decides **which service to use**
* Hard to test
* Hard to change
* No separation of responsibility

---

## 🧠 Real-World Analogy (Bad Office Setup)

👨‍💼 Manager:

* Writes tasks
* Stores files
* Manages database
* Controls everything

👉 Result: Chaos ❌

---

# 🔁 PART 2: Dependency Injection (DI)

## ✅ What is Dependency Injection?

> **Dependency Injection = Giving required objects from outside**

Instead of:

> “I will create my helper”

We say:

> “Someone give me my helper”

---

## ✅ Plain Java WITH Manual DI (Better)

```java
public class TodoService {
    public void addTodo(String task) {
        System.out.println("Todo added: " + task);
    }
}

public class TodoController {

    private TodoService todoService;

    public TodoController(TodoService todoService) {
        this.todoService = todoService;
    }

    public void createTodo(String task) {
        todoService.addTodo(task);
    }
}
```

```java
public class Main {
    static void main(String[] args) {
        TodoService service = new TodoService();
        TodoController controller =
                new TodoController(service);

        controller.createTodo("Learn Spring");
    }
}
```

---

## ⚠️ Still a Problem

* Dependency Injection ✅
* **Inversion of Control ❌**
* You still control object creation

---

# 🔄 PART 3: Inversion of Control (IoC)

## ✅ What is IoC?

> **Inversion of Control = Spring controls object creation**

| Plain Java            | Spring Boot              |
|-----------------------|--------------------------|
| You create objects    | Spring creates objects   |
| You wire dependencies | Spring injects them      |
| You manage lifecycle  | Spring manages lifecycle |

---

## 🧠 Real-World Analogy (Company)

👨‍💼 Employee:

* Doesn’t hire people
* Doesn’t assign roles
* Just does assign work

🏢 Company (Spring):

* Hires
* Assigns
* Manages everyone

---

# 🫘 PART 4: What is a Spring Bean? (VERY IMPORTANT)

## ✅ Definition

> A **Spring Bean** is an object **created and managed by Spring**

📌 If Spring created it → it is a **Bean**

---

### ❌ Not a Bean

```java
TodoService service = new TodoService();
```

### ✅ Bean

```java
@Service
public class TodoService {}
```

---

## 🧠 Where Are Beans Stored?

👉 **Spring IoC Container**

It:

* Creates beans
* Stores beans
* Injects beans
* Manages lifecycle

---

# 🧩 PART 5: Bean-Creating Annotations (TODO Context)

| Annotation        | Used For       | Example        |
|-------------------|----------------|----------------|
| `@RestController` | API layer      | TodoController |
| `@Service`        | Business logic | TodoService    |
| `@Repository`     | DB access      | TodoRepository |
| `@Component`      | Generic helper | TodoMapper     |

📌 Internally all are `@Component`

---

# 🧱 PART 6: Layered Architecture (TODO App)

## 🎯 Why Layers?

> Each layer should do **only ONE job**

---

## 🏗️ TODO App Layers

```
Client (Browser / Postman)
        ↓
TodoController (Presentation)
        ↓
TodoService (Business Logic)
        ↓
TodoRepository (Persistence)
        ↓
Database
```

![Image](https://www.interviewbit.com/blog/wp-content/uploads/2022/06/Spring-Boot-Workflow-Architecture-1024x614.png)

![Image](https://miro.medium.com/0%2AuOLf1KmNN_fr5PJ7.png)

![Image](https://www.interviewbit.com/blog/wp-content/uploads/2022/06/Spring-Boot-Architecture-1-601x1024.png)

---

# 🎨 PART 7: Presentation Layer (Controller)

## 🎯 Responsibility

* Receive HTTP request
* Call service
* Return response

❌ No logic
❌ No database

---

### Example

```java
@RestController
@RequestMapping("/todos")
public class TodoController {

    private final TodoService todoService;

    public TodoController(TodoService todoService) {
        this.todoService = todoService;
    }

    @PostMapping
    public String addTodo(@RequestBody String task) {
        todoService.addTodo(task);
        return "Todo added";
    }
}
```

📌 Layer: **Presentation**
📌 Bean: ✅ Yes

---

# 🧠 PART 8: Service Layer (Business Logic)

## 🎯 Responsibility

* Apply rules
* Decide what should happen

---

### Example

```java
@Service
public class TodoService {

    private final TodoRepository todoRepository;

    public TodoService(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    public void addTodo(String task) {
        if (task.isEmpty()) {
            throw new RuntimeException("Task cannot be empty");
        }
        todoRepository.save(task);
    }
}
```

📌 Layer: **Business Logic**
📌 Bean: ✅ Yes

---

# 🗄️ PART 9: Persistence Layer (Repository)

## 🎯 Responsibility

* Database interaction only

---

### Example

```java
@Repository
public class TodoRepository {

    public void save(String task) {
        System.out.println("Saved to DB: " + task);
    }
}
```

📌 Layer: **Persistence**
📌 Bean: ✅ Yes

---

# 🧱 PART 10: Entity Layer (Optional for Simple TODO)

```java
@Entity
public class Todo {

    @Id
    @GeneratedValue
    private Long id;

    private String task;
    private boolean completed;
}
```

📌 Represents **DB table**

---

# 🔄 PART 11: Full TODO Request Flow

```
POST /todos
   ↓
TodoController
   ↓
TodoService
   ↓
TodoRepository
   ↓
Database
```

Response flows back the same way.

---

# ❌ PART 12: Bad TODO Design (Don’t Teach This)

```java
@RestController
public class TodoController {

    @PostMapping("/todos")
    public void add(@RequestBody String task) {
        System.out.println("Saved todo: " + task);
    }
}
```

❌ No layers
❌ No testing
❌ No scalability

---

# ⚡ PART 13: TODO App – One-Page Cheat Sheet

```
TodoController → Presentation Layer
TodoService → Business Logic
TodoRepository → Persistence Layer

@Bean → Object managed by Spring

DI → Spring injects TodoService into Controller
IoC → Spring creates TodoService & Repository

Flow:
Controller → Service → Repository → DB
```

---

