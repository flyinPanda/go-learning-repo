# 🚀 Go Data Structures & Pointers: The Developer's Cheat Sheet

A concise, scannable reference guide for Go's core data structures and memory management, tailored for developers coming from C# or JavaScript.

---

## 1. Arrays: The Rigid Foundation
- **Description:** Fixed-length sequence of elements of the same type. Size never changes after declaration.
- **Analogy:** An egg carton that holds exactly 12 eggs — you can't add a 13th.
- **C#/JS Context:** Like `int[]` in C#. JS has no fixed-size equivalent.
- **Advantages:** Predictable memory usage, very fast. Use when size is known and unchangeable (e.g., RGB colors `[3]int`).

### Initialization
```go
// 1. Zero-value initialization (fills with defaults: 0, "", false)
var scores int

// 2. Literal initialization
colors := string{"red", "green", "blue"}

// 3. Let compiler count the elements
magicNumbers := [...]int{4, 8, 15, 16, 23, 42}
```

### 🚨 Gotchas
- `[3]int` and `[4]int` are **different types**.
- Arrays are passed **by value** (fully copied) to functions.
- Use `len(arr)` to get size.

---

## 2. Slices: The Dynamic Workhorse
- **Description:** Dynamically-sized view into an underlying array. You'll use this **95% of the time**.
- **Analogy:** A sliding window over a long train — you can widen/narrow it.
- **C#/JS Context:** Like `List<T>` in C# or `[]` arrays in JS.

### Initialization
```go
// 1. Nil slice (preferred for empty, allocates ZERO memory)
var names []string

// 2. Literal initialization
vowels := []string{"a", "e", "i", "o", "u"}

// 3. Using make() (best for performance: pre-allocates capacity)
users := make([]string, 0, 100)  // type, length, capacity

// 4. Slicing existing array/slice
subset := vowels[1:3]  // = ["e", "i"]
```

### Common Operations
```go
// Append (may allocate new underlying array)
nums := []int{1, 2, 3}
nums = append(nums, 4)          // [1 2 3 4]
nums = append(nums, 5, 6)       // [1 2 3 4 5 6]
nums = append(nums, otherSlice[])  // append entire slice

// Copy (returns number of elements copied)
src := []int{1, 2, 3, 4, 5}
dst := make([]int, 3)
n := copy(dst, src)  // dst = [1 2 3], n = 3

// Length and capacity
len(nums)   // number of elements
cap(nums)   // capacity from current position to end of underlying array
```

### 🚨 Gotchas
- Copying a slice (like `subset`) does **not** copy underlying data — modifications affect original.
- `append` may resize the underlying array; the original slice reference may become "stale" regarding capacity.
- Slices are already "reference-like" — **rarely use `*[]T`**.

---

## 3. Maps: The Key-Value Store
- **Description:** Unordered collection of unique key-value pairs.
- **Analogy:** Coat check — unique ticket (key) gets your coat (value).
- **C#/JS Context:** `Dictionary<TKey, TValue>` in C#; `Map` or `{}` objects in JS.

### Initialization
```go
// 1. Nil map (DANGER 🚨 — writing causes PANIC)
var config map[string]string

// 2. Using make() (standard, safe for writing)
scores := make(map[string]int)
scores["Alice"] = 95

// 3. Literal initialization
statusCodes := map[int]string{
    200: "OK",
    404: "Not Found",
}
```

### Common Operations
```go
// Set
scores["Bob"] = 88

// Get (returns zero-value if missing)
val := scores["Alice"]  // 95
val := scores["Unknown"]  // 0

// Check existence
val, exists := scores["Alice"]
if exists {
    // use val
}

// Delete
delete(scores, "Bob")

// Length
len(scores)  // number of key-value pairs

// Iterate
for key, val := range scores {
    fmt.Println(key, val)
}

// Iterate only keys
for key := range scores {
    fmt.Println(key)
}
```

### 🚨 Gotchas
- Map iteration is **intentionally randomized**.
- Missing key returns **zero-value** (e.g., `0`, `""`, `false`), not `null`.
- Always use `val, exists := m[key]` to check existence.
- Maps are **reference types** — **never use `*map[K]V`**.

---

## 4. Structs: The Custom Blueprints
- **Description:** Typed collection of fields (different types allowed) to model data.
- **Analogy:** DMV form with labeled boxes: Name, Age, etc.
- **C#/JS Context:** `class`/`struct` in C#; object in JS.

### Definition & Initialization
```go
type User struct {
    Name string
    Age  int
}

// 1. Zero-value initialization
var u1 User  // Name = "", Age = 0

// 2. Field-name literal (best practice)
u2 := User{
    Name: "Bob",
    Age:  30,
}

// 3. Using new (returns POINTER *User)
u3 := new(User)  // *User, fields are zero-valued

// 4. Pointer literal
u4 := &User{Name: "Alice", Age: 29}
```

### Common Operations
```go
// Access fields
u2.Name = "Charlie"
u2.Age++

// Pointer field access (Go auto-dereferences)
func Birthday(u *User) {
    u.Age++  // no need to write u.(*User).Age
}
```

### 🚨 Gotchas
- **Capitalization controls visibility:**
  - `Name` → exported (public, visible to JSON, other packages)
  - `age` → unexported (private to package)
- Structs are passed **by value** unless you use pointers.
- Use pointers for large structs or when mutation is needed.

---

## 🎯 Pointers & Memory Management

### Core Concept
- Go passes **everything by value** (copies) by default.
- **Pointer** lets you share and mutate the same underlying data.

### Analogy
- **Value:** Emailing a PDF — they edit their copy, yours stays same.
- **Pointer:** Sharing a Google Doc URL — both edit the same document.

### Symbols
| Symbol | Meaning                      | Example              |
|--------|------------------------------|----------------------|
| `&`    | Address of (where does it live?) | `p := &x`           |
| `*`    | Pointer to (go to this address)  | `y := *p`           |

### Declaration & Usage
```go
x := 42
p := &x        // p is *int, holds address of x
y := *p        // y = 42 (dereference)

*p = 100       // x becomes 100
```

### When to Use Pointers with Data Structures

#### 1. Structs (Use Frequently)
Use pointers when:
- Struct is large (avoid copying).
- Function needs to mutate the struct.

```go
func Birthday(u *User) {
    u.Age++
}

myUser := &User{Name: "Alice", Age: 29}
Birthday(myUser)  // mutates original
```

#### 2. Slices (Rarely Use Pointers)
- Slice is already a tiny struct (`ptr, len, cap`) — pass by value.
- Only use `*[]T` if function must change the slice variable itself (e.g., reassign).
- Prefer **returning a new slice**.

#### 3. Maps (Never Use Pointers)
- Maps are reference types under the hood.
- Modifying a map in a function affects caller's map.
- `*map[string]int` is unnecessary.

#### 4. Arrays (Use for Performance)
- Passing `*[N]T` passes an 8-byte address instead of copying `N` elements.
- But in practice, **use slices instead** unless size is fixed and small.

---

## 🔁 Common Patterns & Helpers

### Range Over All Types
```go
// Array/Slice
for i, val := range nums {
    fmt.Println(i, val)
}

// Map
for key, val := range m {
    fmt.Println(key, val)
}

// String (as bytes)
for i, ch := range "hello" {
    fmt.Println(i, ch)  // ch is int32 (Unicode code point)
}
```

### Functions Returning Multiple Values
```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

result, err := divide(10, 2)
if err != nil {
    // handle error
}
```

### Error Handling Pattern
```go
val, err := someFunction()
if err != nil {
    log.Fatal(err)
}
// use val
```

---

## 📦 Quick Reference Table

| Type      | Fixed Size? | Reference Type? | Pass by Pointer?      | Common Use                  |
|-----------|-------------|-----------------|-----------------------|-----------------------------|
| Array     | ✅ Yes      | ❌ No           | Sometimes (`*[N]T`)   | Fixed-size data (RGB, etc.) |
| Slice     | ❌ No       | ❌ No*          | Rarely (`*[]T`)       | General dynamic lists       |
| Map       | ❌ No       | ✅ Yes          | ❌ Never              | Key-value lookup            |
| Struct    | ✅ Yes      | ❌ No           | Often (`*T`)          | Custom data models          |

*Slice is a descriptor with a pointer, but not a reference type like maps.

---

## 🧠 Mental Checklist When Stuck

1. **Do I need fixed size?** → Use **array**.
2. **Do I need dynamic size?** → Use **slice**.
3. **Do I need key-based lookup?** → Use **map**.
4. **Do I need custom fields?** → Use **struct**.
5. **Do I need to mutate in a function or avoid large copies?** → Use **pointer**.
6. **Is it a map?** → Never use pointer.
7. **Is it a slice?** → Rarely use pointer.
8. **Is it a struct?** → Often use pointer.

---

Save this as `go-data-structures-cheatsheet.md` and open it whenever you're writing Go or get stuck.