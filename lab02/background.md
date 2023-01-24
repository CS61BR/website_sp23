---
parent: "Lab 02: Debugging"
title: Background
nav_order: 0
---



## Linked Lists

For various technical reasons, linked lists are more challenging to implement in Rust than in most other languages. Before diving into the `IntList` implementation in this lab, you might want to review [enums](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html) and [boxing](https://doc.rust-lang.org/book/ch15-01-box.html).

The `IntList` implementation in this lab closely resembles "A Bad Singly-Linked Stack" from [Learning Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/). I _highly_ suggest reading through that chapter; it is a great introduction to several Rust concepts.


## IO functions

Several methods in the `adventure` portion of this lab use a signature that looks roughly like this:

```rust
pub fn do_thing<R, W>(input: &mut R, output: &mut W) -> Result<(), Error>
where
    R: BufRead,
    W: Write,
{
    writeln!(output, "I like cheese")?; // using the output
    let buf = String::new();
    input.read_line(&mut buf)?;
}
```
When the game is running, `input` and `output` are just stdin (input from keyboard) and stdout (printing the the console) respectively. You might think that the above code is significantly overcomplicated; after all, why not just do this?
```rust
pub fn do_thing_better() {
    println!("I like cheese");
    let buf = String::new();
    BufReader::new(stdin()).read_line(&mut buf);
}
```
For the most part, `do_thing_better` is a much easier way to accomplish the same thing as `do_thing`. However, `do_thing_better` has one major downside: testing. `do_thing_better` can only take input from stdin and push output to stdout, which means that if we wanted to test it, we would have to run the entire program as a subprocess and capture the output. In contrast, `do_thing` can be tested with "mock" input and output, which is much easier and faster. 


## Looping with `while let`

One unusual structure you will see in this lab is the `while let` loop. For example, here's the `size_iterative` implementation from `int_list.rs`:

```rust
pub fn size_iterative(mut s: &Self) -> usize {
    let mut total = 0;
    while let IntList::More(_, next) = s {
        total += 1;
        s = next;
    }
    total
}
```
The `while let` is shorthand for a `loop` with a `match` statement. Here's an equivalent implementation of the above code:
```rust
pub fn size_iterative(mut s: &Self) -> usize {
    let mut total = 0;
    loop { // like "while true" in other languages
        match s {
            IntList::More(_, next) => {
                total += 1;
                s = next;
            },
            _ => break
        }
    }
    total // implicit return
}
```
