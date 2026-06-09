# Go Core Packages

Core packages are the standard library packages you will use often in Go. They solve common problems without external dependencies.

---

## 1. `fmt`

### Purpose
Formatting and printing.

### Syntax
```go
fmt.Println("hello")
fmt.Printf("age: %d", age)
fmt.Sprintf("name: %s", name)
```

### Example
```go
name := "Rahul"
fmt.Printf("Hello, %s\n", name)
```

### Common use
- printing output,
- formatted strings,
- scanning input.

---

## 2. `strings`

### Purpose
String manipulation.

### Common functions
```go
strings.Contains(s, "go")
strings.HasPrefix(s, "pre")
strings.HasSuffix(s, "fix")
strings.ToUpper(s)
strings.ToLower(s)
strings.TrimSpace(s)
strings.Split(s, ",")
strings.Join(parts, "-")
```

### Example
```go
s := "  go lang  "
fmt.Println(strings.TrimSpace(s))
```

---

## 3. `strconv`

### Purpose
Convert between strings and numbers.

### Syntax
```go
strconv.Atoi("123")
strconv.Itoa(123)
strconv.ParseInt("123", 10, 64)
strconv.FormatInt(123, 10)
```

### Example
```go
n, err := strconv.Atoi("42")
if err == nil {
    fmt.Println(n)
}
```

---

## 4. `math`

### Purpose
Math operations.

### Example functions
```go
math.Sqrt(25)
math.Pow(2, 3)
math.Max(10, 20)
math.Min(10, 20)
```

---

## 5. `time`

### Purpose
Time, date, sleep, timers, durations.

### Syntax
```go
time.Now()
time.Sleep(time.Second)
time.Date(...)
time.After(time.Second)
```

### Example
```go
fmt.Println(time.Now())
time.Sleep(2 * time.Second)
```

---

## 6. `os`

### Purpose
Operating system interaction.

### Common use
```go
os.Args
os.Getenv("HOME")
os.Setenv("KEY", "value")
os.Create("file.txt")
os.Open("file.txt")
```

---

## 7. `io` and `io/ioutil` concept

### Purpose
Input/output helpers.

### Common use
```go
io.Copy(dst, src)
io.ReadAll(reader)
```

### Note
`io/ioutil` is mostly replaced by `io` and `os` in modern Go.

---

## 8. `bufio`

### Purpose
Buffered input/output.

### Example
```go
reader := bufio.NewReader(os.Stdin)
line, _ := reader.ReadString('\n')
```

---

## 9. `encoding/json`

### Purpose
Encode and decode JSON.

### Syntax
```go
json.Marshal(value)
json.Unmarshal(data, &target)
```

### Example
```go
type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
```

---

## 10. `net/http`

### Purpose
Build web servers and make HTTP requests.

### Server example
```go
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello")
})
http.ListenAndServe(":8080", nil)
```

### Client example
```go
resp, err := http.Get("https://example.com")
```

---

## 11. `sort`

### Purpose
Sort slices.

### Example
```go
sort.Ints(nums)
sort.Strings(names)
sort.Slice(items, func(i, j int) bool {
    return items[i].Age < items[j].Age
})
```

---

## 12. `path/filepath`

### Purpose
Work with file paths in an OS-safe way.

### Example
```go
filepath.Join("home", "user", "file.txt")
filepath.Abs(".")
```

---

## 13. `errors`

### Purpose
Create and inspect errors.

### Example
```go
err := errors.New("something went wrong")
```

---

## 14. `context`

### Purpose
Carry deadlines, cancellation, and request values.

### Example
```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel()
```

---

## Mental checklist
- Use `fmt` for output.
- Use `strings` and `strconv` for text conversion.
- Use `time` for timestamps and delays.
- Use `encoding/json` for APIs.
- Use `net/http` for web services.
- Use `os`, `io`, and `bufio` for system/file work.
- Use `context` for cancellation and deadlines.
