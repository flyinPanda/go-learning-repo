# Microservices in Go

A microservice is a small, independent service that does one job well. Go is popular for microservices because it is fast, simple, and good at concurrency.

---

## 1. What is a microservice?

### Definition
A microservice is a small application that handles one business capability, such as:
- user service,
- payment service,
- order service,
- notification service.

### Analogy
A restaurant kitchen:
- one station handles salads,
- one handles grilling,
- one handles desserts.

Each station does one job.

---

## 2. Why Go for microservices?

### Benefits
- Fast startup.
- Low memory usage.
- Strong concurrency support.
- Easy deployment as a single binary.
- Simple standard library for HTTP and JSON.

---

## 3. Basic microservice structure

### Typical layers
- handler / controller,
- service,
- repository / storage,
- model.

### Example folder structure
```text
myservice/
├── go.mod
├── main.go
├── handler/
├── service/
├── repository/
├── model/
└── config/
```

---

## 4. Basic HTTP service

### Example
```go
package main

import (
    "fmt"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello from Go microservice")
}

func main() {
    http.HandleFunc("/hello", helloHandler)
    http.ListenAndServe(":8080", nil)
}
```

---

## 5. JSON API example

### Example
```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

func userHandler(w http.ResponseWriter, r *http.Request) {
    user := User{ID: 1, Name: "Rahul"}
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(user)
}
```

---

## 6. Request handling

### Example
```go
func createUser(w http.ResponseWriter, r *http.Request) {
    var user User
    err := json.NewDecoder(r.Body).Decode(&user)
    if err != nil {
        http.Error(w, "invalid request", http.StatusBadRequest)
        return
    }

    w.WriteHeader(http.StatusCreated)
    json.NewEncoder(w).Encode(user)
}
```

---

## 7. Common service concepts

### Health check
A route that tells whether the service is alive.

```go
func healthHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "ok")
}
```

### Configuration
Load values like:
- port,
- database URL,
- secret keys.

### Logging
Record requests, errors, and failures.

### Validation
Check input before processing.

---

## 8. Concurrency in microservices

Go microservices often use goroutines for:
- background jobs,
- concurrent requests,
- fan-out to multiple services,
- queue workers.

### Example
```go
go func() {
    // background task
}()
```

---

## 9. Database access

### Common pattern
```go
type UserRepository interface {
    Create(user User) error
    FindByID(id int) (User, error)
}
```

### Why use interfaces?
To separate business logic from storage implementation.

---

## 10. Error handling in services

### Example
```go
if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
}
```

### Best practice
Return proper HTTP status codes:
- 200 OK,
- 201 Created,
- 400 Bad Request,
- 404 Not Found,
- 500 Internal Server Error.

---

## 11. Typical production concerns
- Authentication and authorization.
- Timeouts and cancellation.
- Logging and tracing.
- Configuration management.
- Graceful shutdown.
- Database connection pooling.
- Metrics and health checks.

---

## 12. Graceful shutdown

### Idea
Stop accepting new requests, finish active ones, then exit cleanly.

### Pattern
Use `context`, `http.Server`, and OS signal handling.

---

## 13. Design advice
- Keep each service small and focused.
- Use clear boundaries.
- Prefer simple HTTP + JSON first.
- Add messaging or gRPC only when needed.
- Don’t overcomplicate early.

---

## Mental checklist
- A microservice should do one business job.
- Use HTTP handlers for requests.
- Use JSON for API payloads.
- Use interfaces for clean separation.
- Use goroutines for background work.
- Plan for logging, errors, health checks, and shutdown.
