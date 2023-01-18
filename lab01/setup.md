---
parent: "Lab 01: Setup"
title: Setup
nav_order: 1
---

## Install Git

Git is a version control system that allows you to track changes to your code and collaborate with others. It is necessary to install Git in order to clone your repository and pull code from the skeleton repository.

This step will depend on your operating system:
 - Windows: 
     - Go the the [Git for Windows download page](https://git-scm.com/download/) and download the installer.
     - Run the installer, using all the recommended/default options
 - MacOS: 
     - Install Homebrew, a package manager for MacOS, by following the instructions at [https://brew.sh/](https://brew.sh/)
     - In the terminal, install Git by running `brew install git`
 - Linux/Other:
     - Use your system's package manager (apt, dnf, pacman, etc) to install `git`. For example, Ubuntu users should run `sudo apt install git`.

To verify that Git is installed, open up your terminal (Git Bash on Windows), and run `git --version`. You should see something like `git version [numbers]`.


## Install a Text Editor

You will need a text editor to write and edit your code. The instructions in this course assume that you are using Visual Studio Code (VSCode), but you can use a different text editor if you prefer (such as Atom, Sublime, or Neovim).

To install VSCode, go to the [VSCode download page](https://code.visualstudio.com/download) and follow the instructions.


## Install Python

Python is used for running occasional scripts in this course. To install Python, go to the [Python download page](https://www.python.org/downloads/) and follow the instructions. Make sure to install Python 3, not Python 2.


## Install Rust

Rust is a programming language designed for performance, safety, and concurrency. It is the language that you will be using in this course.

To install Rust, follow the instructions at [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install). It's suggested that you use `rustup`, which will automatically install `cargo` and `rustc` for you.

To verify that Rust is installed, run `rustup --version`. You should see something like
```
rustup 1.25.1 (bb60b1e89 2022-07-12)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.65.0 (897e37553 2022-11-02)`
```
but with different numbers.

## Install wasm-pack

Rust can compile code to WebAssembly, which allows us to build web pages with Rust. The wasm-pack tool helps us generate Javascript bindings for those web pages.

To install wasm-pack, follow the instructions at [https://rustwasm.github.io/wasm-pack/](https://rustwasm.github.io/wasm-pack/).

To verify that wasm-pack is installed, run `wasm-pack --version`. You should see something like `wasm-pack [numbers]`.


## Install the rust-analyzer extension

The rust-analyzer extension provides intelligent code completion and other helpful features for working with Rust in your text editor.

If you are using VSCode, open the "Extensions" panel in the left sidebar, and search for the "rust-analyzer" extension. You can also find the extension here: [https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

If you are using a different text editor, you can find installation instructions here: [https://rust-analyzer.github.io/](https://rust-analyzer.github.io/). Any text editor that supports the Language Server Protocol will support rust-analyzer.


## Set up a local repository

A repository is a directory that contains your project files and stores them in a version control system (in this case, Git). You will need to set up a repository in order to clone the skeleton code and push your own code to Github.

 - If you haven't already, make an account on [Github](https://github.com/). You'll need to create either a personal authentication token or an SSH key for your account; instructions on creating a PAT are [here](https://docs.github.com/en/enterprise-server@3.4/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
 - On github, create a new, **private** repository. You can name it anything.
 - Clone your new repository (`git clone https://github.com/yourusername/example-name.git`). Git may give you a warning ("You appear to have cloned an empty repository"), which is completely fine.
 - Your new repository will be receiving code updates from multiple remotes, so you need to tell Git to merge commits on pull. `cd` into your new repository, and run
    ```
    git config pull.rebase false
    ```
 - In your new repository, add the skeleton repository as a remote:
    ```
    git remote add skeleton {{site.data.links.skeleton_repo}}.git
    ```
 - Pull code from the skeleton, and push it to your private repo:
    ```
    git pull skeleton main
    git push
    ``` 
 - Open your new repository on Github. If you see the skeleton code, congratulations! You have successfully set up your repository.

## (Optional) Enroll in the Gradescope course

If you want your assignments to be graded, you can enroll in the [gradescope course]({{site.data.links.gradescope}}) using code {{site.data.links.signup_code}}. Each assignment has an autograder, which you can submit to unlimited times.
