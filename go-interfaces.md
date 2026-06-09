# Go Interfaces

Interfaces describe behavior. A type implements an interface automatically by providing the required methods.

---

## 1. What is an interface?

### Definition
An interface is a set of method signatures. It defines what a type can do, not what it is.

### Analogy
An interface is like a job role description:
- It says what skills are required.
- Any person who has those skills can fill the role.

### Example idea
If something can `Speak()`, then it fits a `Speaker` interface.

---

## 2. Syntax to declare an interface

```go
type Speaker interface {
    Speak() string
}
```

### Meaning
Any type that has a `Speak() string` method satisfies `Speaker`.

---

## 3. Syntax to implement an interface

You do not write `implements` in Go. A type implements an interface automatically.

```go
type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return "Woof"
}
```

Now `Dog` satisfies `Speaker`.

---

## 4. Using an interface

```go
func PrintSpeech(s Speaker) {
    fmt.Println(s.Speak())
}

dog := Dog{Name: "Tommy"}
PrintSpeech(dog)
```

### Why use it?
It lets you write code that works with many types, as long as they support the same behavior.

---

## 5. Empty interface

### Syntax
```go
var x interface{}
```

### Meaning
An empty interface can hold any value.

### Example
```go
var data interface{}
data = 10
data = "hello"
data = true
```

### Note
In modern Go, use `any` for the same meaning.

```go
var data any
```

---

## 6. Type assertion

### Definition
Type assertion is used to extract the real value from an interface.

### Syntax
```go
value := x.(string)
```

### Safe form
```go
value, ok := x.(string)
if ok {
    fmt.Println(value)
}
```

### Example
```go
var data any = "Go"

s, ok := data.(string)
if ok {
    fmt.Println("String:", s)
}
```

---

## 7. Type switch

### Definition
A type switch checks the concrete type stored inside an interface.

### Syntax
```go
switch v := x.(type) {
case string:
    fmt.Println("string", v)
case int:
    fmt.Println("int", v)
default:
    fmt.Println("unknown")
}
```

### Example
```go
func Describe(i any) {
    switch v := i.(type) {
    case string:
        fmt.Println("string:", v)
    case int:
        fmt.Println("int:", v)
    }
}
```

---

## 8. Interface values

### Important idea
An interface value holds:
- the actual value,
- the type of that value.

### Analogy
Like a parcel with two labels:
- what is inside,
- what kind of item it is.

---

## 9. Pointer receiver vs value receiver

### Value receiver
```go
func (d Dog) Speak() string {
    return "Woof"
}
```

### Pointer receiver
```go
func (d *Dog) Rename(name string) {
    d.Name = name
}
```

### Rule of thumb
- Use value receiver for small, read-only methods.
- Use pointer receiver when you need to mutate the struct or avoid copying.

---

## 10. Common interface examples

### `error`
```go
type error interface {
    Error() string
}
```

Any type with `Error() string` can be used as an error.

### Example
```go
type MyError struct {
    Message string
}

func (e MyError) Error() string {
    return e.Message
}
```

---

## 11. Interface composition

### Syntax
```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

type ReadWriter interface {
    Reader
    Writer
}
```

### Meaning
Interfaces can be built from other interfaces.

---

## 12. Practical use
Interfaces are useful for:
- testing and mocking,
- clean architecture,
- plugging different implementations,
- writing reusable code.

---

## Mental checklist
- Use interfaces to describe behavior.
- Types satisfy interfaces automatically.
- Use type assertion when you need the concrete type.
- Use `error` as an interface for failures.
- Keep interfaces small and focused.
