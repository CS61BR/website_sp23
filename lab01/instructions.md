---
parent: "Lab 01: Setup"
title: Instructions
nav_order: 2
---

## Starting the Lab

 - Do the [setup](setup.md).
 - Open the `lab01/` folder in your preferred text editor.
 - Open the `lab01/` folder in your preferred terminal.

## Debugging Exercise

In the lab folder, open up `src/main.rs`. This is a small program that tells you the sum and product of two numbers - at least, that's what it's supposed to do. 

Run the command `cargo run` in the `lab01/` folder. This will run the program, prompting you to enter a number. If you follow the prompts, you'll probably notice that the results might not be correct. You can confirm this by running the command `cargo test`. This will run the included tests, one of which should fail.

Fix the bug in `main.rs`, then re-run `cargo run` and `cargo test`. This time, the program should yield correct results, and the tests should both pass. Congratulations, you've just completed your first assignment!

When you're ready to submit your code to Gradescope, follow the instructions in the [summary](summary.md).
