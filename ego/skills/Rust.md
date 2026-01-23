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

constants are valid for the entire time a program runs, within the scope in which they were declared. can be declared in global or function scope.

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

