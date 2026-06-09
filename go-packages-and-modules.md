# Go Packages and Modules

Packages and modules help organize Go code, reuse it, and manage dependencies.

---

## 1. Package

### Definition
A package is a folder-level unit of code organization in Go.

### Analogy
A package is like a drawer that holds related tools together.

### Example
Files inside the same folder usually belong to the same package.

### Syntax
```go
package main
```

or

```go
package utils
```

---

## 2. `main` package

### Definition
`package main` tells Go this is an executable program.

### Example
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello")
}
```

---

## 3. Importing packages

### Syntax
```go
import "fmt"
```

### Multiple imports
```go
import (
    "fmt"
    "math"
)
```

### Example
```go
fmt.Println(math.Sqrt(16))
```

---

## 4. Exported vs unexported

### Rule
- Capital letter = exported / public.
- Lowercase = unexported / private.

### Example
```go
package person

type User struct {
    Name string
    age  int
}

func NewUser(name string, age int) User {
    return User{Name: name, age: age}
}
```

### Meaning
`Name` is available outside the package, but `age` is not.

---

## 5. Module

### Definition
A module is a collection of Go packages managed together with a `go.mod` file.

### Analogy
A module is like a project folder with its own dependency list.

---

## 6. `go.mod`

### Purpose
It defines the module name and dependency versions.

### Example
```go
module example.com/myapp

go 1.22
```

---

## 7. Create a module

### Command
```bash
go mod init example.com/myapp
```

This creates `go.mod`.

---

## 8. Add dependencies

When you import an external package, Go can fetch it automatically.

### Useful commands
```bash
go mod tidy
go get package-name
```

### Example
```bash
go get github.com/google/uuid
```

---

## 9. Standard project structure

### Example
```text
myapp/
├── go.mod
├── main.go
├── utils/
│   └── utils.go
└── models/
    └── user.go
```

---

## 10. Cross-package use

### Example
`utils/utils.go`
```go
package utils

func Add(a, b int) int {
    return a + b
}
```

`main.go`
```go
package main

import (
    "fmt"
    "example.com/myapp/utils"
)

func main() {
    fmt.Println(utils.Add(2, 3))
}
```

---

## 11. Package naming rules
- Keep package names short and clear.
- Use lowercase names.
- Avoid repeating package name in function names.

Bad:
```go
func UtilsAdd() int
```

Better:
```go
func Add() int
```

---

## 12. Relative thinking
Each package should have one clear responsibility:
- `models` for data types,
- `utils` for helpers,
- `handlers` for HTTP handlers,
- `services` for business logic.

---

## Mental checklist
- Use packages to organize code.
- Use modules to manage dependencies.
- Use `main` for executable programs.
- Capitalize identifiers you want to export.
- Keep package names simple.
