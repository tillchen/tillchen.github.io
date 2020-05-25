# Go Notes


* [Basics](#basics)
* [Data Structures](#data-structures)
* [Structs](#structs)
* [Errors and Failures](#errors-and-failures)
* [Goroutines and Channels](#goroutines-and-channels)
* [Testing](#testing)

## Basics

1. Hello World:

    ```go
    package main

    import (
        "fmt"
        "strings"
        "math/rand"
        "time"
        "github.com/headfirstgo/keyboard" // use go get to download the package
    )

    func main() {
        fmt.Println("Hello Go!")
        fmt.Println(strings.Title("head first go"))
        rand.Seed(time.Now().Unix())
        fmt.Println(rand.Intn(100)) // 0 - 99
    }
    ```

2. Use `go fmt` to format the code. (Go uses tabs instead of spaces.)

3. Use `go run` to compile and run. Or `go build` and `./foo`. `go install` generates the binary in the bin directory.

4. Rune literals are surrounded by single quotes. (Just like C chars.)

5. Initialization:

    ```go
    var foobar bool // the zero (default) value is false
    // The zero values: 0 for int and float; false for bool
    foo := 42 // short variable declaration
    // ONLY ONE variable in the short variable declaration has to be new
    // This allows multiple err for definitions
    ```

6. Go prefers the camelCase and no semicolons.

7. Type conversions:

   * Go uses the C-like style type conversion: `length = float64(width)`
   * And similarly for string to int: `foo, err := strconv.Atoi("123")` (ASCII to Int)

8. Go doesn't allow variable declarations unless we use these variables.

9. We can use the blank identifier just like in Swift:

    ```go
    reader := bufio.NewReader(os.Stdin)
    input, _ := reader.ReadString('\n') // if we don't handle the err
    // or we handle it:
    // if err != nil {
        // log.Fatal(err)
    //}
    ```

10. Like in Swift/Python, there's no need to add parentheses for `if` and `for`.

11. Since Go is syntactically similar to C, it also uses the formatting verbs: `%f`, `%d`, `%s`, `%t` (Boolean), `%v` (Any.) (`%2d` and `%.2f` are also used to round the numbers.)

12. Like in Swift and Kotlin, the types are after the parameter names in a function:

    ```go
    func foo(bar string, foobar int) string {
        return "Hello Go!"
    }

    func foofoo() (string, string) {
        return "Hello", "Go!"
    }
    ```

13. Go is a "pass-by-value" language by default.
    * And we have the C-style pointers and addresses. A shorter form: `fooPointer := &foo` `*fooPointer = 42` `fmt.Println(*fooPointer)`.
    * Use pointers to change the value for functions:

        ```go
        func myFunc(foo *bar) {
            foo.foofoo ++
        }

        myFunc(foofoofoo&)
        ```

14. `const fooBar = 42` or `const fooBar int = 42`

15. Documentations:
    * `go doc strconv` to see the documentation of strconv.
    * `go doc strconv Atoi` to see the specific function.
    * `godoc -http=:6060` starts the local server at port 6060.

16. Add ordinary comments before the package line and before functions to make them doc comments. A few conventions:
    * Comments should be complete sentences.
    * Package comments should begin with "Package + name": `// Package myPackage does nothing.`.
    * Function comments should begin with the function name: `// MyFunction does nothing.`.

17. `import ("os")` and `os.Args[1:]` gives the command-line arguments (except the file name).

18. Variadic functions: `func myFunc (param1 int, param2 ...string) {}` (unlimited string parameters stored as a alice in param2)

19. Functions are first-class in go as well.

    ```go
    func twice(theFunction func()) {
        theFunction()
        theFunction()
    }
    ```

## Data Structures

1. Arrays:
    * `var myArray [4]string`
    * `myArray := [2]int{3, 4}`
    * `fmt.Println(myArray)` (can be printed directly).
    * Like in Python, use `len(meArray)` to check the length.

2. For loops: `for index, value := range myArray`

    ```go
    for index, note := range notes {
        fmt.Println(index, note)
    }
    // or
    for _, note := range notes {
        fmt.Println(note)
    }
    // or
    for index, _ := range notes {
        fmt.Println(index)
    }
    ```

3. Slices: (similar to Python)
    * `var notes []string` then `notes = make([]string, 7)`
    * Or `notes := []string{"foo", "bar"}`
    * `myArray := [5]string{"a", "b, "c", "d", "e"}` `mySlice := myArray[1:3]` gives b, c.
    * We can also do `[1:]` or `[:3]`.
    * The modifications for the original array or the slice will affect each other.
    * `slice = append(slice, "foo", "bar", "foobar")`

4. Maps:
    * `var myMap map[string]int` then `myMap = make(map[string]int)`.
    * Or `myMap := make(map[string]int)`.
    * `myMap["newKey"] = 1` to insert and `myMap["newKey"]` to access.
    * Map literals:

        ```go
        ranks := map[string]int{"bronze": 3, "silver": 2, "gold": 1}
        // or multiline
        elements := map[string]string{
            "H": "Hydrogen",
            "Li": "Lithium",
        }
        emptyMap := map[string]int{} // empty
        ```

    * Use `ok` (a boolean value) to tell if it's a zero value: `grade, ok := grades[name]` then `if !go {}`.
    * Use `delete(myMap, "myKey")` to delete the pair.
    * for loops:
        * `for key, value := range myMap {}`
        * `for key := range myMap {}`
        * `for _, value := myMap {}`
    * `sort.Strings(names)` and then loop to have the sorted map.

## Structs

1. Example: (`go fmt` will format the spaces)

    ```go
    // Capitalize if we want to export the package
    // Lowercase if we want to make it private in its own package
    var MyStruct struct {
        Number float64
        Word string
        Toggle bool
    }
    MyStruct.number = 3.14
    // or define a type
    type Car struct {
        Name string
        TopSpeed float64
    }
    var myCar Car
    // or struct literal
    myCar := Car{Name: "Tesla", TopSpeed: 337}
    // Anonymous fields with just the type name is also allowed
    func (m *Car) myMethod() {
        fmt.Println("This is a method from Car %s", m)
        *m.TopSpeed = 1
    }
    func (m *Car) SetTopSpeed(speed float64) error { // must pass by reference
        if speed < 0 {
            return errors.New("Invalid speed")
        }
        m.TopSpeed = speed
        return nil
    }
    func (m *Car) TopSpeed() float64 { // Don't add Get to the name in the Getter
        return m.TopSpeed
    }
    ```

2. Interfaces:

    * Example:

        ```go
        type MyInterface interface {
            Foo()
            Bar(float64)
            FooBar() string
         }
        func (m MyType) Foo() {}
        func (m MyType) Bar(f float64) {}
        func (m MyType) FooBar() string {
        return "Now I'm complete"
        }
        // No further stuff needed. MyType automatically matches the interface now,
        // if it has defined all the methods.
        ```

    * Type assertion: `robot, ok := noiseMaker.(Robot)` (noiseMaker is the interface var).
    * The empty interface can accept any type. `func AcceptAnything(thing interface{})`

## Errors and Failures

1. Add `defer` to make sure a function call takes place, even if the calling function exist early (returned or panicking). `defer fmt.Println("I'm deferred to the end.")`.

2. `panic("I'm panicking")` to create a panic (crashing the program).

3. `recover` takes the return value of `panic` and exits the program normally.

    ```go
    func calmDown() {
        recover()
    }
    func freakOut() {
        defer calmDown()
        panic("Oh no")
        fmt.Println("I won't be run!")
    }
    func main() {
        freakout()
    }
    ```

    ```go
    func calmDown() {
        p := recover()
        err, ok := p.(error) // assert that it's an error
        if ok {
            fmt.Println(err.Error())
        }
    }
    func main() {
        defer calmDown()
        err := fmt.Errorf("There's an error")
        panic(err)
    }
    ```

4. This is intentionally designed in Go to be clumsy, which discourages its usage. Normally, we just need to do `log.Fatal(err)` and return in the functions.

    ```go
    if err != nil {
        log.Fatal(err)
    }
    ```

## Goroutines and Channels

1. Goroutines (`go` in front of function calls) enable parallelism (lightweight threads):

    ```go
    func main() {
        go a()
        go b() // can't deal with the return value
        time.Sleep(time.Second) // sleep for 1 second to let the routines finish
    }
    ```

2. Channels to enable the communication between goroutines.

    ```go
    func greeting(myChannel chan string) {
        myChannel <- "hi"
        // time.Sleep(time.Second) to synchronize
    }
    func main() {
        myChannel := make(chan string)
        go greeting(myChannel)
        fmt.Println(<-myChannel) // receives "hi"
    }
    ```

## Testing

1. Run `go test`

    ```go
    func TestFoo(t *testing.T) {
        t.Error("test failed")
    }
    ```
