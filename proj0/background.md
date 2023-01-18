---
parent: "Project 0: AoA"
title: Background
nav_order: 0
---

## Gameplay

From the CS61B version of this project:

> In the game “Awakening of Azathoth”, a player has a limited number of tries to reveal a word by guessing letters, one at a time. Each time a player guesses a correct letter, it is revealed. But when a player guesses an incorrect letter, they lose one of their guesses and learn nothing. If the player reveals the word before they run of guesses, Azathoth continues his slumber, but if they run out of guesses, Azathoth is awakened, and the game is lost.


## Types and Borrowed Types

In this project, you'll be working with the types `String` and `Vec<T>`. If you want to allow a function to borrow those types, you might write:

```rust
fn print_stuff(s: &String, v: &Vec<usize>) {
    println!("I have a String: {s}");
    println!("I have a Vec: {v:?}");
}
```
which is a perfectly fine way of doing things. However, you'll often see those types written as:
```rust
fn better_print_stuff(s: &str, v: &[usize]) {
    println!("I have a str: {s}");
    println!("I have a slice: {v:?}");
}
```
which is almost the same thing. `&str` is like a superset of `&String`: it can be a String, but it could also be raw string data. Similarly, `&[usize]` is like a superset of `&Vec<usize>`.

So we can do this:
```rust
let s: String = String::from("cheese");
let v: Vec<usize> = vec![1, 2, 3];
better_print_stuff(&s, &v);
```
but we can also do this:
```rust
let s: &str = "cheese"; // raw string data
let ar: [usize; 3] = [1,2,3];
better_print_stuff(s, &ar);
```

Under the hood, `String` implements `AsRef<str>`, so `&String` can be automatically converted to `&str`. Similarly, `Vec<T>` implements `AsRef<[T]>`, so `&Vec<T>` can be automatically converted to `&[T]`.
