---
parent: "Lab 02: Debugging"
title: Instructions
nav_order: 1
---

## Starting the Lab

 - Open the `lab02/` folder in your preferred text editor.
 - Open the `lab02/` folder in your preferred terminal.
 - Run `git pull skeleton main` to get the latest version of the skeleton code for this lab.

## Choosing a debugger

Since this lab is about debugging, it will probably help significantly for you to have a debugger. You have several options, including but not limited to:
 - My recommendation is the [CodeLLDB extension for VSCode](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb). It has a nice GUI for setting breakpoints, stepping over/into functons, printing variables, etc.
 - If you're too cool for a GUI, you can use `rust-gdb`, which is a thin wrapper around `gdb`. It comes pre-installed with most Rust installations, so you can get up and running immediately. The biggest downside is that `gdb` has a steep learning curve, especially if you want to use it with Rust.
 - You can just stare at the code and think very hard until you find the bugs.


## High Explosives: Phase 0

Some chemistry majors were playing with dihydrogen monoxide, and they accidentally created a bomb! For some inexplicable reason, the only way to defuse it is to enter three secret passwords, each of which are hidden deep inside the bomb. However, you need to be careful: if you enter the wrong password, the bomb will explode, wiping out the entire chemistry department's supply of dihydrogen monoxide.

If you look at `bomb.rs`, you'll notice that the first password is stored in the variable `correct_password`. If only there were a way to see the contents of that variable...
 - In your chosen debugger, set a breakpoint on the line after `correct_password` is set.
 - Run the program in debug mode (if you're using CodeLLDB, a little "debug" button should appear above the main method)
 - When the program stops at the breakpoint, write down the value of `correct_password`.

Once you've retrieved the password, fill in the `password0` function in `bomb_answers.rs`. You can (safely) check your answer by running `cargo test bomb`.

## High Explosives: Phase 1

The second password is buried in a tricky-to-reach spot, so you'll need to do a bit more work:
 - First, fill in `password1` with `IntList::Empty` so that the program doesn't panic at the beginning of phase 1.
 - Set a breakpoint on the line `if attempt.to_string() != correct_password.to_string()`.
 - Run the program in debug mode.
 - When the program stops at the breakpoint, step into both of the `to_string()` functions. They might be assembly - this is perfectly fine! Even in the assembly code, all we're interested in is the variable `buf`, which should appear after `IntList::fmt` is called. 
 - write down the `buf` value.

Once you've retrieved the password, fill in the `password1` function in `bomb_answers.rs`. You can (safely) check your answer with `cargo test bomb`. 


## High Explosives: Phase 2

The third and final password is so long, it takes a loop to check! Luckily, only one part of it actually matters, so you can retrieve the 1337th part of the password using your debugger:
 - First, fill in `password2` with `String::new()` so that the program doesn't panic at the beginning of phase 2.
 - Set a _conditional breakpoint_ on the line that starts with `if i == 1337`. Specifically, you only want the breakpoint to trigger when `i == 1337`.
 - Debug the program and write down the correct value of `peices[1337]`.

Once you've retrieved the password, fill in the `password2` function in `bomb_answers.rs`. You can (safely) check your answer with `cargo test bomb`. Once all the tests pass, run `cargo test --bin bomb`. Congratulations - you saved the chemistry department!

## Choose Your Own Adventure: Bees

I know you've been hard at work debugging, so I wanted to let you relax! I've ~~stolen~~ adapted a "choose your own adventure game", which you can play by running `cargo run --bin adventure_game`.

Enjoy playing the game!

...there may or may not be a few bugs in the bee stage. In the extremely unlikely circumstance that there are some bugs, run `cargo test bee` to check your work after you fix them.

## Choose Your Own Adventure: Species

Once you pass the bee stage, there's a trivia mini-game in the second stage! You'll have a ton of fun playing this, I promise.

...there may or may not be a few bugs with this one, too. Run `cargo test species` when you finish fixing them.

## Choose Your Own Adventure: Palindromes

I admit, the previous few stages may have not been well-tested. But the palindrome stage should make up for it - I put special care into the palindrome testing algorithm. 

...there are no bugs in this level, but hypothetically, if you did fix some bugs here, you could check your work with `cargo test pal`.

## Choose Your Own Adventure: Machine

This stage is actually based on a true story: back in 1635, a student stumbled across a mysterious machine on the 0th floor of Soda Hall. After some experimenting, they figured out that the machine calculated the "sum of elementwise maxes". For example, if given the lists `[1, 2, 3, 4]` and `[6, 1, 7, 2]`, the maxes would be `[6 2 7 4]`, so the machine would output 19.

Unfortunately, the machine mysteriously vanished in 2012, so I had to try to recreate the machine's algorithm from scratch. It might not be a perfect recreation, so if you spot any discrepancies, please fix them. You can check your work with `cargo test machine`.
