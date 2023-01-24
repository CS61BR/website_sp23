---
parent: "Lab 02: Debugging"
title: Summary and Deliverables
nav_order: 3
---

## Assignment Overview

In the "bomb" portion of the lab, you will be figuring out the passwords in `bomb_answers.rs`. In the "adventure" portion of the lab, you will be debugging each of the stages of the game (`bee_counting_stage.rs`, `species_list_stage.rs`, `palindrome_stage.rs`, `machine_stage.rs`).

## Running the Programs

To run the "bomb" program, run `cargo run --bin bomb`. To run the "adventure" program, run `cargo run --bin adventure_game`. To run all the tests, run `cargo test --no-fail-fast`.

## Submitting Your Code

Before submitting your code, make sure to:
 - Run `cargo clippy` and fix any warnings that appear.
 - Run `cargo fmt` to format your code according to the Rust style guidelines.
 - Commit and push your code to your repository.

Then, submit your code to the [Gradescope assignment]({{ site.data.links.gradescope }}).
