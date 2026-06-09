# Go Getting Started, History, Variables & Data Types

This note is a quick reference for getting started with Go, understanding where it came from, why people use it, and how variables and data types work.

---

## 1. What is Go?

### Definition
Go, also called Golang, is an open-source programming language designed at Google for simplicity, speed, and reliability.

### Analogy
Go is like a clean workshop:
- fewer tools,
- fewer hidden tricks,
- easier to keep organized,
- fast to build with.

### Why learn it?
Go is especially popular for:
- backend services,
- microservices,
- cloud tools,
- networking tools,
- concurrent programs.

---

## 2. Short history of Go

### Background
Go was created at Google by Robert Griesemer, Rob Pike, and Ken Thompson. It was designed to make software development simpler while still being fast and scalable.

### Timeline idea
- Designed in the late 2000s.
- Publicly announced in 2009.
- Became widely used for cloud and backend systems over time.

### Why it was created
Go was built to solve problems common in large codebases:
- long compile times,
- complicated dependency management,
- difficult concurrency,
- messy code organization.

### Gotcha
Go is not just “another C-like language.” Its design choices are intentionally different:
- no class inheritance,
- no exceptions in the usual OOP style,
- simple composition over inheritance,
- concurrency is built in.

---

## 3. Benefits of Go

### 1. Simple syntax
Go is easy to read and write compared to many complex languages.

### 2. Fast compilation
Programs compile quickly, which helps development flow.

### 3. Great concurrency support
Goroutines and channels make concurrent programming easier.

### 4. Strong standard library
You can build a lot using built-in packages without extra dependencies.

### 5. Easy deployment
Go programs usually compile into a single binary.

### 6. Good for networking and servers
It is widely used for APIs, services, tooling, and infrastructure software.

### 7. Strong performance
It is compiled and efficient, so it performs well for many backend workloads.

---

## 4. Disadvantages of Go

### 1. Less expressive than some languages
Go intentionally avoids many advanced language features.

### 2. Repetitive code
You may need to write more boilerplate in some cases.

### 3. Error handling can feel verbose
Because Go uses explicit error returns instead of exceptions.

### 4. No inheritance
Some developers miss traditional OOP inheritance patterns.

### 5. Generics came later
Go now supports generics, but older code and many examples still avoid them.

### 6. Ecosystem can feel opinionated
Go prefers “one clear way” to do things, which can feel limiting.

### Gotcha
A beginner often thinks “simple” means “easy immediately.” In Go, simple syntax still requires understanding:
- pointers,
- slices,
- interfaces,
- zero values,
- package organization.

---

## 5. First Go program

### File structure
A simple Go file usually starts with a package and imports.

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

### What each part means
- `package main` means this file builds an executable.
- `import "fmt"` brings in formatting/printing functions.
- `func main()` is the entry point.
- `fmt.Println()` prints output.

### Gotcha
The `main` function must be in the `main` package for executable programs.

---

## 6. How to run Go code

### Basic flow
1. Install Go.
2. Create a folder.
3. Initialize a module.
4. Write a `.go` file.
5. Run it.

### Commands
```bash
go version
go mod init example.com/myapp
go run main.go
go build
```

### Example
```bash
mkdir myapp
cd myapp
go mod init example.com/myapp
go run main.go
```

### Gotcha
If you forget `go mod init`, dependency management becomes awkward in modern Go.

---

## 7. Variables in Go

### Definition
A variable is a named storage location that holds a value.

### Analogy
A variable is like a labeled box:
- the label is the name,
- the contents are the value.

---

## 8. Declaring variables

### 1. Using `var` with type
```go
var age int
```

### 2. Using `var` with value
```go
var age int = 25
```

### 3. Type inference with `var`
```go
var name = "Rahul"
```

### 4. Short declaration with `:=`
```go
name := "Rahul"
age := 25
```

### Gotcha
`:=` works only inside functions, not at package level.

---

## 9. Multiple variable declaration

### Syntax
```go
var x, y int = 10, 20
```

### Type inference
```go
a, b := 1, 2
```

### Example
```go
var firstName, lastName string = "Rahul", "Anand"
```

### Gotcha
All variables in a multi-declaration must be handled carefully because their types must match what you assign.

---

## 10. Zero values

### Definition
If you declare a variable without assigning a value, Go gives it a default zero value.

### Examples
```go
var i int
var f float64
var b bool
var s string
```

### Zero values
- `int` → `0`
- `float64` → `0`
- `bool` → `false`
- `string` → `""`
- pointers, slices, maps, channels, functions, interfaces → `nil`

### Example
```go
var age int
fmt.Println(age) // 0
```

### Gotcha
Many beginners expect `null`. In Go, the default is usually the zero value, not a null-like placeholder.

---

## 11. Constants

### Definition
Constants are values that cannot change after declaration.

### Syntax
```go
const pi = 3.14
const appName string = "MyApp"
```

### Example
```go
const maxUsers = 100
```

### Gotcha
Constants must be assigned when declared.

---

## 12. Common data types

### Numeric types
```go
var a int = 10
var b float64 = 3.14
```

### Boolean
```go
var ok bool = true
```

### String
```go
var text string = "hello"
```

### Rune
```go
var ch rune = 'A'
```

### Byte
```go
var data byte = 255
```

### Gotcha
`rune` is an alias for `int32` and is used for Unicode code points.

---

## 13. Integers in Go

### Common integer types
- `int`
- `int8`
- `int16`
- `int32`
- `int64`

### Unsigned integer types
- `uint`
- `uint8`
- `uint16`
- `uint32`
- `uint64`

### Example
```go
var small int8 = 127
var big int64 = 9000000000
```

### Gotcha
Integer sizes matter. `int` is platform-dependent, while `int64` is fixed-width.

---

## 14. Floating-point numbers

### Types
- `float32`
- `float64`

### Example
```go
var price float64 = 99.99
```

### Gotcha
Floating-point numbers are not exact for all decimal values. Do not use them blindly for money calculations.

---

## 15. Strings

### Definition
A string is an immutable sequence of bytes, often used for text.

### Syntax
```go
var s string = "Go"
```

### Example
```go
name := "Rahul"
fmt.Println(name)
```

### Gotcha
Strings are immutable. You cannot directly modify a character inside a string.

---

## 16. Booleans

### Definition
Boolean values represent truth.

### Values
- `true`
- `false`

### Example
```go
isReady := true
```

### Gotcha
A boolean’s zero value is `false`.

---

## 17. Type conversion

### Definition
Go does not automatically convert between most types.

### Syntax
```go
newType(value)
```

### Examples
```go
var a int = 10
var b float64 = float64(a)

var s string = strconv.Itoa(123)
```

### Gotcha
Go is strict about types. `int` and `float64` are not mixed automatically.

---

## 18. Type inference

### Definition
Go can infer the type from the value you give it.

### Example
```go
x := 10
name := "Go"
```

### Gotcha
Type inference is convenient, but sometimes explicit types make code clearer.

---

## 19. `var` vs `:=`

### `var`
Use when:
- you want to declare at package level,
- you want the zero value first,
- you want type clarity.

### `:=`
Use when:
- you are inside a function,
- you want shorter syntax,
- the type can be inferred easily.

### Example
```go
var count int
message := "hello"
```

### Gotcha
You cannot use `:=` outside a function.

---

## 20. Basic type table

| Type | Example | Zero Value |
|------|---------|------------|
| int | `10` | `0` |
| float64 | `3.14` | `0` |
| bool | `true` | `false` |
| string | `"Go"` | `""` |
| rune | `'A'` | `0` |
| byte | `255` | `0` |
| pointer | `&x` | `nil` |

---

## 21. Common beginner gotchas

- `:=` only works inside functions.
- Uninitialized variables get zero values.
- `nil` is not the zero value for every type.
- Strings are immutable.
- Go does not do much automatic type conversion.
- Constants cannot be reassigned.
- `int` size depends on architecture.
- Floating-point values can be imprecise.
- `package main` is required for executable programs.
- `go mod init` should usually be done early.

---

## 22. Mental checklist

1. Start with `package main`.
2. Add imports only when needed.
3. Use `var` when you want clarity or zero values.
4. Use `:=` inside functions for shorter declarations.
5. Remember zero values.
6. Pick the correct numeric type.
7. Convert types explicitly.
8. Keep strings immutable in mind.
9. Initialize the module with `go mod init`.
10. Use small examples to test type behavior.

---

## 23. Quick example

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    var age int
    name := "Rahul"
    active := true
    score := 99.5

    fmt.Println(age)
    fmt.Println(name)
    fmt.Println(active)
    fmt.Println(score)

    num, _ := strconv.Atoi("42")
    fmt.Println(num)
}
```

---

This note is your starting reference for Go basics, history, variables, and data types.
