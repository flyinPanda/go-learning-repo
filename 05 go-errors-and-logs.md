# Go Errors and Logging

Errors are Go’s normal way of handling problems. Logging helps you record what happened so you can debug later.

---

## 1. What is an error?

### Definition
An error is a value that describes a failure.

### Analogy
Like a red warning slip attached to a result.

### Syntax
```go
var err error
```

### Common pattern
```go
result, err := someFunction()
if err != nil {
    // handle error
}
```

---

## 2. Creating errors

### Syntax
```go
errors.New("message")
```

### Example
```go
err := errors.New("invalid input")
fmt.Println(err)
```

---

## 3. Returning errors

### Example
```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}
```

### Use
```go
val, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
}
```

---

## 4. Error handling style

### Standard pattern
```go
if err != nil {
    return err
}
```

### Example
```go
file, err := os.Open("data.txt")
if err != nil {
    return err
}
defer file.Close()
```

---

## 5. Wrapping errors

### Definition
Wrapping adds context while keeping the original error.

### Syntax
```go
fmt.Errorf("loading config: %w", err)
```

### Example
```go
if err != nil {
    return fmt.Errorf("failed to read file: %w", err)
}
```

### Why use it?
It tells you both:
- what failed,
- where it failed.

---

## 6. Checking error types

### Example
```go
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("file not found")
}
```

### Another example
```go
var pathErr *os.PathError
if errors.As(err, &pathErr) {
    fmt.Println("path error:", pathErr)
}
```

---

## 7. Panic and recover

### Panic
`panic` stops normal execution immediately.

```go
panic("something went terribly wrong")
```

### Recover
`recover` can catch a panic inside a deferred function.

```go
defer func() {
    if r := recover(); r != nil {
        fmt.Println("recovered:", r)
    }
}()
```

### Rule
Use `panic` only for truly unrecoverable situations or programmer mistakes.

---

## 8. Logging basics

### Definition
Logging is writing messages about program behavior.

### Analogy
A flight recorder for your program.

### Simple logging
```go
log.Println("server started")
log.Printf("user id=%d", id)
log.Fatal("fatal error")
```

### Note
`log.Fatal` prints the message and exits the program.

---

## 9. Log levels

Many logging systems use levels:
- debug,
- info,
- warning,
- error,
- fatal.

### Example idea
- `debug` for development detail,
- `info` for normal events,
- `error` for problems,
- `fatal` for unrecoverable problems.

---

## 10. Structured logging

### Definition
Structured logs store fields like key-value pairs.

### Why useful?
They are easier to filter, search, and analyze.

### Example style
```go
log.Printf("user=%s action=%s", user, action)
```

---

## 11. Best practices
- Return errors instead of hiding them.
- Add context when wrapping errors.
- Handle errors close to where they happen.
- Log enough information to debug, but not sensitive data.
- Avoid using panic for normal application errors.

---

## Mental checklist
- Use `error` for normal failures.
- Use `errors.New` and `fmt.Errorf`.
- Use `%w` to wrap errors.
- Use `log` for messages.
- Use panic only rarely.
