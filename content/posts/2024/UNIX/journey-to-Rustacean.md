---
title: Journey to Rustacean
date: 2024-05-20T08:49:23+05:30
url: /journey-to-Rustacean/
categories:
   - Linux
   - Notes
tags:
    - rust
    - linux
    - terminal
    - programming
draft: false
author: aayan
showauthor: false
showDateOnlyInArticle: false
showDateUpdated: true
showHeadingAnchors: false
showPagination: false
showReadingTime: true
showTableOfContents: true
showTaxonomies: true
showWordCount: true
sharingLinks: false
showEdit: false
showViews: false
showLikes: false
showSummary: true
summary: Pursuit of Rustacean enlightenment
---
<!--more-->

## Rust



## Rust Notes

### Understanding Main function and Macros of Rust using Hello World.

```rs
// This is a comment
fn main() {
    println!("Hello, world!");
    // This prints hello, world with a \n by default
}
```

- **`fn`**: funcation (like mini program)
- **`main`**: the main functon, where the program begins
- <u>macro</u>: (like built in function). How is it a macro? -> **`!`**
    - **`print!`**: just a normal print. all output of multiple `print!` will be in one line without spaces
    - **`println!`**: Prints each `println!` strings in different lines
- <u>Comments</u>: `//`

### Comments

```rs
// This is a comment
fn main() {
    println!("Hello, world!");
    /* 
    This is a 
    multi - line 
    comment
    block. (/* xyz */)
    */
}
```

### Errors

```rs
fn main() {
    println!('Hello, world!');
}
```

- In rust, when we want to declare a string, it needs to be in single quatation `"xyz"`, not in single quatation.

### Premitives: Integer

- Scalar types: int, char, foat, boolean 
- unsigned: never -ve (u8, u16, u32, u64, u128, usize)
- signed: can be -ve or +ve (i8, i16, i32, i64, i128, isize)
- **Default is i32 (best practice)**

```rs
// {} -> Place holder
// MAX -> Max value that can be represented by this integer type
fn main() {
    println!("Max size of a u32: {}", u32::MAX);
}
```
Output: Max size of a u32: 4294967295

### Premitives: Float

- Floats: f32, f64
- pi = 3.14

```rs
fn main() {
    println!("Max size of a f32: {}", f32::MAX);
    println!("Max size of a f64: {}", f64::MAX);
}
// Max size of a f32: 340282350000000000000000000000000000000
// Max size of a f64: 179769313486231570000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

### Premitives: boolean
- True or False
- representation: bool



### Premitives: character
- char - 4 bytes


### Variables
- let
- variables in rust are immutable

```rs
fn main() {
    let hello = "Hello, world!";
    // let hello: &str = "Hello, world!";
    // let _hello = "Hello, world!";
    //      unused variable: `hello`, mute output
    println!("{}", hello);
}
```

- Mutable variable

```rs
fn main() {
    let hello = "Hello, world!";
    println!("{}", hello);
    
}
```

---

**More content soon!**

---

