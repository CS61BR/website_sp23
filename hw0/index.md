---
title: "Rust Crash Course"
has_children: false
nav_order: 1
---


This is a quick introduction to how to write code in Rust. Each example will be shown side-by-side with a rough Python equivalent. All of these examples can be copy-pasted into the [Rust Playground](https://play.rust-lang.org/).


## Hello World


<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python

print("Hello World!")

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn main() {
    println!("Hello World!");
}
```
</td>
</tr>
</table>

## Fibonacci

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
def fib(n: int) -> int:
    if n < 2:
        return n;
    return fib(n-1) + fib(n-2)




print(f"fib(12) is {fib(12)}")

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn fib(n: usize) -> usize {
    if n < 2 {
        return n;
    }
    fib(n-1) + fib(n-2)
}

fn main() {
    println!("fib(12) is {}", fib(12));
}
```
</td>
</tr>
</table>

In Rust, functions automatically return the last expression, so it's possible to just write `fib(n-1) + fib(n-2)` rather than `return fib(n-1) + fib(n-2);`. Note the lack of a semicolon: semicolons are only for statements that you don't want to return. 

Rust also has several integer types, including `usize`, `isize`, `u8`, `i8`, `u32`, `i32`, `u64`, and `i64`. In general, `usize` is the most common for integers.


## Primes

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
def is_prime(n: int) -> bool:
    for d in range(2, n - 1):
        if n % d == 0:
            return False
    return True




def next_prime(n: int) -> int:
    while not is_prime(n):
        n += 1
    return n




print(next_prime(90))

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn is_prime(n: usize) -> bool {
    for d in 2..n-1 {
        if n % d == 0 {
            return false;
        }
    }
    true
}

fn next_prime(mut n: usize) -> usize {
    while !is_prime(n) {
        n += 1
    }
    n
}

fn main() {
    println!("{}", next_prime(90));
}
```
</td>
</tr>
</table>

Just your regular ol' `for` and `while` loops. Rust also has `loop {}`, which works the same `while true {}`. Note the `mut n`: the keyword `mut` means that a variable can be changed.

## What time is it?

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
import time


print(f"it is {time.time()}")

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
use std::time::Instant;

fn main() {
    println!("it is {:?}", Instant::now());
}
```
</td>
</tr>
</table>

Note that `{:?}` is the "debug" formatter, and `{}` is the "display" formatter. This means that `{}` is usually prettier, but it's not always available. 

## Mutations

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
def add_one(lst: list[int]):
    lst.append(1)



lst = [4,3,2]
add_one(lst)
print(lst)

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn add_one(lst: &mut Vec<usize>) {
    lst.push(1)
}

fn main() {
    let mut lst = vec![4, 3, 2];
    add_one(&mut lst);
    println!("{lst:?}");
}
```
</td>
</tr>
</table>

The `&mut` on the Rust side means that we're taking a mutable reference. Essentially, this means that `add_one` is allowed to read and change the list.

You may hear the term "ownership" when talking about references. In this example, `main` "owns" the list, and `add_one` is "borrowing" it. Since `add_one` is just "borrowing" it, `add_one` has to give the list back to `main` once it finishes running (this happens automatically).


## Swap

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python







a = 4
b = 5
a, b = b, a
print(f"a = {a}, b = {b}")

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn swap(a: &mut usize, b: &mut usize) {
    let h = *a;
    *a = *b;
    *b = h;
}

fn main() {
    let mut a = 4;
    let mut b = 5;
    swap(&mut a, &mut b);
    println!("a = {a}, b = {b}");
}
```
</td>
</tr>
</table>

It's actually impossible to make a `swap` function in Python. Note that both `a` and `b` need to be mutable in order to create mutable references to them.

## Evens

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
def simple(lst: list[int]) -> list[int]:
    evens = []
    for e in lst:
        if e % 2 == 0:
            evens.append(e);
    return evens




def fancy(lst: list[int]) -> list[int]:
    return [e for e in lst if e % 2 == 0]



lst = [1,2,3,1001]
print(simple(lst))
print(fancy(lst))

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn simple(lst: &Vec<i32>) -> Vec<i32> {
    let mut evens = Vec::new();
    for &e in lst {
        if e % 2 == 0 {
            evens.push(e);
        }
    }
    evens
}

fn fancy(lst: &[i32]) -> Vec<i32> {
    lst.iter().filter(|&e| e % 2 == 0).copied().collect()
}

fn main() {
    let lst = vec![1,2,3,1001];
    println!("{:?}", simple(&lst));
    println!("{:?}", fancy(&lst));
}
```
</td>
</tr>
</table>

Different ways to get the even elements of a list. I used `i32` instead of `usize` here just for fun; any integer type would work.

The `&` means immutable borrow, which means that `simple` can read the list, but not modify it.  

## Square Root

<table>
    <thead>
        <th>Python</th>
        <th>Rust</th>
    </thead>
<tr>
<td style="font-size: inherit !important" markdown="block">

```python
def square_root(n: int) -> int | None:
    for x in range(0, n + 1):
        if x * x == n:
            return x
    return None




n = 100
x = square_root(n)
if x is not None:
    print(f"The square root of {n} is {x}")
else:
    print(f"{n} has no square root")

```
</td>
<td style="font-size: inherit !important" markdown="block">

```rust
fn square_root(mut n: usize) -> Option<usize> {
    for x in 0..=n {
        if x * x == n {
            return Some(x)
        }
    }
    None
}

fn main() {
    let n = 100;
    match square_root(n) {
        Some(x) => println!("The square root of {n} is {x}"),
        None => println!("{n} has no square root")
    }
}
```
</td>
</tr>
</table>

Instead of `null`, Rust has the `Option<T>` type. An `Option<usize>` is either a `Some(usize)` or a `None`. If you are handed an option, you can figure out the variant with a `match` or `if let` statement. 