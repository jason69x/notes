`rustc` - rust compiler `rustc hello.rs` -> hello*

ahead-of-time-compiled language, meaning you can give a compiled program to someone and it will run without installing rust

`cargo` - used for building code, downloading and building dependencies
`cargo new basics` -> cargo.toml & src->main.rs, also inits git
`cargo build` `cargo run` `cargo update` `cargo check`

`TOML` - toms obvious, minimal language `Cargo.toml`

use top-level directory for configs,readme and src for code

`Cargo.lock` - records the exact version of every dependency that were used when your project last successfully built. gives us reproducible/deterministic builds.
#### 3. Common Programming Concepts
###### 3.1 variables and mutability
by default, variables are immutable.
you can make them mutable by adding `mut` in front of variable name.

`const` constants are values that are bound to a name and are not allowed to change. always immutable. their type must be annotated.
can be set to only constant expressions, not the result of a value that can only be computed at runtime.
`const THREE_HOURS_IN_SECONDS: u32 = 3*60*60;`

constants are valid for the entire time a program runs, within the scope in which they were declared. can be declared in global or function scope. inlined by the compiler. references to const live in static memory.

`let` can only be used inside a function

variables can be shadowed.
```rust
let spaces = "    ";
let spaces = spaces.len(); // spaces is still immutable, we just transforming it
```
###### 3.2 Data types
`i32 u8 f64`
`char` - 4 bytes , unicode scalar value
`let x: char = '\u{D7FF}'` 

tuple, 
used to group values of different types together.
tuples have fixed length.
```rust
let tup: (i32, f64, u8) = (500,6.4.1);
let (x,y,z) = tup; // destructure by pattern matching
let x = tup.0;     // or access by index
```

`()` - unit
expressions implicitly return unit value if they don't return any other value.

array,
group values of same type. fixed length.
```rust
let arr: [i32;5] = [1,2,3,4,5];
let arr = [3; 5];  // [3,3,3,3,3]   [val; times]
let first = arr[0];
```
###### 3.3 Functions
`fn my_func(x: i32, y: char) {}`
function signatures must declare the type of each parameter.
function bodies are made up of a series of statements optionally ending with a expression.

- statements are instructions that perform some action and do not return a value.
- expressions evaluate to a resultant value
```rust
let y = {
		let x = 3;
		x+2 
	};  // this expression returns 5, y = 5;
```

`fn fun(x: i32) -> i32 {x+3}`
functions return last expression implicitly or you can early `return`
if last statement is not expression `()` is returned

comments - ` //  /* */` 
###### 3.5 Control Flow
`if` conditions must be a `bool`

because `if` is an expression, we can use it on right side of `let` statement to assign the outcome to a variable.
`let var = if condition {} else {};`

`if` and `else` should result in same type, coz rust needs to know type of each variable at compile time. so it can verify the type is valid everywhere. else compiler will be more complex.

`loop {}` - $\infty$ 

you can return a value using `break` ` let x = loop { break 5; }`

```rust
`counting_up: loop { // loop labels
		loop{
			break `counting_up;
		}
}
```

`while cond {}`

```rust
for ele in arr {
}
for num in (1..4).rev(){
}
```
#### Understanding Ownership
###### 4.1 What is Ownership?
ownership, discipline for ensuring safety of rust programs.
safety is the absence of undefined behavior.

rust checks if a variable is defined or not at compile time, this reduces runtime exceptions.

foundational goal of rust is to ensure that your programs never have undefined behaviour and to catch them at compile time.

variables live in frames.
a frame is a mapping of variables to values within a single scope.
frames are organized into a stack of currently-called-functions.

when expressions reads a variable, the variable's value is copied.
```rust
let a = 5;
let mut b = a;
b+=2;
```

to transfer access to data without copying it, rust uses pointers.

rust provides a construct called `Box` for putting data on heap.
```rust
let a = Box::new([0;1_000_000]); // a is pointer to data on heap
let b = a;  // copies pointer from a to b, a is moved
```

`Box::new(T)` first creates `T` on stack then move it to heap.

frames in the stack are associated with a specific function, and are deallocated when the function returns.

rust does not allow manual memory management.

*box deallocation principle* 
if a variable owns a box, when rust deallocates variable's frame, then rust deallocates the box's heap memory.

rust checks at compile time whether we are accessing deallocated memory.

*moved heap data principle*
if a variable `x` moves ownership of heap data to variable `y` then `x` cannot be used after the move.

copy vs move depends on type.

rust does not try to determine whether if-statements will or will not execute. rust assumes it might execute. therefore s might be moved.

ownership is primarily a discipline of heap management.

- all heap data must be owned by exactly one variable.
- rust deallocates heap data once its owner gets out of scope.
```rust
b = Box::new(3);
    {
        b = Box::new(2);  // old heap data is dropped immediately
    }
```
###### 4.2 References and Borrowing
references are non-owning pointers.
`&x` - reference/borrow to x
`&String` - reference to a `String`

```rust
let mut v: Vec<i32> = vec![1,2,3]; // must be initialized vec![]
v.push(4);
```

*pointer safety principle*
data should never be aliased and mutated at the same time.

rust maintains safety of references via borrow checker.

variables have 3 kinds of permissions on their data:
- Read: data can be copied to another location
- Write: data can be mutated
- Own: data can be moved or dropped

by default, a variable has read/own permission on its data.
if using mut it also has write permission.
the key idea is that references can temporarily remove these permissions.

`num = &v[2]` 
v cannot be mutated or dropped while num is in use.
places lose permissions when they become unused.

creating a reference to data ("borrowing") causes that data to be temporarily read-only until the reference is no longer in use.

- shared references `&x` 
- exclusive references `&mut x`

data must outlive any references to it.
###### 4.3 Fixing Ownership Errors
if a value does not own heap data, then it can be copied without a move.
`get_first(&name)`
rust only looks at the type signature, which just says "some String in the input gets borrowed". rust conservatively decides then that both `name.0` & `name.1` get borrowed, and eliminates write and own permissions on both.
![[rust_permissions.png|400]]
###### 4.4 The Slice Type
slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.
a slice is kind of a reference, so it is a non-owning pointer.

a string slice is a reference to part of a `String`
`let hello: &str = &s[0..5];`

`String` metadata is stored on stack, underlying `Vec<u8>` vec of bytes

`s[..5]  s[0..]  s[..]`

`s.as_bytes()` - produces immutable reference to `s`
`for (i,&byte) in bytes.iter().enumerate()`

string literal are slices, & immutable

`&[i32]`

a slice is represented as a "fat pointer" which contains a pointer to the beginning of the range and a length of the range.
one advantage of slices over index-based ranges is that the slice cannot be invalidated while it's being used.

`&String` - 8bytes, pointer
`&str` - 16bytes, pointer + length
###### 4.5 Ownership Recap
garbage collector works at runtime.
the collector scans through memory to find data that's no longer used - that is, the running program can no longer reach that data from a function-local variable. then the collector deallocates the unused memory for later use.

GC pause the execution of the program and checks from root for unreachable objects and free's them (memory is simple marked as available for the runtime) and then it starts the execution again.

after a scan it focuses only on regions that might contain new references.

reference counting, each object keeps a counter of how many references currently point to it. when the counter reaches zero, the object is freed immediately

rust's ownership model puts pointers front-and-center.
- improves runtime performance by avoiding garbage collection
- improves predictability by preventing accidental "leaks" of data


ownership at runtime

- rust allocates local variables in stack frames, which are allocated when a function is called and deallocated when the call ends.
- local variables can hold either data or pointers.
- pointers can be created either through boxes(pointers owning data on the heap) or references(non-owning pointers).

![[ownership_review.png|400]]
![[slices_review.png|320]]

a move of a variable with a non-copyable type(like `Box<T>` or `String`) requires the (**RO**) persmissions, and the move eliminates all permissions on the variable. this rule prevents the use of moved variables.

borrowing a variable(creating a reference to it) temporarily removes some of the variable's permissions.

locks work at runtime, borrow checker checks for bugs at compile time.

immutable reference disables the borrowed data from being mutated or moved. (**R**)
mutating an immutable reference is not ok.
mutating the immutably borrowed data is not ok.
moving data out of the reference is not ok.

mutable reference disables the borrowed data from being read,written or moved.
mutating a mutable reference is ok.
accessing the mutably borrowed data is not ok.

![[ownership_example.png|300]]
rust normally disallows multiple mutable accesses to the same array, even when those accesses are disjoint.
`let (r0,r1) = (&mut v[0..2],&mut v[2..4];`
`v.split_at_mut(2)`

target string does not need to be heap allocated, use `&str` rather than `String`

```rust
fn get_first(strs: &mut (String, String)) -> &mut String 
    &mut strs.0
    
fn get_second(strs: &mut (String, String)) -> &mut String
    &mut strs.1

fn transfer_string(strs: &mut (String, String))
    let fst = get_first(strs);
    let snd = get_second(strs);
    fst.push_str(snd);
    snd.clear();
```
rust rejects this function, because both functions take `&mut (String,String)` and return a `&mut String`

from the compilers perspective
- `fst` is a mutable borrow of `strs`
- `snd` is also a mutable borrow of `strs`

rust cannot prove that these two mutable references point to different fields.

`fn transfer_string((fst,snd): &mut (String,String))`
#### Using Structs to Structure Related Data 
a structure, is a custom data type lets you package together and name multiple related values that make up a meaningful group.
###### Defining and Instantiating Structs
```rust
struct User {
	active: bool,
	username: String,
	email: String,
}
```
![[field_init.png|400]]
*field init* shorthand, because the function parameters had same name as struct fields.

you can create a new instance of a struct that includes most of the values from another instance of the same type,but changes some.
you can do this with *struct update syntax*.
```rust
let user2 = User{
	email: String::from("reze@san.com"),
	..user1 // use rest of the values of user1
};
```

*tuple structs*
`struct Color(u8,u8,u8);`
`let Color(r,g,b) = colors;`

*unit-like structs*
`struct AlwaysEqual` 
useful when used with traits


it is also possible for structs to store references to data owned by something else, but to do so we require the use of *lifetimes*.
lifetimes ensure that the data referenced by a struct is valid for as long as the struct is.

rust tracks ownership at both struct-level and field-level.
if we borrow `x` field of a `Point` struct, then both `p` and `p.x` temporarily lose their permissions(but not `p.y`).

`:?` - `Debug` format `:#?`
`#[derive(Debug)]` - outer attribute

`println!` takes a reference 
`dbg!` takes ownership, returns after call, prints file and line no.
###### 5.3 Method Syntax
unlike functions, methods are defined within the context of a struct,enum or trait object.
their first parameter is always `self`
which represents the instance of struct the method is being called on.

`impl Rectangle { fn area(&self)->u32{} }`

methods must have a parameter named `self` of type `Self`  for their first parameter.
`self` is short for `self: Self`
`Self` is an alias for struct type

all functions defined within `impl` block are called *associated functions*. we can define associated functions as functions that don't have `self` as their first parameter.

rust only give special meaning to `self` if it is first parameter.

to call associative function we use `::`, used for namespaces also.

each struct is allowed to have multiple `impl` blocks.

when we overwrite `*self`, rust will implicitly drop the data previously in `*self`.
### Enums and Pattern Matching
enums allows you to define a type by enumerating its possible variants.
they gives us a way of saying a value is one of a possible set of values.
an enum value can only be one of its variants.
```rust
enum IpAddrKind{
	v4, // variant with no data
	v6, // unit variant
}
let ip_v4 = IpAddrKind::v4;
```

`IpAddrKind` is now a custom data type.

variants are namespaced under its identifier.

```rust
enum IpAddr {
	v4(u8,u8,u8,u8), // variants can store data
	v6(String), // we get constructor function automatically
	move { x: i32, y: i32 },
}
let home = IpAddr::v4(String::from("127.0.0.1"));
let mov = IpAddr::move { x: 5, y: 6};
```

we can also define methods on enums using `impl`

`Option<T>`- value could be something or nothing.
```rust
enum Option<T> {
	None,
	Some(T),
}
```
included in the prelude plus its variants.
```rust
let some_number = Option::Some(5);  // type : Option<i32>
let some_char = Some('a'); // type: Option<char>
let absent_number: Option<i32> = None; // need to give type for None coz compiler can't infer what would be the type of its corresponding Some value.
```

`Option<T>` and `T` are different types.

everywhere that a value has a type that isn't `Option<T>`, you can safely assume that the value isn't null.

###### 6.2 The match Control Flow Construct
```rust
    match coin { // coin can be any type
        Coin::Penny => 1, // match arm , pattern => code,
        Coin::Nickel => 5,
		Coin::Quarter(state) => {println!("{state:?}"); 25},
		_ => 0, // catch all, without value capture
    }
```

the code associated with each arm is an expression, resultant value is the value that gets returned for the entire `match` expression.

the arm's patterns must cover all possibilities.
```rust
match opt
	Some(_) => {} // doesn't moves opt
	Some(s) => {} // moves opt

match &opt
```
###### 6.3 if let and let else
match one pattern while ignoring the rest.
the code in `if let` block only runs if the value matches the pattern.
```rust
if let Some(max) = config_max {
	println!("{max}");
}
else {} // optional , same as _
```

```rust
let Some(x) = get_val() else { return None;}
```

#### Managing Growing Projecsts with Packages,Crates and Modules
a package can contain multiple binary crates and optionally one one library crate.