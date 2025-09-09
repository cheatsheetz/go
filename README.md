# Go Cheat Sheet

Comprehensive reference for Go programming language covering syntax, goroutines, interfaces, packages, and essential features for modern Go development.

---

## Table of Contents
- [Basic Syntax](#basic-syntax)
- [Data Types](#data-types)
- [Variables and Constants](#variables-and-constants)
- [Control Structures](#control-structures)
- [Functions](#functions)
- [Structs and Methods](#structs-and-methods)
- [Interfaces](#interfaces)
- [Packages and Imports](#packages-and-imports)
- [Error Handling](#error-handling)
- [Goroutines and Channels](#goroutines-and-channels)
- [Pointers](#pointers)
- [Slices and Maps](#slices-and-maps)

---

## Basic Syntax

| Feature | Syntax | Example |
|---------|--------|---------|
| Package Declaration | `package packagename` | `package main` |
| Import | `import "package"` | `import "fmt"` |
| Function Declaration | `func name(params) returnType` | `func add(a, b int) int` |
| Variable Declaration | `var name type` | `var count int` |
| Short Declaration | `name := value` | `count := 10` |
| Comments | `// single line` or `/* multi-line */` | `// This is a comment` |

### Basic Program Structure
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

## Data Types

### Basic Types
| Type | Description | Example |
|------|-------------|---------|
| `bool` | Boolean | `true`, `false` |
| `string` | String | `"hello"` |
| `int` | Integer (32 or 64 bit) | `42` |
| `int8`, `int16`, `int32`, `int64` | Specific bit integers | `int32(100)` |
| `uint`, `uint8`, `uint16`, `uint32`, `uint64` | Unsigned integers | `uint(100)` |
| `byte` | Alias for uint8 | `byte('A')` |
| `rune` | Alias for int32 (Unicode) | `rune('€')` |
| `float32`, `float64` | Floating point | `3.14` |
| `complex64`, `complex128` | Complex numbers | `complex(1, 2)` |

### Zero Values
```go
var b bool       // false
var i int        // 0
var f float64    // 0.0
var s string     // ""
var p *int       // nil
```

### Type Conversion
```go
i := 42
f := float64(i)
u := uint(f)
s := string(rune(i))

// String conversions
import "strconv"

// String to int
num, err := strconv.Atoi("123")
// Int to string
str := strconv.Itoa(123)
// String to float
f, err := strconv.ParseFloat("3.14", 64)
```

## Variables and Constants

### Variable Declaration
```go
// Long form
var name string = "Go"
var age int = 5

// Type inference
var name = "Go"
var age = 5

// Short declaration (inside functions only)
name := "Go"
age := 5

// Multiple variables
var a, b, c int = 1, 2, 3
x, y, z := 1, 2.0, "three"

// Zero values
var i int     // 0
var s string  // ""
var b bool    // false
```

### Constants
```go
const Pi = 3.14159
const World = "世界"

// Multiple constants
const (
    StatusOK = 200
    StatusNotFound = 404
)

// iota (auto-incrementing)
const (
    Sunday = iota    // 0
    Monday           // 1
    Tuesday          // 2
    Wednesday        // 3
)
```

## Control Structures

### If Statements
```go
// Basic if
if x > 0 {
    fmt.Println("positive")
}

// If-else
if x > 0 {
    fmt.Println("positive")
} else if x < 0 {
    fmt.Println("negative")
} else {
    fmt.Println("zero")
}

// If with short statement
if v := math.Pow(x, n); v < lim {
    return v
}
```

### Switch Statements
```go
// Basic switch
switch day {
case "monday":
    fmt.Println("Start of work week")
case "friday":
    fmt.Println("TGIF")
default:
    fmt.Println("Regular day")
}

// Switch with no expression (replaces if-else chain)
switch {
case score >= 90:
    grade = "A"
case score >= 80:
    grade = "B"
default:
    grade = "F"
}

// Type switch
switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
default:
    fmt.Printf("Unknown type: %T\n", v)
}
```

### Loops
```go
// For loop (only loop in Go)
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-like loop
for condition {
    // code
}

// Infinite loop
for {
    // code
    if someCondition {
        break
    }
}

// Range loop
slice := []int{1, 2, 3, 4, 5}
for index, value := range slice {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// Range with just value
for _, value := range slice {
    fmt.Println(value)
}

// Range over map
m := map[string]int{"a": 1, "b": 2}
for key, value := range m {
    fmt.Printf("%s: %d\n", key, value)
}
```

## Functions

### Function Declaration
```go
// Basic function
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return // naked return
}

// Variadic function
func sum(numbers ...int) int {
    total := 0
    for _, number := range numbers {
        total += number
    }
    return total
}

// Usage
result := sum(1, 2, 3, 4, 5)
```

### Function as Values
```go
// Function as variable
var fn func(int, int) int = add

// Anonymous function
multiply := func(a, b int) int {
    return a * b
}

// Function as parameter
func calculate(a, b int, op func(int, int) int) int {
    return op(a, b)
}

result := calculate(10, 5, add)      // 15
result = calculate(10, 5, multiply)  // 50
```

### Closures
```go
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

// Usage
c1 := counter()
fmt.Println(c1()) // 1
fmt.Println(c1()) // 2

c2 := counter()
fmt.Println(c2()) // 1 (independent counter)
```

## Structs and Methods

### Struct Definition
```go
type Person struct {
    Name string
    Age  int
    City string
}

// Creating structs
p1 := Person{Name: "Alice", Age: 30, City: "NYC"}
p2 := Person{"Bob", 25, "LA"}  // positional

// Anonymous struct
car := struct {
    Brand string
    Year  int
}{
    Brand: "Toyota",
    Year:  2023,
}
```

### Struct Embedding
```go
type Address struct {
    Street string
    City   string
    State  string
}

type Employee struct {
    Person           // embedded struct
    Address         // embedded struct
    ID      int
    Salary  float64
}

// Usage
emp := Employee{
    Person:  Person{Name: "John", Age: 35},
    Address: Address{Street: "123 Main St", City: "Boston"},
    ID:      1001,
    Salary:  75000,
}

fmt.Println(emp.Name)   // Access embedded field directly
fmt.Println(emp.Street) // Access embedded field directly
```

### Methods
```go
// Method with value receiver
func (p Person) FullInfo() string {
    return fmt.Sprintf("%s is %d years old", p.Name, p.Age)
}

// Method with pointer receiver
func (p *Person) HaveBirthday() {
    p.Age++
}

// Usage
person := Person{Name: "Alice", Age: 30}
fmt.Println(person.FullInfo()) // Alice is 30 years old
person.HaveBirthday()
fmt.Println(person.Age)        // 31
```

## Interfaces

### Interface Definition
```go
type Writer interface {
    Write([]byte) (int, error)
}

type Reader interface {
    Read([]byte) (int, error)
}

// Interface composition
type ReadWriter interface {
    Reader
    Writer
}

// Empty interface
type Any interface{} // equivalent to interface{}
```

### Interface Implementation
```go
type File struct {
    name string
}

// Implement Writer interface
func (f File) Write(data []byte) (int, error) {
    fmt.Printf("Writing to %s: %s\n", f.name, string(data))
    return len(data), nil
}

// Usage
var w Writer = File{name: "test.txt"}
w.Write([]byte("Hello, World!"))
```

### Type Assertion and Type Switch
```go
// Type assertion
var i interface{} = "hello"
s := i.(string)           // Assert that i is a string
s, ok := i.(string)       // Safe type assertion

if ok {
    fmt.Println("String:", s)
}

// Type switch
func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s\n", v)
    case bool:
        fmt.Printf("Boolean: %t\n", v)
    default:
        fmt.Printf("Unknown type: %T\n", v)
    }
}
```

### Common Interfaces
```go
// Stringer interface (like toString in other languages)
type Stringer interface {
    String() string
}

func (p Person) String() string {
    return fmt.Sprintf("%s (%d)", p.Name, p.Age)
}

// Error interface
type error interface {
    Error() string
}

type MyError struct {
    Msg string
}

func (e MyError) Error() string {
    return e.Msg
}
```

## Packages and Imports

### Package Declaration
```go
// In file math/calc.go
package math

import "fmt"

// Exported function (starts with capital letter)
func Add(a, b int) int {
    return a + b
}

// Unexported function (starts with lowercase letter)
func subtract(a, b int) int {
    return a - b
}
```

### Import Statements
```go
// Standard import
import "fmt"
import "math"

// Multiple imports
import (
    "fmt"
    "math"
    "strings"
)

// Import with alias
import (
    f "fmt"
    m "math"
)

// Blank import (for side effects)
import _ "image/png"

// Dot import (not recommended)
import . "fmt"
```

### Init Function
```go
package main

import "fmt"

func init() {
    fmt.Println("This runs before main")
}

func main() {
    fmt.Println("This runs in main")
}
```

## Error Handling

### Basic Error Handling
```go
import (
    "errors"
    "fmt"
)

func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Usage
result, err := divide(10, 2)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Result:", result)
```

### Custom Errors
```go
type ValidationError struct {
    Field   string
    Message string
}

func (e ValidationError) Error() string {
    return fmt.Sprintf("validation error on field '%s': %s", e.Field, e.Message)
}

func validateAge(age int) error {
    if age < 0 {
        return ValidationError{
            Field:   "age",
            Message: "cannot be negative",
        }
    }
    return nil
}
```

### Error Wrapping (Go 1.13+)
```go
import (
    "errors"
    "fmt"
)

func process() error {
    err := someFunction()
    if err != nil {
        return fmt.Errorf("failed to process: %w", err)
    }
    return nil
}

// Check wrapped errors
err := process()
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("File not found")
}

// Unwrap errors
var validationErr ValidationError
if errors.As(err, &validationErr) {
    fmt.Println("Validation error:", validationErr.Field)
}
```

## Goroutines and Channels

### Goroutines
```go
import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")    // Start goroutine
    say("hello")       // Run in main goroutine
}
```

### Channels
```go
// Create channel
ch := make(chan int)

// Buffered channel
ch := make(chan int, 100)

// Send and receive
ch <- 42        // Send
value := <-ch   // Receive

// Close channel
close(ch)

// Check if channel is closed
value, ok := <-ch
if !ok {
    fmt.Println("Channel is closed")
}
```

### Channel Patterns
```go
// Worker pattern
func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second)
        results <- job * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // Start workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    // Send jobs
    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    // Collect results
    for r := 1; r <= 9; r++ {
        <-results
    }
}
```

### Select Statement
```go
select {
case msg1 := <-ch1:
    fmt.Println("Received from ch1:", msg1)
case msg2 := <-ch2:
    fmt.Println("Received from ch2:", msg2)
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
default:
    fmt.Println("No channel ready")
}
```

### Sync Package
```go
import "sync"

// WaitGroup
var wg sync.WaitGroup

for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        fmt.Printf("Goroutine %d\n", id)
    }(i)
}

wg.Wait() // Wait for all goroutines to complete

// Mutex
var mu sync.Mutex
var count int

func increment() {
    mu.Lock()
    defer mu.Unlock()
    count++
}

// Once
var once sync.Once

func setup() {
    fmt.Println("Setup called")
}

func doWork() {
    once.Do(setup) // setup will be called only once
}
```

## Pointers

### Pointer Basics
```go
var p *int        // Pointer to int, initially nil

i := 42
p = &i            // p points to i
fmt.Println(*p)   // Dereference pointer, prints 42

*p = 21           // Set value through pointer
fmt.Println(i)    // i is now 21

// Zero value of pointer is nil
var p2 *int
fmt.Println(p2 == nil) // true
```

### Pointers with Structs
```go
type Person struct {
    Name string
    Age  int
}

p := Person{Name: "Alice", Age: 30}
ptr := &p

// Two ways to access fields
fmt.Println((*ptr).Name)  // Explicit dereferencing
fmt.Println(ptr.Name)     // Go allows direct access

// Modifying through pointer
ptr.Age = 31
fmt.Println(p.Age)        // 31
```

### new() Function
```go
// new() allocates memory and returns a pointer
p := new(int)    // p is *int, points to zero value of int (0)
*p = 42
fmt.Println(*p)  // 42

// Equivalent to:
var i int
p = &i
```

## Slices and Maps

### Arrays vs Slices
```go
// Array (fixed size)
var arr [5]int = [5]int{1, 2, 3, 4, 5}
arr2 := [...]int{1, 2, 3, 4, 5}  // Size inferred

// Slice (dynamic array)
var slice []int = []int{1, 2, 3, 4, 5}
slice2 := []int{1, 2, 3, 4, 5}

// Create slice from array
slice3 := arr[1:4]  // Elements 1, 2, 3
```

### Slice Operations
```go
// make() function
slice := make([]int, 5)      // length 5, capacity 5
slice2 := make([]int, 5, 10) // length 5, capacity 10

// append()
slice = append(slice, 6)
slice = append(slice, 7, 8, 9)
slice = append(slice, []int{10, 11, 12}...)

// copy()
dest := make([]int, len(slice))
copy(dest, slice)

// Slice properties
fmt.Println(len(slice))    // Length
fmt.Println(cap(slice))    // Capacity

// Slicing
fmt.Println(slice[1:3])    // Elements 1 and 2
fmt.Println(slice[:3])     // First 3 elements
fmt.Println(slice[2:])     // From index 2 to end
fmt.Println(slice[:])      // All elements
```

### Maps
```go
// Create map
m := make(map[string]int)
m["apple"] = 5
m["banana"] = 3

// Map literal
colors := map[string]string{
    "red":   "#FF0000",
    "green": "#00FF00",
    "blue":  "#0000FF",
}

// Check if key exists
value, ok := colors["red"]
if ok {
    fmt.Println("Red color code:", value)
}

// Delete key
delete(colors, "red")

// Iterate over map
for key, value := range colors {
    fmt.Printf("%s: %s\n", key, value)
}

// Zero value of map is nil
var nilMap map[string]int
fmt.Println(nilMap == nil) // true
```

### Advanced Slice and Map Operations
```go
// Multi-dimensional slice
matrix := make([][]int, 3)
for i := range matrix {
    matrix[i] = make([]int, 4)
}

// Map of slices
groups := map[string][]string{
    "fruits":     {"apple", "banana", "orange"},
    "vegetables": {"carrot", "broccoli", "spinach"},
}

// Slice of maps
users := []map[string]interface{}{
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
}
```

---

## Resources
- [Official Go Documentation](https://golang.org/doc/)
- [Go by Example](https://gobyexample.com/)
- [Effective Go](https://golang.org/doc/effective_go.html)
- [Go Tour](https://tour.golang.org/)
- [Go Package Documentation](https://pkg.go.dev/)

---
*Originally compiled from various sources. Contributions welcome!*