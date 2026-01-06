`rustc`  - rust compiler
`rustup`  - rust updater
`cargo`  - rust build system and package manager

`println!()`  - rust macro , `\n` default

`cargo new basics`
`Cargo.toml`  - config file 
	
in rust packages of code are referred to as *crates*

`cargo build,run,check`


*prelude* - set of items by default in every programs scope

if it isn't in *prelude* , you can use `use std::io;` to bring it into programs scope

`fn main()`

`let apple = 5;`   - immutable
use `let` to create variables , by default variables & references in rust are immutable

`let mut guess = 5;`  - mutable

`String::new()` -  function that returns a new instance of a `String`

`::` - indicates that `new` is an *associated function* ( a function implemented on a type)
`::` - *path separator*

`io::stdin().read_line(&mut guess)`

`read_line()` - take user input from the standard input and append that into the string provided as argument.

`Result Ok  Err`

```rust
println!("x = {x} and y + 2 = {}", y + 2);
```

*crate* - collection of rust source code files. binary crate, library crate

#### variables and mutability

by default , variables are immutable.
you can make them mutable by adding `mut` in front of variable name
`println!("{x}")` - format argument must be a string literal
positional arguments are zero-based.

*constants* , 
are values that are bound to a name . always immutable
`const X_FIRST: u32 = 20;`
the type of the value *must* be annotated.

constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

constants are valid for the entire time a program runs, within the scope in which they were declared. not deallocated when a scope ends.
a `const` is a compile-time constant, not a runtime variable. compiler inlines the value wherever used. embedded directly into the generated code.

`const` can be used in global scope , `let` can only be used in a function

only functions marked `const` can be called at compile time to generate `const` values.
`const fn calc_val()->u8 {}`

variables can be *shadowed*.

shadowing in the same scope : old binding is dropped immediately
shodowing in a nested scope : inner binding ends when block ends.

over-shadowed variables occupy space in the stack frame. shadowing create a new binding in the current stack frame. shadowed variable becomes unreachable till it is.

a variable cannot be assigned a different type than that it was defined with.

rust is a *statically typed* language, must know the types of all variables at compile time

*singed int* starts with `i` , *unsigned int* starts with `u`

![[int_types_rust.png | 300]]

![[number_literals_rust.png|250]]

*tuples* 
have fixed length, supports different data types

`let tup: (i32,f64,u8) = (500,6.4,1);`
`let tup = (4,3,2);`
`let (x,y,z) = tup;`  - pattern matching to destructure a tuple
`tup.0`

`()`  - unit/empty tuple


*arrays* 
have fixed length, same data type
`let a = [1,2,3];`
`let a: [i32;5] = [1,2,3,4,5];`
`let a = [3;5];`  - 3 repeated 5 times 

both *tuple* & *arrays* are immutable by default in rust.

in `println!` , placeholder `"{}"` must be
- `{}` empty
- `{1}` indexed
- `{x}` named
not arbitary expression

`fn add(x:i32){}`

in function signatures, you *must* declare the type of each parameter.

- statements are instructions that perform some action and do not return a value
- expressions evaluate to a resultant value

expressions do not include ending semicolons

```rust
fn five() -> i32 {
	5
}
```

return value of the function is the value of the final expression in the block of function.
you can return early using `return` and specifying a value.
functions return last expression implicitly.

a curly-brace block `{ ... }` is a expression and a syntactic scope.

#### control flow
`if` condition *must* be a `bool`.

if is an expression.
`let x = if condition {5} else {6};`

blocks of code evaluate to the last expression in them, and numbers by themselves are also expressions.

results from each arm of `if` must be the same type.

`loop {}`
executes a block of code forever, until explicitly stopped.
`break continue`
every `break` with a value must return the same type.

```rust
let result = loop {
	counter +=1;
	if counter == 10 {
		break counter*2; // return this value from loop after break 
	}
};
```

loop labels

```rust
let ten = 'counting_up: loop {
		break `counting_up 10;
}
```

```rust
let value = 'block: {
    if true {
        break 'block 42;
    }
    0
};
```

`while cond {}`
`for ele in arr {}`
`for idx in (1..4).rev() {}`
`(1..=4)`


#### Ownership

each time an interpreted program reads a variable, then the interpreter must check whether that variable is defined.
runtime exceptions overhead.
rust performs these checks at compile time.

goal of rust is to ensure that programs never have undefined behavior.

rust provides a construct called `Box` for putting data on heap.
`let a = Box::new([0;1_000_000]);`


**box deallocation principle :**

If a variable owns a box, when Rust deallocates the variable’s frame, then Rust deallocates the box’s heap memory.


```rust
let a = Box::new([0;1_000_000]);
let b = a;
```

the statement `let b = a` **moves** ownership of the box from `a` to `b`.

```rust
fn main() {
    let first = String::from("Ferris");
    let full = add_suffix(first);
    println!("{full}");
}

fn add_suffix(mut name: String) -> String {
    name.push_str(" Jr.");
    name
}
```

here, in the function `add_suffix`, `name` is taking the ownership of the box pointed by first.

1. At L1, the string “Ferris” has been allocated on the heap. It is owned by `first`.
2. At L2, the function `add_suffix(first)` has been called. This moves ownership of the string from `first` to `name`. The string data is not copied, but the pointer to the data is copied.
3. At L3, the function `name.push_str(" Jr.")` resizes the string’s heap allocation. This does three things. First, it creates a new larger allocation. Second, it writes “Ferris Jr.” into the new allocation. Third, it frees the original heap memory. `first` now points to deallocated memory.
4. At L4, the frame for `add_suffix` is gone. This function returned `name`, transferring ownership of the string to `full`.

*all references becomes invalid after reallocation*.

***immutability*** applies to the binding, not to heap. whether you can modify the heap value depends on how you access it and who owns it.

the heap memory is inaccessible for mutation through a immutable binding. *moving ownership is different from borrowing*.

```rust
let s = String::from("kiara");
let mut t = s; //ownership moved
t.push('!'):
```

mutability is checked at the current owners binding, not at allocation time.

| Thing    | What `mut` affects                      |
| -------- | --------------------------------------- |
| `mut r`  | Can `r` be reassigned                   |
| `&mut T` | Can the **pointed-to value** be mutated |

**Moved heap data principle:** if a variable `x` moves ownership of heap data to another variable `y`, then `x` cannot be used after the move.

ownership is primarily a discipline of heap management.

- All heap data must be owned by exactly one variable.
- Rust deallocates heap data once its owner goes out of scope.
- Ownership can be transferred by moves, which happen on assignments and function calls.
- Heap data can only be accessed through its current owner, not a previous owner.

Rust does not in general try to determine whether if-statements will or won't execute. Rust just assumes that it _might_ be executed, and therefore `s` _might_ be moved.


#### References and Borrowing
	
```rust
fn main() {
    let m1 = String::from("Hello");
    let m2 = String::from("world");
    greet(&m1, &m2); // note the ampersands
    let s = format!("{} {}", m1, m2);
}

fn greet(g1: &String, g2: &String) { // note the ampersands
    println!("{} {}!", g1, g2);
}
```

![[references_rust.png|400]]

references are **non-owning pointers**. reference(or "borrow")

```rust
let mut x: Box<i32> = Box::new(1);
let a: i32 = *x;         // *x reads the heap value, so a = 1
*x += 1;                 // *x on the left-side modifies the heap value,
                         //     so x points to the value 2

let r1: &Box<i32> = &x;  // r1 points to x on the stack
let b: i32 = **r1;       // two dereferences get us to the heap value

let r2: &i32 = &*x;      // r2 points to the heap value directly
let c: i32 = *r2;    // so only one dereference is needed to read it
```


![[deref._rust.png|700]]

`&mut a` - exclusive borrow of `a` that allows modification, only **one** mutable reference to a value can exists at a time. you cannot have any immutable references alive at the same time.

`String` is not a pointer type `Box` is.

A reference's lifetime must be *entirely inside* the lifetime of the value it borrows.

```rust
let mut v: Vec<i32> = vec![1, 2, 3];
v.push(4);
```

**Pointer Safety Principle :**

data should never be aliased and mutated at the same time.

https://rust-book.cs.brown.edu/ch04-02-references-and-borrowing.html
the core idea behind the borrow checker is that variables have three kinds of **permissions** on their data:
- **Read** (***R***): data can be copied to another location.
- **Write** (***W***): data can be mutated.
- **Own** (***O***): data can be moved or dropped.

these permissions don't exist at runtime, only within the compiler. they describe how the compiler "thinks" about your program before the program is executed.

by default, a variable has read/own permissions(***RO***) on its data.If a variable is annotated with `let mut`, then it also has the write permission (***W***). The key idea is that **references can temporarily remove these permissions.**

creating a reference to data("borrowing" it) causes that data to be temporarily read-only until the reference is no longer in use.

mutable reference is created with the `&mut` operator.The type of `num` is written as `&mut i32`. Compared to immutable references, you can see two important differences in the permissions:

1. When `num` was an immutable reference, `v` still had the ***R*** permission. Now that `num` is a mutable reference, `v` has lost _all_ permissions while `num` is in use.
2. When `num` was an immutable reference, the place `*num` only had the ***R*** permission. Now that `num` is a mutable reference, `*num` has also gained the ***W*** permission.

think of reference as aliasing. `num  *num`

The first observation is what makes mutable references _safe_. Mutable references allow mutation but prevent aliasing. The borrowed place `v` becomes temporarily unusable, so effectively not an alias.

The second observation is what makes mutable references _useful_. `v[2]` can be mutated through `*num`. For example, `*num += 1` mutates `v[2]`. Note that `*num` has the W permission, but `num` does not. `num` refers to the mutable reference itself, e.g. `num` cannot be reassigned to a *different* mutable reference.

---

LLVM - optimization and code generation

`let x:i64 = 123_i32;`
`let x: i32 = 3_000_000_000_i64 as i32;`
rust does not check overflow for `as` casts at compile or runtime.

rust will look at how the variable is *used* to determine the type.

| Operation       | Overflow behavior | Extra info            |
| --------------- | ----------------- | --------------------- |
| `overflowing_*` | Wraps             | Returns overflow flag |
| `wrapping_*`    | Wraps             | No flag               |
| `saturating_*`  | Clamp             | No wrap               |
| `carrying_add`  | Wraps             | Carry in/out          |
| `borrowing_sub` | Wraps             | Borrow in/out         |
| Default `+`     | Panic / Wrap      | Build-dependent       |

`assert_eq!(a,b);`
evaluates both expressions, checks `a==b`. if false, the program panics and prints both values.

`todo!("todo")` - not yet implemented, program panics

- a block in rust contains a sequence of expressions, enclosed by braces `{}`
- the final expression of a block determines the value and type of the whole block.
- if the last expression ends with `;` , then the resulting value and type is `()`.

`dbg!(exp)`
macro used for debugging. it prints the value of an expression along with filename and line number and returns the value of the expression.
prints to `stderr`.

every process gets its own standard streams in memory buffer.
accesible via file descriptor table. these descriptors points to kernel-managed file descriptions representing underlying device or file. user-space buffers.

memory may still exist for a out-of-scope variable, but rust considers the value *deallocated* and prevents access.

mutability of the *binding* and mutability of the *data* are different things.

**Match Expression**

```rust
match val {
	1 => println!("one"),
	10 => println!("ten"),
	_ => println!("something"),
}
```

*labels* can be attached to arbitrary blocks, not just loops.

unreachable code is compile time error.

**Macros`!`**

macros are expanded into rust code during compilation, and can take a variable number of arguments.
- `format!(format,..)` - works like println! but return result as string.
- `unreachable!("msg")` - panic if executed, eg. `_` in `match`.
- `eprintln!(format,..)` - prints to `stderr`
- `assert!(cond)` - panics if false


if the compiler can prove the access is safe, it removes the runtime check for better performance.

`{arr:?}` gives the debug output. `{:#?}` - pretty printing format.

`let (x,y,z) = tuple` - pattern matching

*patterns* can be used directly inside conditionals `if` `while` `match` to both check the condition and extract values in a single step.

**references**

references provides a way to access another value without taking ownership of the value, and is also called *borrowing*.

a borrow lasts until its last use. *non-lexical lifetimes*

`& T` - shared reference
`&mut T` - exclusive reference

*rule :*
while an immutable borrow exists, the borrowed place must not be mutated.

when taking a immutable reference to some object , we assume that borrowed place won't change, if we allowed mutation then if some thread changed the value, then this might create undesired results.

references can never be *null* in rust.

a shared reference does not allow modifying the value it refers to, even if that value was mutable.

**slices** 

a slice gives you a view into a larger collection.
slices always borrow data from the sliced type.

```rust
let a: [i32;5] = [10,20,30,40,50];
let s: &[i32] = &a[2..4];
println!("{s:?}"); // [30,40]

&a[..] //slice of complete array
```

slices can't be grown once created.

**strings**

`&str` is a slice of UTF-8 encoded bytes, similar to `&[u8]`
`String` is an owned buffer of UTF-8 encoded bytes, similar to `Vec<T>`

```rust
let s1: &str = "World"; // lives in program's read only memory
let s = String::from("hello");
let slice: &str = &s[1..]; // borrows String s , "hello" on heap
s.push_str("kiara");
```

`format!()` - generate an owned string from dynamic values.

```rust
    println!("{:?}", b"abc"); // byte string literal  [97,98,99]
    println!("{:?}", &[97, 98, 99]);  // of type &[u8]
	println!(r#"<a href="link.html">link</a>"#); // raw string #-delimeter for ""
```


`for i in v.iter() &v` `v.iter_mut() &mut v` 


**user defined types**

```rust
struct Person {
	name: String,
	age: u8,
}
let mut person: Person = Person {  // named fields
		name: String::from("kiara"),
		age: 25,
}
let waifu = Person{name,age}; //variable names should match field
```

tuple structs, usually used for single-field wrappers

```rust
struct Newtons(f64);
struct Person (String,u32);  //tuple struct
let waifu = Person(String::from("kiara"),25); // fields accessed via .0 .1

let kiara = Person{..person};
```


**Enums**

enumerations allow you to collect a set of values under one type.
variations

*mental model* - "a value of this type is exactly one of these possibilities."

```rust
enum IpAddrKind {
	V4(u8,u8,u8,u8),  // stores 4 u8 ints
	V6, // None data
}

let localhost: IpAddrKind = IpAddrKind::V4(192,168,0,1)
```


*option enum* 

used if a value can be something or NULL.
included in programs scope by defualt

```rust
enum Option<T> {  // enum for something or nothing. rust doesn't have null
	Some(T),
	None,
}
let x: Option<i32> = Some(5);
let y: Option<i32> = None;
let z: i32 = 10 + y.unwrap_or(default: 0);
```

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i: i32) => Some(i+1),
	}
}
```

*static* 
variables will live during the whole execution of the program.

`for i in vec` - `vec` is no longer usable after the loop. giving ownership of each element 1 by 1 to i & gets dropped after each iteration. ownership of `vec` moved into the loop, becomes unusable at the start of the loop. vec gets dropped at the end of loop.

`let output: Vec<i32> = input.iter().map(|e| e+1).collect();`

```rust
enum Message {
    Quit,
    Write(String),
    Move { x: i32, y: i32 },
    ChangeColor(i32, i32, i32),
}

fn process(msg: Message) {
    match msg {
        Message::Quit => println!("Quit requested"),
        Message::Write(text) => println!("Message: {}", text),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::ChangeColor(r, g, b) => println!("Color: {}, {}, {}", r, g, b),
    }
}
```

`match` must be exhaustive , every possible variant must be handled.

null pointer bugs are impossible, use `Option<T>  Some(T)  None`

*enum for errors*

```rust
enum Result<T,E> {
	Ok(T),
	Err(E),
}
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err("division by zero".to_string())
    } else {
        Ok(a / b)
    }
}
match divide(10, 2) {
    Ok(v) => println!("result = {}", v),
    Err(e) => println!("error: {}", e),
}
```

| Struct                            | Enum                         |
| --------------------------------- | ---------------------------- |
| All fields exist at once          | Exactly one variant exists   |
| Represents “has these properties” | Represents “is one of these” |
| Good for records                  | Good for states / choices    |


```rust
enum TrafficLight
    Red,
    Yellow,
    Green,

impl TrafficLight 
    fn duration(&self) -> u32 
        match self 
            TrafficLight::Red => 60,
            TrafficLight::Yellow => 5,
            TrafficLight::Green => 45,
```

##### Pattern Matching

```rust
let (a,b,c) = tuple;
let (_,b,c) = tuple;
let (..,c) = tuple;  // .. can only be used once per tuple pattern
let [first,..,last] = array;
```

*match*

```rust
	    let input = 'x';
    match input {
        'q'                       => println!("Quitting"),
        'a' | 's' | 'w' | 'd'     => println!("Moving around"),
        '0'..='9'                 => println!("Number input"),
        key if key.is_lowercase() => println!("Lowercase: {key}"),
        _                         => println!("Something else"),
    }
```

here *key* is a pattern binding. it matches any value that reaches this arm. `if key.is_lowercase()` is a *match gaurd*.

- The condition defined in the guard applies to every expression in a pattern with an `|` as a whole.

```rust
match x 
    key @ ('a' | 'b' | 'c') if key != 'b' => println!("a or c"),
    _ => println!("other"),
    
let opt = Some(123);
match opt {
    outer @ Some(inner) => {
        println!("outer: {outer:?}, inner: {inner}");
    }
    None => {}
}

```


*match on struct*

```rust
struct Foo
    x: (u32, u32),
    y: u32,
    
let foo = Foo { x: (1, 2), y: 3 }
    match foo {
        Foo { y: 2, x: i }   => println!("y = 2, x = {i:?}"),
        Foo { x: (1, b), y } => println!("x.0 = 1, b = {b}, y = {y}"),
        Foo { y, .. }        => println!("y = {y}, other fields were ignored"),
```


`&String` is automatically coerced to `&str` by compiler.
`String` is heap allocated.
`trim()` - removes whitespaces, return `&str`


**let control flow**

`if let Some(x) = result {}`

unlike `match`, `if let` does not have to cover all branches.

*let else*

```rust
    let Some(s) = maybe_string else {
        return Err(String::from("got None"));
    };
```

the *else* case must diverge (`return`,`break`, or panic - anything but falling off the end of block).

**Method and Traits**

rust allows us to associate functions with new types. by using `impl`.

```rust
struct CarRace {
    name: String,
    laps: Vec<i32>,
}
impl CarRace {
    // No receiver, a static/associated function
    fn new(name: &str) -> Self {
        Self { name: String::from(name), laps: Vec::new() }
    }
    // Exclusive borrowed read-write access to self
    fn add_lap(&mut self, lap: i32) {
        self.laps.push(lap);
    }
    // Shared and read-only borrowed access to self
    fn print_laps(&self) {
        println!("Recorded {} laps for {}:", self.laps.len(), self.name);
        for (idx, lap) in self.laps.iter().enumerate() {
            println!("Lap {idx}: {lap} sec");
        }
    }
    // Exclusive ownership of self (covered later)
    fn finish(self) {
        let total: i32 = self.laps.iter().sum();
        println!("Race {} is finished, total lap time: {}", self.name, total);
    }
}
fn main() {
    let mut race = CarRace::new("Monaco Grand Prix");
    race.add_lap(70);
    race.add_lap(68);
    race.print_laps();
    race.add_lap(71);
    race.print_laps();
    race.finish();
    // race.add_lap(42);
}
```

associated functions are accessed using `::` 
`new` has no `self` parameter, so it is an associated function. which means it belongs to the *type*, not an instance.

- `::` - associated functions/constansts/types
- `.` - methods that take `self`,`&self`,`&mut self`

the `self` arguments specify the object the method acts on.

- `&self` - borrows the object from the caller using a shared and immutable reference. the object can be used afterwards.
- `&mut self` - borrows the object using a unique and mutable reference. can be used after wards.
- `self` - takes ownership of the object and moves it away from the caller. the object will be dropped when the methods returns unless its ownership is explicity transmitted. complete ownership doesn't automatically mean mutibility.
- `mut self` - same as above, but the method can mutate object.
- no receiver - becomes a static/associated method on the struct.

`String`,`Vec` are stack handles pointing to heap data.

methods can be also be called like associated functions by explicitly passing the receiver `CarRace::add_lap(&mut self,20)`

`self` is an abbreviated term for `self: Self`, struct name could also be used.
`Self` is a type alias for the type the `impl` block is in.

**Traits**

```rust
trait Pet {
	fn talk(&self) -> String;
	fn greet(&self);
}
```

- a trait defines a number of methods that types must have in order to implement the trait.

```rust
trait Pet {
    fn talk(&self) -> String;
    fn greet(&self) {
        println!("Oh you're a cutie! What's your name? {}", self.talk());
    }
}
struct Dog {
    name: String,
    age: i8,
}
impl Pet for Dog {
    fn talk(&self) -> String {
        format!("Woof, my name is {}!", self.name)
    }
}
fn main() {
    let fido = Dog { name: String::from("Fido"), age: 5 };
    dbg!(fido.talk());
    fido.greet();
}
```

to implement Trait for type, you can use `impl Trait for Type {..}` block.

traits may provide default implementations of some methods and they can rely on all the methods in a trait.

need to implement *all non-default methods* of a trait
trait implementations are *per-type*.

*super-traits*
```rust
trait Pet: Animal + Owner {  // types impl Pet must also impl Animal Owner
    fn name(&self) -> String;
} 
```

*associated types*

```rust
struct Meters(i32);
struct MetersSquared(i32);

trait Multiply {
    type Output;
    fn multiply(&self, other: &Self) -> Self::Output;
}
impl Multiply for Meters {
    type Output = MetersSquared;
    fn multiply(&self, other: &Self) -> Self::Output {
        MetersSquared(self.0 * other.0)
    }
}
fn main() {
    println!("{:?}", Meters(10).multiply(&Meters(20)));
}
```

*deriving*

```rust
#[derive(Debug, Clone, Default)]
struct Player {
    name: String,
    strength: u8,
    hit_points: u8,
}

fn main() {
    let p1 = Player::default(); // Default trait adds `default` constructor.
    let mut p2 = p1.clone(); // Clone trait adds `clone` method.
    p2.name = String::from("EldurScrollz");
    // Debug trait adds support for printing with `{:?}`.
    println!("{p1:?} vs. {p2:?}");
}
```

