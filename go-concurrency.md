# Go Concurrency

Concurrency means making progress on multiple tasks at the same time. Go is built for this with goroutines and channels.

---

## 1. What is concurrency?

### Definition
Concurrency is managing multiple tasks in overlapping time periods.

### Analogy
Like a chef handling several dishes at once:
- one dish boils,
- another is chopped,
- another is baked.

The chef switches attention between them efficiently.

---

## 2. Goroutines

### Definition
A goroutine is a lightweight function running concurrently.

### Syntax
```go
go functionName()
```

### Example
```go
go fmt.Println("running in goroutine")
```

### Real example
```go
func task() {
    fmt.Println("task running")
}

func main() {
    go task()
    time.Sleep(time.Second)
}
```

### Notes
- Goroutines are very cheap compared to OS threads.
- If main exits, goroutines stop too.

---

## 3. Channels

### Definition
Channels are used to pass data between goroutines.

### Analogy
A channel is like a conveyor belt or pipe between workers.

### Syntax to declare
```go
var ch chan int
ch = make(chan int)
```

### Syntax to send and receive
```go
ch <- 10
value := <-ch
```

### Example
```go
func main() {
    ch := make(chan int)

    go func() {
        ch <- 42
    }()

    value := <-ch
    fmt.Println(value)
}
```

---

## 4. Buffered channels

### Definition
A buffered channel can hold values up to its capacity.

### Syntax
```go
ch := make(chan int, 3)
```

### Example
```go
ch := make(chan int, 2)
ch <- 1
ch <- 2
```

### Note
Sending blocks only when the buffer is full.

---

## 5. Closing channels

### Syntax
```go
close(ch)
```

### Example
```go
go func() {
    ch <- 1
    ch <- 2
    close(ch)
}()
```

### Receiving from closed channel
```go
v, ok := <-ch
```

If `ok` is `false`, the channel is closed and empty.

---

## 6. Range over channel

### Syntax
```go
for v := range ch {
    fmt.Println(v)
}
```

### Use
This reads values until the channel is closed.

---

## 7. Select

### Definition
`select` waits on multiple channel operations.

### Analogy
Like choosing whichever phone rings first.

### Syntax
```go
select {
case v := <-ch1:
    fmt.Println(v)
case ch2 <- 10:
    fmt.Println("sent")
default:
    fmt.Println("no channel ready")
}
```

### Example
```go
select {
case msg := <-messages:
    fmt.Println(msg)
case <-time.After(time.Second):
    fmt.Println("timeout")
}
```

---

## 8. WaitGroup

### Definition
`sync.WaitGroup` waits for multiple goroutines to finish.

### Syntax
```go
var wg sync.WaitGroup
wg.Add(1)
wg.Done()
wg.Wait()
```

### Example
```go
var wg sync.WaitGroup

wg.Add(1)
go func() {
    defer wg.Done()
    fmt.Println("working")
}()

wg.Wait()
```

---

## 9. Mutex

### Definition
A mutex protects shared data from concurrent access problems.

### Analogy
A single-key bathroom lock:
- only one goroutine can enter the critical section.

### Syntax
```go
var mu sync.Mutex
mu.Lock()
mu.Unlock()
```

### Example
```go
var mu sync.Mutex
var count int

func inc() {
    mu.Lock()
    count++
    mu.Unlock()
}
```

### Best practice
Use `defer mu.Unlock()` after locking when possible.

---

## 10. Atomic operations

### Definition
Atomic operations are low-level safe operations on shared values.

### Use
Useful for counters and flags when you want fast synchronization.

---

## 11. Context

### Definition
`context.Context` carries cancellation, timeout, and request-scoped values.

### Analogy
Like a control signal that tells goroutines:
- stop now,
- wait only this long,
- or carry request info.

### Syntax
```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
```

### Example
```go
select {
case <-ctx.Done():
    fmt.Println("cancelled")
case result := <-ch:
    fmt.Println(result)
}
```

---

## 12. Common concurrency patterns

### Fan-out
One input is processed by many goroutines.

### Fan-in
Many goroutines send results to one channel.

### Worker pool
A fixed number of workers process tasks from a shared queue.

---

## 13. Important warnings
- Never write to shared data from multiple goroutines without synchronization.
- Use channels or mutexes to avoid race conditions.
- Always think about who closes the channel.
- Prefer simple designs first.

---

## Mental checklist
- Use goroutines for concurrent tasks.
- Use channels to communicate.
- Use `select` when waiting on many channel events.
- Use `WaitGroup` to wait.
- Use mutexes for shared mutable state.
- Use `context` for cancellation and timeouts.
