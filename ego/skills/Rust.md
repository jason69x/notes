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

*crate* - collection of rust source code files

- binary crate
- library crate


#### variables and mutability

by default , variables are immutable.
`println!("{x}")` - format argument must be a string literal
you can make them mutable by adding `mut` in front of variable name

*constants* , 
are values that are bound to a name . always immutable
`const X_FIRST: u32 = 20;`
the type of the value *must* be annotated.

constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

constants are valid for the entire time a program runs, within the scope in which they were declared.

`const` can be used in global scope , `let` can only be used in a function

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
`let tup : (4,3,2);`
`let (x,y,z) = tup;`  - pattern matching to destructure a tuple
`tup.0`

`()`  - unit/empty tuple


*arrays* 
have fixed length, same data type
`let a = [1,2,3];`
`let a: [i32;5] = [1,2,3,4,5];`
`let a = [3;5];`  - 5 element each 3

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

```rust
let result = loop {
	counter +=1;
	if counter == 10 {
		break counter*2; // return this value from loop after break 
	}
};
```

loop labels

```
'counting_up: loop {
		break `counting_up;
}
```

`while cond {}`
`for ele in arr {}`
`for idx in (1..4).rev() {}`
`(1..=4)`


#### Ownership

each time an interpreted program reads a variable, then the interpreter must check whether that variable in defined.
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

here, int the function `add_suffix` `name` is taking the ownership of the box pointed by first.

1. At L1, the string “Ferris” has been allocated on the heap. It is owned by `first`.
2. At L2, the function `add_suffix(first)` has been called. This moves ownership of the string from `first` to `name`. The string data is not copied, but the pointer to the data is copied.
3. At L3, the function `name.push_str(" Jr.")` resizes the string’s heap allocation. This does three things. First, it creates a new larger allocation. Second, it writes “Ferris Jr.” into the new allocation. Third, it frees the original heap memory. `first` now points to deallocated memory.
4. At L4, the frame for `add_suffix` is gone. This function returned `name`, transferring ownership of the string to `full`.


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

