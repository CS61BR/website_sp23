---
parent: "Project 0: AoA"
title: Instructions
nav_order: 1
---

## Starting the Project

 - Run `git pull skeleton main` to get the latest version of the skeleton code for this lab.
 - Open the `proj0/` folder in your preferred text editor.
 - Open the `proj0/` folder in your preferred terminal.

## Running the Starter Code

In this project, the game logic has already been implemented: you can view it in `src/main.rs`. The game is played between two "players": a guesser and a chooser. In the following portions of this project, you'll be implementing several AI choosers and guessers.

If you run `cargo run`, you can play the game. Currently, the game uses `ConsoleGuesser`, which uses input from the keyboard as the guess, and `CheeseChooser`, which always chooses the word "cheese". Neither of these two are very good at the game, so we need to implement better players.

## Guesser: LetterFrequencyGuesser

`LetterFrequencyGuesser` is an AI that will play the "guesser" role. Its strategy is very simple: it will guess the most common letter that hasn't been guessed yet.

 - First, fill in the `freq_map` function in `src/guessers/mod.rs`. This function should take in a list of words and return the counts for each letter. For example, if the input is `["i", "like", "cheese"]`, it should return
`{c:1, e:4, h:1, i:2, k:1, l:1, s:1}`. 
 - Once you have filled in the `freq_map` function, use it to implement `LetterFreqGuesser` in `src/guessers/letter_freq_guesser.rs`. The `guess` function should return the most common letter that doesn't appear in `guesses`. If there are ties, return the letter that comes first alphabetically. For example, if the frequency map is `{a:2, b:3, d:2, e:7, f:3}` and `guesses` is `['d', 'e']`, it should return `'b'`.
 - Run `cargo test letter` to test your code.
 - In the `main` function in `src/main.rs`, replace `ConsoleGuesser` with `LetterFrequencyGuesser`. Run the game with `cargo run`. How well does `LetterFrequencyGuesser` do against the `CheeseChooser`?


## Guesser: PatternAwareLFG

The `LetterFrequencyGuesser` disregards a lot of important information: specifically, it doesn't make use of `pattern`, which tells the guesser which letters match the word. The `PatternAwareLFG` is an AI that uses `pattern` to narrow down the list of words.

For example, if the possible words are `["broth", "curry", "dough", "cones", "cakes", "pesto"]`, and the pattern is `"-o---"`, the only words that match that pattern are `["dough", "cones"]`. The frequency map should now be `{c:1, d:1, e:1, g:1, h:1, n:1, o:2, s:1, u:1}`, since we only need to consider those two words.

Don't worry about "extra" letters: for example, `"cheese"` should match the pattern `"----e"`, even though `"cheese"` has extra `'e'`s. The "extra" letters will be handled by the `PatternAndGuessAwareLFG`.

 - Implement `PatternAwareLFG` in `src/guessers/pattern_aware_lfg.rs`. It should operate very similarly to `LetterFreqGuesser`, except it should only consider words that match the given pattern. 
 - Run `cargo test pattern` to test your code.
 - In the `main` function in `src/main.rs`, replace `LetterFrequencyGuesser` with `PatternAwareLFG`. Run the game with `cargo run`. How well does `PatternAwareLFG` do against the `CheeseChooser`?

## Guesser: PatternAndGuessAwareLFG

The `PatternAwareLFG` is fairly good at the game, but it can still be improved. For example, suppose the first guess was `'e'`, and the word had no `'e'` in it. The pattern would be `-----`, which seems to provide no useful information. However, this tells us that the word has no `'e'`s, which can actually be used to narrow down the list significantly: from `["broth", "curry", "dough", "cones", "cakes", "pesto"]` to just `["broth", "curry", "dough"]`.

 - Implement `PagaLFG` (which stands for "Pattern and Guess Aware Letter Frequency Guesser") in `src/guessers/paga_lfg.rs`. It should filter down the list of words using the `pattern` and `guesses`, and then return the most common letter in the remaining words.
 - Run `cargo test paga` to test your code.
 - In the `main` function in `src/main.rs`, replace `PatternAwareLFG` with `PagaLFG`. Run the game with `cargo run`. How well does `PagaLFG` do against the `CheeseChooser`?

## Chooser: RandomChooser

The main flaw of `CheeseChooser` is that it is very, very predictable. This isn't very fun to play against, so let's make a better chooser.

`RandomChooser` should randomly select a word from the given list to be the secret word, and it should store the currently revealed pattern. The `make_guess` function should reveal all letters matching the input `guess`, returning how many letters were revealed. For example, if the secret word is `"curry"`, the current pattern is `"c----"`, and the guess is `'r'`, the pattern should be updated to `"c-rr-"`, and it should return 2.

Implement `RandomChooser` in `src/choosers/random_chooser.rs`, then run the tests with `cargo test random`. Once it works, replace `CheeseChooser` in `main` with `RandomChooser`. How well does it do against the AI guessers?

## Chooser: EvilChooser

`RandomChooser` is lawful neutral: it picks a word, then honestly represents it while the guesser tries to guess all the letters in the word. In contrast, `EvilChooser` will be chaotic evil: rather than picking a word, it will _pretend_ to pick a word, then try to dodge the guesser's guesses as much as possible.

For example, if the possible words are `["broth", "curry", "dough", "cones", "pesto"]`, and the first guess is "o", the chooser has 4 options:
 - it can reveal the pattern `"-----"`, which matches the word `"curry"`.
 - it can reveal the pattern `"-o---"`, which matches the words `"dough"` and `"cones"`.
 - it can reveal the pattern `"--o--"`, which matches the word `"broth"`.
 - it can reveal the pattern `"----o"`, which matches the word `"pesto"`.

In order to keep its options open, the `EvilChooser` should reveal the pattern `"-o---"`, which narrows the word list down to `["dough", "cones"]`. This allow the `EvilChooser` to delay picking a word for another turn, forcing the guesser to spend another guess to figure out what the word is. If `EvilChooser` chose any of the other options, a good guesser like `PatternAndGuessAwareLFG` would be able to tell exactly what the word is.

In general, the `EvilChooser` should always reveal the pattern with the highest number of matching words, then pare the word list down to only the words that match the pattern. 

Some miscellaneous details:
 - in the `new` method, you can assume that `possible_words` is non-empty, and that all the words have the same length.
 - If there's a tie between two patterns, the `EvilChooser` should choose the pattern that comes first alphabetically.
 - Since the `EvilChooser` always has a list of possible words, `get_word` should just return the first word in the list.

Implement `EvilChooser` in `src/choosers/evil_chooser.rs`, then run the tests with `cargo test evil`. Once it works, replace `RandomChooser` in `main` with `EvilChooser`. How well does it do against the AI guessers?
