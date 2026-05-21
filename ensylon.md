# 🚀 Complete Interview Preparation Guide — Development Engineer Role

> **Your Goal:** Crack the interview tomorrow. Read every section, understand the concepts, and memorize the answers in your own words.

---

## 📋 TABLE OF CONTENTS

1. [Java Fundamentals](#1-java-fundamentals)
2. [SQL & Databases](#2-sql--databases)
3. [ReactJS Basics](#3-reactjs-basics)
4. [Backend & REST API Concepts](#4-backend--rest-api-concepts)
5. [AI Integration & Gemini API](#5-ai-integration--gemini-api)
6. [Cloud Basics & Docker](#6-cloud-basics--docker)
7. [Quarkus & Kotlin Awareness](#7-quarkus--kotlin-awareness)
8. [Project Explanations](#8-project-explanations)
9. [HR Questions & Answers](#9-hr-questions--answers)
10. [Quick Revision Cheat Sheet](#10-quick-revision-cheat-sheet)

---

# 1. Java Fundamentals

## 🔷 1.1 OOP Concepts

Object-Oriented Programming is a paradigm that organizes software design around **data (objects)** rather than functions.

### 4 Pillars of OOP

---

### 1. Encapsulation
**Definition:** Wrapping data (variables) and the methods that operate on the data within a single unit (class), and restricting direct access to some components.

**Real-World Analogy:** A ATM machine — you can only interact with it through defined buttons. You cannot access the internal cash mechanism directly.

```java
public class BankAccount {
    private double balance; // private = hidden from outside

    public double getBalance() {     // controlled access
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
}
```

**Why it matters:** Prevents unauthorized access, makes code maintainable.

---

### 2. Inheritance
**Definition:** A class (child/subclass) acquires properties and behaviors of another class (parent/superclass).

**Real-World Analogy:** A "Car" inherits from "Vehicle". Car has everything a vehicle has + its own features.

```java
class Vehicle {
    String brand;
    int speed;

    void start() {
        System.out.println("Vehicle started");
    }
}

class Car extends Vehicle {
    int numberOfDoors;

    void playMusic() {
        System.out.println("Playing music");
    }
}

// Usage
Car car = new Car();
car.start();       // inherited from Vehicle
car.playMusic();   // Car's own method
```

**Types of Inheritance in Java:**
- Single: A → B
- Multilevel: A → B → C
- Hierarchical: A → B, A → C
- **Note:** Java does NOT support multiple inheritance through classes (to avoid diamond problem). It supports it through **interfaces**.

---

### 3. Polymorphism
**Definition:** One entity, many forms. The same method behaves differently based on the object calling it.

**Two types:**

#### Compile-time Polymorphism (Method Overloading)
Same method name, different parameters.
```java
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
}
```

#### Runtime Polymorphism (Method Overriding)
Child class provides its own implementation of a parent method.
```java
class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}

// Usage - same reference, different behavior
Animal a = new Dog();
a.sound(); // Bark

Animal b = new Cat();
b.sound(); // Meow
```

---

### 4. Abstraction
**Definition:** Hiding implementation details and showing only essential features to the user.

**Real-World Analogy:** When you press the brake in a car, you don't need to know the hydraulic mechanism — you just know "pressing brake = car slows down."

#### Abstract Class
```java
abstract class Shape {
    abstract double area(); // no implementation, must be overridden

    void printArea() {
        System.out.println("Area = " + area());
    }
}

class Circle extends Shape {
    double radius;

    Circle(double r) { this.radius = r; }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

#### Interface
```java
interface Payable {
    void processPayment(double amount); // always abstract by default
    default void printReceipt() {
        System.out.println("Payment processed");
    }
}

class RazorpayGateway implements Payable {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing via Razorpay: " + amount);
    }
}
```

---

### Interface vs Abstract Class — Most Asked Question

| Feature | Abstract Class | Interface |
|---|---|---|
| Methods | Can have abstract + concrete | All abstract (Java 7), can have default (Java 8+) |
| Variables | Can have instance variables | Only public static final (constants) |
| Constructor | Yes | No |
| Inheritance | `extends` (single) | `implements` (multiple allowed) |
| Use when | Sharing code among related classes | Defining a contract/capability |
| Example | Vehicle (Car, Bike share code) | Flyable (Bird, Plane both fly but unrelated) |

---

## 🔷 1.2 Collections Framework

### Hierarchy Overview
```
Collection
├── List (ordered, allows duplicates)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set (no duplicates)
│   ├── HashSet (no order)
│   ├── LinkedHashSet (insertion order)
│   └── TreeSet (sorted)
└── Queue
    ├── PriorityQueue
    └── LinkedList

Map (key-value pairs)
├── HashMap (no order)
├── LinkedHashMap (insertion order)
└── TreeMap (sorted by key)
```

---

### ArrayList vs LinkedList

| Feature | ArrayList | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Doubly linked list |
| Access (get by index) | O(1) - fast | O(n) - slow |
| Insert/Delete (middle) | O(n) - slow (shifting) | O(1) - fast (pointer change) |
| Memory | Less (no pointers) | More (prev + next pointers) |
| Use when | Frequent read/access | Frequent insert/delete |

```java
// ArrayList - backed by array
ArrayList<String> list = new ArrayList<>();
list.add("Java");
list.add("Python");
System.out.println(list.get(0)); // O(1)

// LinkedList
LinkedList<String> ll = new LinkedList<>();
ll.addFirst("Java");
ll.addLast("Python");
```

---

### HashMap Internals — Very Important

**How HashMap works:**
1. You call `put(key, value)`
2. Java calls `hashCode()` on the key → gives a hash number
3. Hash is mapped to a bucket (array index) using `hash % arraySize`
4. Value is stored in that bucket
5. On `get(key)`: same hash → same bucket → uses `equals()` to find exact key

```
HashMap<String, Integer>
"John" → hashCode() → 1247 → bucket 7 → {John: 25}
"Alice" → hashCode() → 4891 → bucket 3 → {Alice: 30}
```

**Hash Collision:** Two keys map to same bucket.  
**Resolution:** Java uses a linked list (Java 7) or balanced tree (Java 8+) inside the bucket.

**Default initial capacity:** 16  
**Load factor:** 0.75 (resizes when 75% full)

**Why HashMap is O(1) average?** Because hash function directly computes the bucket location — no searching needed.

```java
HashMap<String, Integer> map = new HashMap<>();
map.put("John", 25);
map.put("Alice", 30);

System.out.println(map.get("John")); // 25
System.out.println(map.containsKey("Alice")); // true

// Iteration
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " → " + entry.getValue());
}
```

---

### HashMap vs HashSet

| Feature | HashMap | HashSet |
|---|---|---|
| Stores | Key-Value pairs | Only keys (values) |
| Duplicates | No duplicate keys | No duplicates |
| Null | 1 null key allowed | 1 null allowed |
| Backed by | Own array | Internally uses HashMap |
| Use when | You need key-value mapping | You need unique elements |

---

## 🔷 1.3 String vs StringBuilder

```java
// String is IMMUTABLE — each operation creates a new object
String s = "Hello";
s = s + " World"; // new object created in memory!

// For loops, String creates too many objects
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i; // creates 1000 new String objects — BAD
}

// StringBuilder is MUTABLE — modifies in place
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // modifies same object — GOOD
for (int i = 0; i < 1000; i++) {
    sb.append(i); // efficient
}
String final = sb.toString();
```

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Mutable | No | Yes | Yes |
| Thread safe | Yes (immutable) | No | Yes |
| Performance | Slow (for operations) | Fast | Slower than SB |
| Use when | Simple fixed strings | String operations in single thread | Multi-threaded string operations |

---

## 🔷 1.4 Exception Handling

```java
try {
    int result = 10 / 0; // ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero: " + e.getMessage());
} catch (Exception e) {
    System.out.println("General exception: " + e.getMessage());
} finally {
    System.out.println("This always runs — close DB connections here");
}
```

### Checked vs Unchecked Exceptions

| Type | Description | Examples |
|---|---|---|
| Checked | Must handle at compile time | IOException, SQLException, FileNotFoundException |
| Unchecked (Runtime) | Not required to handle | NullPointerException, ArrayIndexOutOfBoundsException, ArithmeticException |

### Custom Exception
```java
class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

// Usage
void withdraw(double amount) throws InsufficientFundsException {
    if (amount > balance) {
        throw new InsufficientFundsException("Balance too low!");
    }
    balance -= amount;
}
```

---

## 🔷 1.5 Multithreading Basics

**Thread:** A lightweight subprocess — smallest unit of execution.

**Why?** To perform multiple tasks simultaneously (e.g., download a file while the UI remains responsive).

```java
// Method 1: Extending Thread
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

// Method 2: Implementing Runnable (PREFERRED)
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task running");
    }
}

// Usage
Thread t1 = new Thread(new MyTask());
t1.start(); // start() creates new thread and calls run()
// t1.run() would NOT create new thread — just calls method directly
```

**Thread States:** NEW → RUNNABLE → RUNNING → BLOCKED/WAITING → TERMINATED

**Synchronization:** Prevents two threads from modifying shared data at the same time.
```java
synchronized void withdraw(double amount) {
    // only one thread can execute this at a time
    balance -= amount;
}
```

---

## 🔷 1.6 JVM / JDK / JRE

```
JDK (Java Development Kit)
└── JRE (Java Runtime Environment)
    └── JVM (Java Virtual Machine)
```

| Component | Full Form | Role |
|---|---|---|
| JDK | Java Development Kit | Full toolkit for developers: compiler (javac), JRE, debugger, tools |
| JRE | Java Runtime Environment | Runtime libraries + JVM — needed to RUN Java programs |
| JVM | Java Virtual Machine | Executes bytecode; makes Java platform-independent |

**How Java achieves platform independence:**
1. Java source code (`.java`) → compiled by javac → **bytecode** (`.class`)
2. JVM (available for every OS) interprets and runs the bytecode
3. "Write Once, Run Anywhere"

---

## 🔷 1.7 == vs .equals()

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

// == compares REFERENCES (memory addresses)
System.out.println(s1 == s2);      // true (same string pool reference)
System.out.println(s1 == s3);      // false (s3 is a new object in heap)

// .equals() compares CONTENT (values)
System.out.println(s1.equals(s3)); // true (same content)
```

**Rule:** Always use `.equals()` to compare String values. Use `==` for primitives or when you actually want to check reference equality.

---

## 🔷 1.8 Heap vs Stack

| Feature | Stack | Heap |
|---|---|---|
| Stores | Method calls, local variables, references | Objects, instance variables |
| Size | Small | Large |
| Access | Fast (LIFO) | Slower |
| Lifetime | Only during method execution | Until garbage collected |
| Thread | Each thread has own stack | Shared across all threads |

```java
void calculate() {
    int x = 10;         // x goes in Stack
    String name = "Java"; // reference goes in Stack, "Java" object in Heap
    Person p = new Person(); // p (reference) in Stack, Person object in Heap
}
// When method ends, stack frame is popped — x, name, p references are gone
// Person object in heap stays until GC cleans it
```

---

## 📌 Java Interview Questions & Answers

**Q: Explain polymorphism with a real example.**
> Polymorphism means one interface, multiple implementations. In a payment system, you might have a `Payment` interface with a `processPayment()` method. `CreditCardPayment`, `UPIPayment`, and `NetBankingPayment` all implement it differently. The calling code just calls `payment.processPayment()` without caring which type it is — the correct behavior runs automatically based on the actual object.

**Q: What is the difference between Array and ArrayList?**
> Array has fixed size defined at creation; ArrayList is dynamic and can grow/shrink. Array can store primitives; ArrayList only stores objects. ArrayList has built-in methods like `add()`, `remove()`, `size()`. For known fixed-size data, use Array; for dynamic collections, use ArrayList.

**Q: Why is Java platform independent?**
> Java compiles to bytecode (`.class` file) rather than machine code. The JVM installed on each OS (Windows, Mac, Linux) interprets this bytecode and converts it to platform-specific machine code at runtime. So the same `.class` file runs on any system with a JVM — "Write Once, Run Anywhere."

**Q: What is the difference between `throw` and `throws`?**
> `throw` is used inside a method to actually throw an exception object: `throw new Exception()`. `throws` is used in the method signature to declare that this method might throw that exception: `void read() throws IOException`.

**Q: Why would you use an interface over an abstract class?**
> When you want to define a contract/capability that unrelated classes can implement. For example, `Serializable`, `Comparable`, `Runnable` are interfaces — a `Dog`, `BankAccount`, and `Thread` are completely unrelated classes but all can implement `Runnable`. Interfaces also allow multiple implementation, which abstract classes don't.

---

# 2. SQL & Databases

## 🔷 2.1 Core SQL Concepts

### Basic Queries
```sql
-- SELECT with WHERE
SELECT name, salary FROM employees WHERE department = 'Engineering';

-- ORDER BY
SELECT * FROM employees ORDER BY salary DESC;

-- LIMIT
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;

-- DISTINCT
SELECT DISTINCT department FROM employees;
```

---

### JOINs — Most Important

```
Table: employees          Table: departments
id | name  | dept_id      id | dept_name
1  | Alice  | 10          10 | Engineering
2  | Bob    | 20          20 | Marketing
3  | Charlie| NULL        30 | HR
```

```sql
-- INNER JOIN: only matching rows from BOTH tables
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
-- Result: Alice-Engineering, Bob-Marketing (Charlie excluded — no match)

-- LEFT JOIN: all from left table + matching from right
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
-- Result: Alice-Engineering, Bob-Marketing, Charlie-NULL

-- RIGHT JOIN: all from right + matching from left
SELECT e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
-- Result: Alice-Engineering, Bob-Marketing, NULL-HR

-- FULL OUTER JOIN: all rows from both
SELECT e.name, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
-- Result: All rows, NULLs where no match
```

---

### GROUP BY + Aggregate Functions

```sql
-- COUNT employees per department
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;

-- AVG salary per department
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- HAVING (filter after group — like WHERE but for groups)
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5; -- departments with more than 5 employees

-- Aggregate functions: COUNT, SUM, AVG, MIN, MAX
SELECT 
    department,
    COUNT(*) as total,
    SUM(salary) as total_salary,
    AVG(salary) as avg_salary,
    MIN(salary) as min_salary,
    MAX(salary) as max_salary
FROM employees
GROUP BY department;
```

---

### Subqueries

```sql
-- Employees earning more than company average
SELECT name, salary 
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Second highest salary
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- OR using DENSE_RANK (better approach)
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM employees
) ranked
WHERE rnk = 2;
```

---

### Window Functions

```sql
-- ROW_NUMBER: unique sequential number
SELECT name, salary, 
       ROW_NUMBER() OVER (ORDER BY salary DESC) as row_num
FROM employees;

-- RANK: same rank for ties, gaps after ties (1,1,3)
SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- DENSE_RANK: same rank for ties, NO gaps (1,1,2)
SELECT name, salary,
       DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
FROM employees;

-- Running total
SELECT name, salary,
       SUM(salary) OVER (ORDER BY name) as running_total
FROM employees;

-- Partition by department
SELECT name, department, salary,
       AVG(salary) OVER (PARTITION BY department) as dept_avg
FROM employees;
```

---

### Practice Questions with Solutions

**Q1: Find employees earning above their department average**
```sql
SELECT e.name, e.salary, e.department
FROM employees e
WHERE e.salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department
);
```

**Q2: Find duplicate records (by email)**
```sql
SELECT email, COUNT(*) as count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

**Q3: Nth highest salary**
```sql
-- Second highest salary
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1; -- skip 1, take 1

-- For Nth: OFFSET N-1
```

**Q4: Count employees per department, include departments with 0 employees**
```sql
SELECT d.dept_name, COUNT(e.id) as employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.dept_id
GROUP BY d.dept_name;
```

**Q5: Running total of sales by date**
```sql
SELECT sale_date, amount,
       SUM(amount) OVER (ORDER BY sale_date) as running_total
FROM sales;
```

---

## 🔷 2.2 Database Concepts

### Normalization
Process of organizing a database to reduce redundancy and improve data integrity.

**1NF (First Normal Form):**
- Each column has atomic (indivisible) values
- No repeating groups
- BAD: `skills = "Java, Python, React"` (multiple values in one column)
- GOOD: Separate rows for each skill

**2NF (Second Normal Form):**
- Must be in 1NF
- Every non-key attribute must depend on the ENTIRE primary key (no partial dependency)

**3NF (Third Normal Form):**
- Must be in 2NF
- No transitive dependencies (non-key column depends on another non-key column)

---

### Primary Key vs Foreign Key

```sql
CREATE TABLE departments (
    id INT PRIMARY KEY,    -- PRIMARY KEY: unique identifier, NOT NULL
    name VARCHAR(50)
);

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(id)  -- FOREIGN KEY: links to another table
);
```

**Primary Key:** Uniquely identifies each row in a table. Cannot be NULL. One per table.  
**Foreign Key:** A column in one table that references the primary key of another table. Enforces referential integrity.

---

### Indexing

**What is an index?** A data structure (B-tree) that speeds up SELECT queries at the cost of slower INSERT/UPDATE and more storage.

```sql
-- Without index: scans entire table (full table scan)
SELECT * FROM employees WHERE email = 'alice@example.com'; -- O(n)

-- Create index
CREATE INDEX idx_email ON employees(email);

-- With index: directly looks up location (like a book index)
-- O(log n) with B-tree index
```

**When to use index:**
- Columns used frequently in WHERE clauses
- Columns used in JOIN conditions
- Columns used in ORDER BY

**When NOT to index:**
- Small tables (full scan is faster)
- Columns rarely searched
- Columns with many duplicate values (low cardinality)

---

## 📌 SQL Interview Questions & Answers

**Q: Difference between WHERE and HAVING?**
> `WHERE` filters rows BEFORE grouping — it operates on individual rows. `HAVING` filters AFTER grouping — it operates on groups/aggregated results. `WHERE` cannot use aggregate functions; `HAVING` can. Example: `WHERE salary > 50000` vs `HAVING AVG(salary) > 50000`.

**Q: What is a JOIN? How many types are there?**
> A JOIN combines rows from two or more tables based on a related column. Types: INNER JOIN (only matching rows), LEFT JOIN (all from left + matching from right), RIGHT JOIN (all from right + matching from left), FULL OUTER JOIN (all rows from both tables).

**Q: Difference between DELETE, TRUNCATE, DROP?**
> `DELETE` removes specific rows (can use WHERE), is logged, can be rolled back. `TRUNCATE` removes ALL rows, faster, minimal logging, cannot use WHERE, can be rolled back in some DBs. `DROP` removes the entire table including its structure — cannot be rolled back.

---

# 3. ReactJS Basics

## 🔷 3.1 Core Concepts

### Components
```jsx
// Functional Component (modern, preferred)
function Greeting({ name }) {
    return <h1>Hello, {name}!</h1>;
}

// Usage
<Greeting name="Alice" />
```

---

### Props vs State

| Feature | Props | State |
|---|---|---|
| Definition | Data passed FROM parent TO child | Data managed WITHIN the component |
| Mutable? | No (read-only in child) | Yes (using setState or useState) |
| Who controls? | Parent component | The component itself |
| Example | username passed to ProfileCard | form input value, toggle status |

```jsx
// Props example
function Button({ label, onClick }) {       // props received from parent
    return <button onClick={onClick}>{label}</button>;
}

// State example
function Counter() {
    const [count, setCount] = useState(0);  // state — managed internally
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
    );
}
```

---

### useState
```jsx
import { useState } from 'react';

function LoginForm() {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [isLoading, setIsLoading] = useState(false);

    const handleSubmit = async () => {
        setIsLoading(true);
        // API call
        setIsLoading(false);
    };

    return (
        <form>
            <input value={email} onChange={(e) => setEmail(e.target.value)} />
            <input value={password} onChange={(e) => setPassword(e.target.value)} />
            <button disabled={isLoading} onClick={handleSubmit}>
                {isLoading ? 'Logging in...' : 'Login'}
            </button>
        </form>
    );
}
```

---

### useEffect
**Purpose:** Runs side effects — API calls, subscriptions, DOM manipulation — after component renders.

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
    const [user, setUser] = useState(null);

    // Runs after every render (no dependency array)
    useEffect(() => {
        console.log('Component rendered');
    });

    // Runs only ONCE on mount (empty dependency array)
    useEffect(() => {
        console.log('Component mounted');
        return () => console.log('Component unmounted'); // cleanup
    }, []);

    // Runs when userId changes
    useEffect(() => {
        fetch(`/api/users/${userId}`)
            .then(res => res.json())
            .then(data => setUser(data));
    }, [userId]); // dependency array

    return <div>{user ? user.name : 'Loading...'}</div>;
}
```

---

### Virtual DOM
**What is the real DOM?** The actual HTML tree that the browser renders. Manipulating it is slow.

**What is Virtual DOM?**  
React keeps a lightweight JavaScript copy of the real DOM in memory.

**How it works:**
1. State changes → React creates new Virtual DOM tree
2. React **diffs** new virtual DOM vs old virtual DOM (reconciliation)
3. Only the **changed parts** are updated in the real DOM
4. This is much faster than re-rendering the entire DOM

```
State changes
    ↓
New Virtual DOM created
    ↓
Diff algorithm compares old vs new Virtual DOM
    ↓
Only the minimal changes applied to real DOM
    ↓
Browser re-paints only changed elements
```

---

### Component Lifecycle (with Hooks)

```
Mounting:
  → useState initializes state
  → Component renders (returns JSX)
  → DOM updates
  → useEffect with [] runs

Updating:
  → State/props change
  → Component re-renders
  → DOM updates
  → useEffect with [dependency] runs if dependency changed

Unmounting:
  → Cleanup function in useEffect runs
  → Component removed from DOM
```

---

## 📌 React Interview Questions & Answers

**Q: What is the Virtual DOM?**
> The Virtual DOM is React's lightweight in-memory representation of the real DOM. When state changes, React creates a new Virtual DOM tree and compares it with the previous one using a diffing algorithm. Only the actual differences are then applied to the real DOM, making updates faster than rewriting the entire DOM tree.

**Q: What is useEffect? Give an example.**
> `useEffect` is a hook for performing side effects in functional components — things like fetching data, subscriptions, or manually changing the DOM. It runs after the component renders. The dependency array controls when it runs: empty array `[]` means run once on mount, and `[value]` means run whenever `value` changes.

**Q: Difference between state and props?**
> Props are read-only data passed from a parent component to a child — the child cannot modify them. State is mutable data managed within the component itself — it can be updated using `useState`, and changes trigger a re-render. Think of props as function arguments and state as local variables.

---

# 4. Backend & REST API Concepts

## 🔷 4.1 REST API

**REST (Representational State Transfer)** is an architectural style for designing networked applications using HTTP.

### HTTP Methods
```
GET    /users         → Get all users
GET    /users/1       → Get user with id 1
POST   /users         → Create new user
PUT    /users/1       → Update user 1 (full update)
PATCH  /users/1       → Partial update of user 1
DELETE /users/1       → Delete user 1
```

### HTTP Status Codes
```
2xx Success:
  200 OK              — Request succeeded
  201 Created         — Resource created
  204 No Content      — Success, nothing to return (DELETE)

4xx Client Errors:
  400 Bad Request     — Invalid request data
  401 Unauthorized    — Not authenticated
  403 Forbidden       — Authenticated but not authorized
  404 Not Found       — Resource doesn't exist
  422 Unprocessable   — Validation failed

5xx Server Errors:
  500 Internal Error  — Server crashed
  503 Service Unavail — Server overloaded/down
```

---

## 🔷 4.2 Authentication vs Authorization

| Feature | Authentication | Authorization |
|---|---|---|
| Question | "Who are you?" | "What can you do?" |
| Verifies | Identity | Permissions |
| Example | Login with password | Admin can delete; user can only read |
| Technologies | JWT, Sessions, OAuth | RBAC, ACL |

### JWT (JSON Web Token)
```
JWT = Header.Payload.Signature

Header: { "alg": "HS256", "typ": "JWT" }
Payload: { "userId": 123, "role": "admin", "exp": 1700000000 }
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

**How JWT auth works:**
1. User logs in with username/password
2. Server validates credentials, creates JWT, sends to client
3. Client stores JWT (localStorage or cookie)
4. Client sends JWT in every request: `Authorization: Bearer <token>`
5. Server validates JWT signature on every request — no DB lookup needed!

---

## 🔷 4.3 Rate Limiting

**What is rate limiting?** Restricting the number of requests a client can make in a given time window.

**Why?** Prevent API abuse, DDoS attacks, and server overload.

**Example:** GitHub API allows 5000 requests/hour per authenticated user.

**Implementation approaches:**
- **Fixed Window:** Count resets every minute
- **Sliding Window:** Counts requests in rolling time window
- **Token Bucket:** Tokens refilled at a rate; each request consumes a token

**Common headers in response:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 47
X-RateLimit-Reset: 1700000060
```

---

## 🔷 4.4 How to Scale an API

**Vertical Scaling:** Add more power to existing server (more CPU, RAM). Simple but has limits.

**Horizontal Scaling:** Add more servers. Distribute load using a **Load Balancer**.

```
Clients
   ↓
Load Balancer (Nginx, AWS ALB)
   ↓          ↓          ↓
Server 1   Server 2   Server 3
   ↓
Database (shared or replicated)
```

**Other scaling strategies:**
- **Caching:** Store frequent responses in Redis to avoid DB hits
- **Database read replicas:** Write to master, read from replicas
- **CDN:** Serve static assets from edge servers close to users
- **Message queues:** Decouple slow tasks (emails, notifications) using queues like RabbitMQ/Kafka
- **Microservices:** Split monolith into independent services

---

## 📌 Backend Interview Questions & Answers

**Q: What is REST API?**
> REST is an architectural style for web APIs that uses HTTP methods (GET, POST, PUT, DELETE) to perform operations on resources. Each resource has a unique URL. REST APIs are stateless — every request contains all information needed to process it. They're widely used because they're simple, scalable, and work well with any client (browser, mobile, server).

**Q: What is rate limiting?**
> Rate limiting restricts how many API requests a client can make in a given time period. It prevents API abuse, DDoS attacks, and ensures fair usage. For example, allowing 100 requests per minute per user. When exceeded, the server returns a 429 (Too Many Requests) status code.

**Q: How would you scale an API?**
> I'd start with caching frequently accessed data using Redis to reduce database load. Then add a load balancer to distribute traffic across multiple server instances. For the database, I'd set up read replicas to handle read-heavy traffic. For long-running tasks like emails, I'd use a message queue like RabbitMQ to process asynchronously. As the app grows further, I'd consider breaking it into microservices.

**Q: Authentication vs Authorization?**
> Authentication is verifying identity — "Are you who you say you are?" like a username/password login. Authorization is verifying permissions — "Are you allowed to do this?" like checking if a logged-in user has admin privileges. In my TicketWise project, I used JWT for authentication and role-based access control (RBAC) for authorization — regular users could view events, while admins could create and manage them.

---

# 5. AI Integration & Gemini API

## 🔷 5.1 What is an LLM API?

**LLM (Large Language Model):** An AI model trained on massive text data that can understand and generate human-like text.

**LLM API:** A REST API that lets you send text prompts and receive AI-generated responses.

```javascript
// Typical LLM API call (Gemini)
const response = await fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-Goog-Api-Key': API_KEY
    },
    body: JSON.stringify({
        contents: [{
            parts: [{ text: "Summarize this event description: " + eventText }]
        }]
    })
});

const data = await response.json();
const aiResponse = data.candidates[0].content.parts[0].text;
```

---

## 🔷 5.2 Prompt Engineering

**What is prompt engineering?** The practice of designing effective inputs (prompts) to get desired outputs from an AI model.

**Techniques:**

**1. Clear Instructions**
```
BAD:  "Write something about the event"
GOOD: "Write a 3-sentence professional summary of this event for a ticketing website. 
       Focus on key details: date, venue, and what attendees can expect."
```

**2. Role Assignment**
```
"You are an expert event copywriter. Generate engaging descriptions for events..."
```

**3. Few-shot Examples**
```
"Here are examples of good event descriptions:
 Example 1: [input] → [output]
 Example 2: [input] → [output]
 Now generate for: [new input]"
```

**4. Output Format Control**
```
"Respond ONLY with a JSON object in this format:
 { 'summary': '...', 'tags': ['...'], 'category': '...' }"
```

---

## 🔷 5.3 How to Talk About Your Gemini Integration

**For TicketWise, structure your answer:**

> "In TicketWise, I integrated the Gemini API to automatically generate event descriptions and summaries. When an organizer creates an event, they provide basic details like title, date, and category. I send this to the Gemini API with a carefully crafted prompt asking it to generate a professional event description. I also used it to auto-generate relevant tags for events, which improved discoverability.
>
> The challenge I faced was managing API rate limits — I implemented a queue system so requests were spaced out. I also added error handling and fallbacks in case the API was unavailable, so organizers could still create events with manual descriptions.
>
> This feature significantly reduced the time organizers spent on descriptions and improved the overall quality of event listings."

---

## 📌 AI Interview Questions & Answers

**Q: How did you use the Gemini API in your project?**
> In TicketWise, I used Gemini API for automatic event description generation. Organizers provide basic event details, and I send those to Gemini with a structured prompt to generate professional descriptions and relevant tags. I handled rate limiting, implemented retry logic, and added graceful fallbacks.

**Q: What is prompt engineering?**
> Prompt engineering is designing effective inputs for AI models to get desired outputs. It involves being specific about the task, providing context, specifying output format, and sometimes giving examples. For instance, in TicketWise I crafted prompts that specified the tone (professional), length (2-3 sentences), and format (JSON) for generated descriptions.

**Q: What challenges did you face integrating an AI API?**
> Three main challenges: (1) Rate limits — I added queuing to space out requests. (2) Response consistency — AI responses aren't always structured the same way, so I used explicit format instructions and JSON parsing with error handling. (3) Latency — AI API calls take 1-3 seconds, so I ran them asynchronously and showed the user a loading state.

---

# 6. Cloud Basics & Docker

## 🔷 6.1 What is Cloud Computing?

Cloud computing is the delivery of computing services — servers, storage, databases, networking, software — over the internet.

**Instead of:** Buying and maintaining physical servers  
**You:** Rent computing resources from cloud providers and pay only for what you use.

**Service Models:**
```
IaaS (Infrastructure as a Service): Virtual machines, storage (EC2, Azure VMs)
PaaS (Platform as a Service): Runtime, DB, deployment platform (Heroku, App Engine)
SaaS (Software as a Service): Ready-to-use apps (Gmail, Salesforce, Dropbox)
```

**Deployment Models:**
- **Public Cloud:** Resources shared with others (AWS, GCP, Azure)
- **Private Cloud:** Dedicated resources for one org
- **Hybrid Cloud:** Mix of both

---

## 🔷 6.2 AWS vs GCP Basics

| Service Type | AWS | GCP |
|---|---|---|
| Compute (VMs) | EC2 | Compute Engine |
| Serverless | Lambda | Cloud Functions |
| Object Storage | S3 | Cloud Storage |
| Managed Database | RDS | Cloud SQL |
| Container orchestration | EKS (Kubernetes) | GKE |
| AI/ML | SageMaker | Vertex AI |
| CDN | CloudFront | Cloud CDN |

---

## 🔷 6.3 Key Cloud Concepts

### Scalability
**Vertical scaling:** Make the server bigger (more CPU/RAM)  
**Horizontal scaling:** Add more servers

### Load Balancing
Distributes incoming traffic across multiple servers so no single server is overwhelmed.

### Auto-scaling
Cloud automatically adds/removes servers based on traffic demand.
- High traffic at 9am → Scales up to 10 servers
- Low traffic at 3am → Scales down to 2 servers
- Pay only for what you use!

### Why Cloud?
1. **No upfront hardware cost** — pay-as-you-go
2. **Global reach** — deploy in any region worldwide
3. **High availability** — 99.99% uptime SLAs
4. **Scalability** — handle any traffic spike
5. **Managed services** — cloud handles maintenance, security patches

---

## 🔷 6.4 Docker Basics

**The Problem Docker Solves:**
> "It works on my machine but not in production!"

Different environments (dev laptop vs server) have different OS, library versions, configurations.

**Docker Solution:** Package your app + all its dependencies into a **container** that runs identically everywhere.

### Key Concepts

```
Image: Blueprint/template for a container (like a class in OOP)
Container: Running instance of an image (like an object)
Dockerfile: Instructions to build an image
Docker Hub: Registry to store and share images
```

### Simple Dockerfile
```dockerfile
# Start from Node.js image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json .
RUN npm install

# Copy source code
COPY . .

# Expose port
EXPOSE 3000

# Start command
CMD ["node", "server.js"]
```

### Basic Commands
```bash
docker build -t my-app .          # Build image from Dockerfile
docker run -p 3000:3000 my-app    # Run container, map port 3000
docker ps                          # List running containers
docker stop <container-id>         # Stop container
docker images                      # List all images
```

### Container vs VM

| | Container | Virtual Machine |
|---|---|---|
| Includes | App + libraries + runtime | Full OS + app + libraries |
| Size | Megabytes | Gigabytes |
| Startup | Seconds | Minutes |
| Isolation | Process-level | Full OS-level |
| Overhead | Low | High |

---

## 📌 Cloud Interview Questions & Answers

**Q: What is cloud computing?**
> Cloud computing is delivering computing services — servers, storage, databases, networking — over the internet on a pay-as-you-go basis. Instead of buying physical servers, you rent resources from providers like AWS or GCP. The main benefits are cost efficiency (no upfront hardware), scalability (handle any traffic), global reach, and high availability.

**Q: What is Docker?**
> Docker is a containerization platform that packages an application and all its dependencies into a standardized unit called a container. This ensures the app runs consistently across different environments — developer laptops, staging servers, production. Containers are lightweight (share the host OS kernel) and start in seconds, unlike VMs that include a full OS.

**Q: What is the difference between scaling vertically and horizontally?**
> Vertical scaling means upgrading the existing server — adding more CPU, RAM, or storage to a single machine. It's simpler but has physical limits. Horizontal scaling means adding more servers and distributing traffic across them using a load balancer. Cloud platforms like AWS support both, but horizontal scaling is preferred for production systems as it's more resilient — if one server fails, others continue serving traffic.

---

# 7. Quarkus & Kotlin Awareness

## 🔷 7.1 Quarkus

**What is Quarkus?**  
Quarkus is a modern Java framework designed for cloud-native applications and microservices. It's optimized for containers.

**Why Quarkus over Spring Boot?**
| Feature | Quarkus | Spring Boot |
|---|---|---|
| Startup time | Very fast (milliseconds) | Slower (seconds) |
| Memory usage | Very low | Higher |
| Cloud-native | Designed for it | Adapted for it |
| GraalVM native | First-class support | Supported but complex |
| Dev experience | Hot reload, fast | Good but slower |

**Key features of Quarkus:**
1. **Fast startup:** Perfect for serverless and containers
2. **Low memory footprint:** Cheaper to run in cloud
3. **Native compilation:** Can compile to native binary (no JVM needed)
4. **Cloud-native:** Kubernetes and Docker integration built-in
5. **Developer Joy:** Live reload in dev mode

**Simple Quarkus REST endpoint:**
```java
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class GreetingResource {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello from Quarkus!";
    }
}
```

---

## 🔷 7.2 Kotlin

**What is Kotlin?**  
Kotlin is a modern, statically-typed programming language that runs on the JVM. Created by JetBrains (makers of IntelliJ IDEA). Adopted by Google as preferred language for Android.

**Why Kotlin over Java?**

```kotlin
// Java — verbose
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }
    // equals, hashCode, toString... many more lines
}

// Kotlin — concise
data class Person(val name: String, val age: Int)
// That's it! Kotlin auto-generates everything
```

**Key Kotlin Features:**

**1. Null Safety**
```kotlin
// Java — NullPointerException is common
String name = null;
name.length(); // CRASH! NullPointerException

// Kotlin — null safety built into type system
var name: String = "Alice"  // cannot be null
var nickname: String? = null // ? means nullable

nickname?.length  // safe call — returns null instead of crashing
nickname ?: "Unknown"  // Elvis operator — default value if null
```

**2. Data Classes**
```kotlin
data class Event(val title: String, val date: String, val price: Double)
// Auto-generates: equals(), hashCode(), toString(), copy()
```

**3. Extension Functions**
```kotlin
// Add methods to existing classes without modifying them
fun String.isEmail(): Boolean {
    return this.contains("@") && this.contains(".")
}

// Usage
"alice@example.com".isEmail() // true
```

**4. Coroutines (async)**
```kotlin
// Clean async code — no callback hell
suspend fun fetchUser(id: Int): User {
    return apiService.getUser(id) // non-blocking
}
```

**Interoperability:** Kotlin and Java can be used together in the same project. You can call Java code from Kotlin and vice versa.

---

## 📌 Quarkus & Kotlin Interview Questions

**Q: What is Quarkus and why would you use it?**
> Quarkus is a Java framework optimized for cloud-native, containerized environments. It offers very fast startup times (milliseconds vs seconds for Spring Boot) and extremely low memory footprint, making it cost-efficient in cloud deployments. It supports native compilation through GraalVM, which is ideal for serverless functions and microservices that need to start instantly.

**Q: Why is Kotlin preferred over Java in modern backend development?**
> Kotlin is more concise — what takes 50 lines in Java might take 10 in Kotlin. It has built-in null safety that prevents the infamous NullPointerException at compile time. Data classes auto-generate boilerplate like `equals`, `hashCode`, and `toString`. It's fully interoperable with Java, so existing Java libraries work seamlessly. Coroutines provide clean async programming. These features improve developer productivity and code quality.

---

# 8. Project Explanations

## 🔷 TicketWise — Full Explanation

**What is it?**  
An event ticketing platform where organizers create events and users purchase tickets.

**Tech Stack:**
- Backend: Node.js + Express
- Database: PostgreSQL / MongoDB
- Auth: JWT (JSON Web Tokens)
- AI: Gemini API
- Payments: (if applicable)

**STAR Format Explanation:**

**Situation:** I wanted to build a real-world application that solved the problem of fragmented event ticketing — organizers struggling to manage events and users having inconsistent ticket booking experiences.

**Task:** Design and build a full-stack ticketing platform with proper authentication, role-based access, event management, and AI-powered features.

**Action:**
- Designed REST APIs for user auth, event CRUD, ticket management
- Implemented JWT authentication and role-based access control (admin/organizer/attendee roles)
- Integrated Gemini API for automatic event description generation
- Built background job processing for ticket confirmation emails
- Designed relational database schema with proper relationships and indexes

**Result:** A fully functional ticketing platform with AI-powered descriptions, secure authentication, and scalable backend architecture.

---

**Key Technical Decisions to Mention:**

**1. Why JWT for auth?**
> JWT is stateless — the server doesn't need to store sessions. Each token contains user info and is cryptographically signed. This makes it easily scalable across multiple servers since any server can validate the token without a DB lookup.

**2. Why role-based access control?**
> Different user types need different permissions. Organizers should only manage their own events; admins can manage everything; attendees can only view and book. RBAC cleanly separates these concerns using middleware that checks roles on each API endpoint.

**3. Background processing for emails:**
> Sending confirmation emails synchronously would slow down the ticket booking response. By using a job queue (like Bull or similar), the API responds immediately to the user while the email is sent asynchronously in the background.

---

## 🔷 Vrikshami — Full Explanation

**What is it?**  
A platform for plant nurseries or a tree/plant-related e-commerce/community platform.

**Tech Stack:**
- Node.js REST APIs
- Razorpay payment integration
- Database design

**Key Points to Mention:**

**Razorpay Integration:**
> "I integrated Razorpay for handling payments. The flow was: (1) User selects items and checks out, (2) Backend creates a Razorpay order with the amount, (3) Frontend uses Razorpay checkout widget to collect card/UPI details, (4) On success, Razorpay sends a payment ID, (5) Backend verifies the payment signature using Razorpay's SDK to confirm it's authentic, (6) Only then the order is marked as paid in our database. This prevents fake payment confirmations."

**Database Design:**
> "I designed normalized tables for users, products, categories, orders, and order_items. Used foreign keys to maintain referential integrity. Added indexes on frequently queried columns like product category and user_id in orders for faster queries."

---

# 9. HR Questions & Answers

## "Tell me about yourself"

> "I'm a final-year Electronics and Telecommunications Engineering student at [College]. During my studies, I discovered a strong passion for backend development and building scalable systems.
>
> I've built two significant full-stack projects: TicketWise, an event ticketing platform where I implemented JWT authentication, role-based access control, and Gemini AI integration; and Vrikshami, where I built REST APIs with Razorpay payment integration.
>
> I'm proficient in Node.js, SQL, and React, and I've been actively strengthening my Java skills as well. I've participated in hackathons and NSS, which have sharpened my problem-solving and communication skills.
>
> I'm excited about this role because it aligns perfectly with my backend development experience and gives me the opportunity to work with modern technologies like Quarkus and Kotlin."

---

## "Why should we hire you?"

> "Three reasons: First, my project experience — I've built production-quality applications with JWT auth, role-based access, payment integration, and AI APIs. These aren't tutorial projects; they involved real architectural decisions.
>
> Second, my AI integration experience — I've worked hands-on with LLM APIs like Gemini, which is increasingly valuable. I understand prompt engineering, API rate limiting, and handling non-deterministic responses.
>
> Third, I'm a fast learner who adapts quickly. When I encountered challenges like integrating Razorpay or designing database schemas, I figured them out. While I'm building my Java expertise, my strong foundation in backend development, REST APIs, and system design transfers directly to any technology stack."

---

## "Why software after ENTC engineering?"

> "My ENTC background actually strengthened my software journey. Electronics gave me low-level thinking — understanding how systems work at a fundamental level, which helps in writing efficient code.
>
> But during projects and coursework, I found myself most engaged when solving problems through software. I started building applications — first small scripts, then full-stack projects. When I saw my TicketWise platform actually work end-to-end, with real users booking tickets through an AI-powered system, I knew software engineering was my path.
>
> ENTC taught me analytical thinking, systems thinking, and problem decomposition — skills that make me a better software engineer than someone who only studied CS theory."

---

## "Where do you see yourself in 5 years?"

> "In the next 2-3 years, I want to become proficient in enterprise Java development, cloud architecture, and system design — becoming the kind of engineer who can design reliable, scalable backend systems.
>
> In 5 years, I'd like to be leading technical decisions on a team — not just writing code but mentoring juniors, doing architecture reviews, and contributing to the engineering culture. I'm also interested in the intersection of AI and backend systems, which feels like where the industry is heading."

---

## "What are your weaknesses?"

> "My biggest weakness is perfectionism in system design — I sometimes spend too long thinking about the ideal architecture before building. I've learned to balance this by timebox my design phase and getting to working code faster, then refining as I learn more about the actual requirements.
>
> Another weakness is deep Java expertise — I have solid fundamentals, but I haven't built large production Java systems yet. That said, I've been actively working on this and am confident I can ramp up quickly."

---

## "Tell me about a challenge you faced in your project"

> "In TicketWise, the most challenging problem was handling concurrent ticket bookings for limited-capacity events. If 100 users simultaneously click 'Book' for the last 5 tickets, a naive implementation could oversell.
>
> I solved this using database-level transactions with SELECT FOR UPDATE — when a booking attempt starts, it locks the ticket record, checks availability, and either confirms or rejects the booking atomically. This prevented race conditions without hurting performance for normal load.
>
> It taught me that building software for concurrent users requires thinking beyond just 'happy path' scenarios."

---

# 10. Quick Revision Cheat Sheet

## ✅ Java One-liners

| Concept | Key Point |
|---|---|
| OOP Pillars | Encapsulation, Inheritance, Polymorphism, Abstraction |
| Encapsulation | Private data + public getters/setters |
| Inheritance | `extends` keyword; single inheritance via classes |
| Polymorphism | Overloading = compile-time; Overriding = runtime |
| Abstraction | Hide details; abstract class + interfaces |
| Interface vs Abstract | Interface = contract, multiple; Abstract = partial impl, single |
| ArrayList vs LinkedList | ArrayList = fast read O(1); LinkedList = fast insert O(1) |
| HashMap | Uses hash + equals; O(1) average; handles collision via chaining |
| String vs StringBuilder | String = immutable; StringBuilder = mutable, faster in loops |
| == vs equals() | == checks reference; equals() checks value |
| Heap vs Stack | Heap = objects; Stack = method calls + local vars |
| Checked Exception | Must handle at compile time: IOException |
| Unchecked Exception | Runtime: NullPointerException, ArrayIndexOutOfBounds |
| JVM | Executes bytecode; platform-independent |
| Thread | start() creates new thread; run() doesn't |

---

## ✅ SQL One-liners

| Concept | Key Point |
|---|---|
| INNER JOIN | Only matching rows from both tables |
| LEFT JOIN | All from left, matching from right |
| GROUP BY | Group rows; use with aggregates |
| HAVING | Filter after GROUP BY (like WHERE for groups) |
| WHERE vs HAVING | WHERE = before grouping; HAVING = after grouping |
| DELETE vs TRUNCATE | DELETE = specific rows, logged; TRUNCATE = all rows, fast |
| Index | B-tree structure; speeds SELECT; slows INSERT/UPDATE |
| Primary Key | Unique, NOT NULL identifier |
| Foreign Key | References primary key of another table |
| 2nd highest salary | `SELECT MAX(salary) WHERE salary < (SELECT MAX...)` |

---

## ✅ React One-liners

| Concept | Key Point |
|---|---|
| Props | Read-only data passed from parent to child |
| State | Mutable data managed within component; useState |
| useEffect | Side effects after render; dependency array controls when |
| Virtual DOM | JS copy of DOM; diff + minimal real DOM updates |
| Re-render trigger | State change or props change |

---

## ✅ Backend One-liners

| Concept | Key Point |
|---|---|
| REST | Stateless HTTP-based API; uses GET/POST/PUT/DELETE |
| 200 | OK |
| 201 | Created |
| 401 | Unauthorized (not logged in) |
| 403 | Forbidden (logged in but no permission) |
| 404 | Not Found |
| JWT | Stateless auth token; Header.Payload.Signature |
| Authentication | "Who are you?" |
| Authorization | "What can you do?" |
| Rate limiting | Limit requests per time window; 429 status when exceeded |

---

## ✅ Cloud/Docker One-liners

| Concept | Key Point |
|---|---|
| Cloud | Rent computing over internet; pay-as-you-go |
| IaaS | VMs (EC2) |
| PaaS | Platform (Heroku) |
| SaaS | Apps (Gmail) |
| Container | App + deps in isolated unit; runs identically anywhere |
| Docker | Containerization platform |
| Dockerfile | Instructions to build image |
| Image | Blueprint; Container = running instance |
| Container vs VM | Container = lightweight, seconds startup; VM = full OS |
| Horizontal scaling | More servers + load balancer |
| Vertical scaling | Bigger server |

---

## ✅ Last Minute Mindset Checklist

- [ ] I know OOP with real examples (Payment, Vehicle, Notification systems)
- [ ] I can write a SQL JOIN query from memory
- [ ] I can explain how HashMap works internally
- [ ] I can explain my TicketWise project confidently in 3 minutes
- [ ] I know the difference between Authentication and Authorization
- [ ] I can explain Virtual DOM to a non-technical person
- [ ] I know what Docker is and why it's used
- [ ] I have an answer ready for "Why software after ENTC?"
- [ ] I can explain what Quarkus and Kotlin are (awareness level)
- [ ] I've prepared specific examples for every HR question

---

## 🎯 Final Tips

1. **STAR Method for project questions:** Situation → Task → Action → Result
2. **When you don't know something:** "I haven't worked with that specifically, but based on my understanding of [related concept], I'd approach it by..." — never just say "I don't know."
3. **Ask a clarifying question** if you're unsure what they're asking — it shows analytical thinking.
4. **Think aloud** during problem-solving — they want to see your reasoning process.
5. **Connect your projects** to every technical answer — it makes abstract concepts concrete.
6. **Be confident about your backend experience** — TicketWise and Vrikshami are legitimately strong fresher projects.

---

> 💪 **You've got this. Your projects are stronger than most freshers. Convert that experience into confident, specific answers.**
