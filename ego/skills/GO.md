ref : *the go programming language - kernighan*

###### chapter 2 : program structure

**2.1. Names**

a name begins with a letter or a underscore _ 

entity declared in a function is local to that function, else it is available to all files in the current package. eg. global variables/functions.

if a name begins with a upper-case letter it is exported, meaning it can be used outside its package by importing the package. package names are always in lower-case. eg fmt.Printf

camelCase , acronyms as it is eg escapeHTML
  
**2.2 Declarations** 

package name -> imports -> package level declarations

only in `package main` the go runtime calls `main()` automatically.

keep all related files together in one folder as one package.
use different folders for logically different packages.
keep package main in root folder for your executable.
package name is tied to folder not to individual files.
all files in the same package must be in the same directory else compiler treat them as diff package


all files in a folder must declare the same package.

func -> list of parameters -> list of results -> function body

`import "fmt"` this works because its part of the standard go library and go compiler knows its path.

`import _ "fmt"`

**2.3 Variables** 

`var name type = expression` 

either the *expression* or *type* part may be omitted, but not both. if the *type* is omitted its initial value is determined by initializer expression and if the *expression* is omitted its initial value is *zero value* of the *type*.

`var i,j,k int`
`var b,f,s = true,2.3,"four"`

within a function, you can use short variable declaration, 

`name := expression`
the type of name is determined by the type of expression

`:=` is a declaration and `=` is a assignment

you can reassign a variable using `:=` but at least one new variable should also be declared, else "no new variables on left side of :=" error.

*pointers*-

we can return address of a local variable , it will remain in existence even after the call has returned.

*new*- 

the expression `new(T)` creates a *unnamed variable* to type **T** , initialized it to zero value of **T** , and returns its address, which is a value of type `*T` .

`struct{}  [0]int` has 0 size.

compiler may choose local variables to be allocated on heap or stack.

*tuple assignment*- 

`i,j = j,i` , all of right-hand side is evaluated first before variables are updated.

type declarations - `type name underlying-type`

`func (c Celsius) String() string{}`
`c.String()`

method for type Celsius

it is an error to import a package and then not refer to it.

`func init() {}`  - called automatically before main()

dependencies are initialized first.
at package level the order in which declarations are made has no effect.

`a = b*4`
`b = 5`

unused variable -> error

###### chapter 3 : basic data types

*rune - int32*  , unicode code point
*byte - uint8*   , raw data

untyped constants are context dependent and can change their type 3/4.5

`fmt.Printf("%d %[1]o %#[1]x",val)`

`[1]` - indicates that use first operand again.
`#` - print 0x and 0 for octal,hex

*rune* literals are written as character with single quotes, %c or %q with quotes

*strings* , sequence of *runes*
len(string) - number of bytes and not no. of characters
a character can be more than a byte

`s[:5]`

strings are immutable, but a variable can be assigned a diff. string
substring and copying of strings uses the same underlying memory making them cheap. 

```
`...`  - raw string literal
```

constant generator *iota* , starts from 0,1,2 ...

###### chapter 4 : composite types

*arrays*-

`var a [3]int` 

array literal, `var a [3]int = [3]int{1,2,3}`

use `%T` to print type of a variable

*slice* - `[]T`

`make([]T,len,cap)`

use `append(s,1,2,3)` to increase length/capacity, returns pointer to a slice. (must capture it if reallocation happens, good practice to always use `s = append(s,1,2,3)`.

`copy(dest,src)` - used to copy slices , overwrites data starting from index 0.
returns number of elements copied

reallocation creates a new underlying array and copies elements of prev. array into it.



*variadic argument*-  $\ldots$    `s = append(s,t...)` 

`func hello(x []int,y ...int){}`

`stack = stack[:len(stack)-1]`   // pop

`t...` to spread a slice **t** .

`...` must be the last argument

*map* - unordered collection of key/value pairs, where keys are distinct.

`map[K]V` , key type must be comparable

`ages := make(map[string]int)`

`delete(ages,"alice")`

if a key is not present in a map its default is *zero-value* of the value type.
`ages["bob"]++`   is valid.  // 1

`for i,val := range slice {}`
`for key,val := range map{}`

`age,ok = ages["bob"]`, use ok to test whether a key is present in the map or not

*structs*-

`type Person struct{ a,B int}`

to export a struct *field* use capital letter.

*struct embedding*, refer to the names at the leaves of the implicit tree without giving the intervenning names.

`w.Circle.Center.X` can be written as `w.X` by declaring *anonymous field* in structs

```
type Point struct{
X,Y int
}
type Circle struct{
Radius int
Point
}
type Wheel struct{
Circle
spoke int
}
```

 \` \` - raw string literal

*json* - 

`json.MarshalIndent(slice,prefix4eline,identstring)`

*field tags*,  is a string of metadata associated at compile time with the field of a struct. space separated key-value pairs. can be any literal string.

```
Year int `json:"released,omitempty"`
```

`json.Unmarshal(data,&destDs)`

you can choose which field to unmarshal by just defining the req. fields in struct.


###### chapter 5 : Functions

```
func name(parameter-list) (result-list){
body
}
```

results may be named. initialized to *zero-value* for its type.
return without explicit value just returns the *named return variable*.
arguments are passed by value, except slices,maps,function,pointer,channels.

need to explicitly close open files and network connections.

`fmt.Errorf`

functions are *first class* values in go , they have types and maybe assigned to variables or passed to or returned from functions. zero-value for func in *nil*.

*anonymous functions* can be nested inside a function. *closure*
`inside := func(y int) int{ return x+y}`

named functions can only be declared at package level.
to use recursion with anonymous function , define it first

each closure captures the same variable *i* , not its value at that moment.

*defer*, is ordinary function or method call prefixed by keyword defer.
the function and arguments are evaluated when the statement in executed ,
but the actual call is *deferred* until the function containing it comes to an end.
deferred calls are executed in reverse order.

anonymous functions capture variable by reference, so value at time of execution matters.
careful when using defer in loop, use a function with defer to do the job instead inside loop 

if *recover*, function is called within a deferred function and function containing defer statement is panicking , recover ends the current state of panic and return the panic value. it return *nil* at other times.
`if p := recover(); p != nil{}`

###### chapter 6 : Methods

`func (p Point) Dist(q Point) float64{}`
here, p is the receiver. Dist is a method of the Point type
`p.Dist(q)`

each type has its own namespace for methods

you can define methods for any named type, except for underlying types pointer and interfaces
`type arr []int`

pass address of a variable using pointer to the method to modify value and avoid copying

if receiver argument is a *variable* of type T and receiver parameter is `*T` , the compiler implicity takes the address of the variable `p.Scale(2)`  implicit `&p`

if receiver argument has type `*T` and receiver parameter has type T, then
`pptr.Scale(2)` implicit `(*pptr)`

###### chapter 7 : Interfaces

to implement a interfaces , define all the methods of the interface.

###### chapter 8 : go routines and channels

*main* is also a go routine
new goroutines are created by the *go* statement.
ordinary function or method prefixed by keyword *go*.

when *main* returns , all goroutines are terminated and program exits.

a *channel* is a communication mechanism that lets one goroutine send values to another goroutine.

`ch := make(chan int)`

`ch <- x`  send 
`x = <-ch` receive
`<-ch`  discard result

`close(ch)` - no more values will be sent on this channel, if send then panic
receive works on closed channel until no more values left, then nil values of channel type

`ch = make(chan int,3)`   buffered channel of capacity 3

*unbuffered channels*,
a send operation on an unbuffered channel blocks the sending goroutine until another goroutine executes a corresponding receive on the same channel. similarly, if receive operation is performed first, it is blocked until send in performed on the same channel.

*done channel*,
`done := make(chan struct{})`
`done <- struct{}{}`  send signal to close
`<-done` received signal ,  waits coz its unbuffered channel

*pipelines*, channels can be used to connect goroutines so output of one is input of another.

range on channels,

if channel is not closed and empty, loop waits to receive
if channel is closed and empty, loop exits

`chan<-int`   send only channel
`<-chan int` receive only channel

compile error if closing receive only channel

*buffered channel* , pointer to a buffer in memory , behaves like a queue
send/receive are blocked if channel full/empty.

*goroutine leak*, stucked goroutines are not automatically garabage collected

`var wg sync.WaitGroup`
`wg.Add(1)`
`wg.Done()`

```go
go func(){
wg.Wait()
close(ch)
}()
```

*select*, multiplex operations on channels
it waits until a communication for some case is ready to proceed.
select picks one case that is ready randomly
if no case is ready it runs default if present
if no default, then it blocks until one case is ready

`var mu sync.Mutex`
`mu.Lock()`
`defer mu.Unlock()` // imp if CS panics

`sync.RWMutex()` - multiple readers single writer lock

`var load sync.Once`
`load.Do(f)` f is some function, it will be called only once even if there are multiple goroutines calling it. after first execution, all other calls are ignored

goroutines stack is not fixed in size, it grows and shrink as needed.
go runtime has its own scheduler that uses *m:n scheduling* technique. 
maps *m* goroutines to *n* os threads.
doensn't need a switch to kernel context.

GOMAXPROCS is the *n* 

---

when using unbuffered channels , the send operation blocks the enclosing functions until the value if recieved by a receiver. else deadlock

`interface{}` - empty interface with no methods. meaning any value in go satisfies this interface.
this way interface{} can represent any type.
any value of any type can be assigned to a variable of type interface{}

---
**RPC**

```
service PrimeService{
rpc ListPrimes(Input) returns (Output);
}                                        - define a service in .proto

message Input{
int32 Num=1;
}                                        -define a message in .proto
```

use `repeated int32 Nums = 1;` to return $\geq 0$ results

to send/receive a streaming message use `stream` keyword before message type `(stream Input)`.

`option go_package = "path_to_protofiles;package_name"`

path and package name defined here would be used to import the generated code files to use rpc methods. import the generated code using the path provides then use the package_name to use them.

`syntax = "proto3";`

`.pb.go`  - message structs, marshal/unmarshal helpers.
`_grpc.pb.go`  - interfaces for your service (server and client stubs)



`os.Getenv("PORT")` - returns value as a string

`lis,err := net.Listen("tcp",":"+port)`

`log.Fatalf()`  - log an error message and immediately stop the program

`s := grpc.NewServer()` - creates  a new grpc server instance

`pb.RegisterWorkerServer(s,&WorkerServer{workerID:workderID})` - now server s knows which functions handle which rpc calls

`s.Serve(lis)` - listen for grpc calls on address lis

`var mu sync.Mutex` 
`mu.Lock()` - acquire the lock (waits if someone else has it)
`mu.Unlock()` - release the lock

`time.NewTicker(d)`   - repeats every d ; sends the current time repeatedly to `ticker.C`
`time.NewTimer(d)` - fires once after d
`time.Sleep(d)` - blocks for d




