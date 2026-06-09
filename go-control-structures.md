# Go Control Structures

Go control structures decide the flow of execution in a program. They let you run code only when a condition is true, repeat code, or choose between multiple paths.

---

## 1. `if`, `else if`, `else`

### Definition
`if` checks a condition. If the condition is true, Go runs that block. If not, it can move to `else if` or `else`.

### Analogy
Think of it like a checkpoint:
- If the condition passes, you go through.
- If not, you try another gate.
- If none match, you go to the default path.

### Syntax
```go
if condition {
    // code
} else if anotherCondition {
    // code
} else {
    // code
}
```

### Example
```go
age := 18

if age >= 18 {
    fmt.Println("Adult")
} else {
    fmt.Println("Minor")
}
```

### With short statement
You can declare a variable inside `if`.

```go
if num := 10; num > 5 {
    fmt.Println("Greater than 5")
}
```

### Notes
- The `{` must stay on the same line as `if`.
- The variable declared in `if` is available only inside that `if` block.

---

## 2. `switch`

### Definition
`switch` chooses one block from multiple cases based on a value or condition.

### Analogy
Like selecting one lane at a toll booth depending on your ticket number.

### Syntax
```go
switch expression {
case value1:
    // code
case value2:
    // code
default:
    // code
}
```

### Example
```go
day := "Monday"

switch day {
case "Monday":
    fmt.Println("Start of week")
case "Friday":
    fmt.Println("Weekend soon")
default:
    fmt.Println("Regular day")
}
```

### Switch with short statement
```go
switch n := 3; n {
case 1:
    fmt.Println("one")
case 2:
    fmt.Println("two")
case 3:
    fmt.Println("three")
}
```

### Notes
- Go `switch` does not fall through by default.
- Use `fallthrough` only when you explicitly want the next case to run.
- `default` runs if no case matches.

---

## 3. `for`

### Definition
`for` is Go’s only loop keyword. It can work as:
- a traditional loop,
- a while-style loop,
- a range loop.

### Analogy
Like a machine repeating steps until a stop condition is reached.

### Traditional loop syntax
```go
for initialization; condition; post {
    // code
}
```

### Example
```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

---

## 4. `for` as while loop

### Syntax
```go
for condition {
    // code
}
```

### Example
```go
count := 0
for count < 3 {
    fmt.Println(count)
    count++
}
```

---

## 5. Infinite loop

### Syntax
```go
for {
    // code
}
```

### Example
```go
for {
    fmt.Println("running forever")
    break
}
```

---

## 6. `break` and `continue`

### `break`
Stops the loop immediately.

```go
for i := 0; i < 10; i++ {
    if i == 5 {
        break
    }
    fmt.Println(i)
}
```

### `continue`
Skips the current iteration and moves to the next one.

```go
for i := 0; i < 5; i++ {
    if i == 2 {
        continue
    }
    fmt.Println(i)
}
```

---

## 7. `range`

### Definition
`range` is used to loop over slices, arrays, maps, strings, and channels.

### Analogy
Like walking through each item in a box one by one.

### Syntax
```go
for index, value := range collection {
    // code
}
```

### Example with slice
```go
nums := []int{10, 20, 30}

for i, v := range nums {
    fmt.Println(i, v)
}
```

### Example with map
```go
scores := map[string]int{"A": 90, "B": 80}

for key, value := range scores {
    fmt.Println(key, value)
}
```

### Example with only value
```go
for _, v := range nums {
    fmt.Println(v)
}
```

### Example with only index
```go
for i := range nums {
    fmt.Println(i)
}
```

---

## 8. `goto` and labels

### Definition
`goto` jumps to a labeled statement. It is rarely used.

### Syntax
```go
goto labelName

labelName:
    // code
```

### Example
```go
i := 0
if i == 0 {
    goto end
}
fmt.Println("skipped")

end:
fmt.Println("done")
```

### Note
Avoid `goto` unless you really need it.

---

## Mental checklist
- Use `if` for conditions.
- Use `switch` when comparing one value against many cases.
- Use `for` for all loops.
- Use `range` for slices, maps, strings, and arrays.
- Use `break` to stop, `continue` to skip.
