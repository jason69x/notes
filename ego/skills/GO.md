
8core,16threads means there are 8 physical cores and 16 physical threads . 2 per core. kernel threads are mapped to these threads. from os perspective there are 16 cpu's and it can schedule kernel thread to any of these logical "cores". its helpful because it a thread is blocking cpu can use another thread to execute. this improves cpu utilization.

*go routines* , go routines are independently executing functions that are mapped to kernel threads by multiplexing . they are handled by the *go runtime*. go runtime handles everything from scheduling to preemption. stack size is handled by go runtime.

*channels* . a channel is used to send and receive data between go routines.
reading from a channel is blocking. a function waits till it receives data from a channel or till the channel is closed.

unbuffered channels only allow synchronous communication between go routines. that is a sending routine is blocked after sending data to the channel until the data is received by other go routine and then it is unblocked.

buffered channels are asynchronous

unbuffered channels do not stores anything since they are unbuffered, go runtime transfers the data directly from sender to receivers memory.

---

go is a compiled language . it first compiles the given code into machine code and executes it sequentially.

`go run` - compiles and runs the program
`go build` - compiles and save the executable
`go get` - download libraries
`go install` - install packages


`package main` declares which package the current file belongs to. eg. main.

main package must contain a main function. package main defines a standalone executable program, not a library.

```go
import (   // used to import other packages (libraries)
"fmt"
"math"     // must contain packages that are or will be used
)
```

`func func_name(x int,y float)(z float){}`

opening brace should be on the same line as the function signature (or control statement)

go only has postfix and they are statements and not expressions

`j = i++` is not valid   &   `++i` is not valid

`:=` can only be used within a function
`var s string` can be used for package level variables

`_`   blank identifier

```go
start := time.Now()
time.Sleep(2 * time.Second)
elapsed := time.Since(start)
fmt.Println(elapsed.Nanoseconds())
```

Buffered i/o is fast because it decouples sender and receiver timings. sender can send data to a in-memory buffer (user space) and flush this data in large chunks or when it is full. this reduces total system calls made. in unbuffered io sender needs to wait for the receiver to get ready and receive complete data before sending more. 

`os.Open("input.txt")`
`file.Close()`
`scanner := bufio.NewScanner(file)`

```go
for scanner.Scan(){   // scan a line and removes \n from it returns true if                                 there is a line else false
line := scanner.Text()  // retrieve scanned text
}
```

`%v - any value in natural format` 
`%T - type of any value`

`value,exists = myMap[key]` 

`const` must be assigned a value known at compile time. number,string,bool

`[]color.Color{color.Pink,color.White}`
`gif.GIF{x:2,y:3}`     notation to instantiate composite types

`numbers = append(numbers,10,30)`

no defer functions runs after `os.Exit()` call

if you don't close the channel for range loops run forever

closing a channel is broadcast, every receiver gets notified

WaitGroup -> to wait for all workers to finish gracefully
Done Channel -> to cancel workers early if needed

chan struct{} takes 0 bytes of memory

```go
var makimaFeelings string = "i love you"
```

either of type of expression part may be omitted but not both. if type is omitted it is determined by initializer expression, if initializer is omitted then variable's initial value is *zero value* of the type.

*zero values* - 0 , false , "" , nil , \[0,0,0] , { x : 0, y : "" }

`:=` is a declaration `=` is a assignment

`:=` contains existing variables on lhs then it will only assign a new value to those vars. lhs must have at least one new var else compile error

functions can return address of local variables. these variables will be shifted to the heap from the stack. compiler's *escape analysis* does this job.

`new(int)` returns address of unnamed variable of type int

compiler decides wheter some variable will go on heap or stack.

