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