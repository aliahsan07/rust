Cargo is Rust's build system and package manager
It handles building your code, downloading the libraries your code depends on
and building those libraries. 
Cargo comes installed with Rust when using official installers.

building with cargo build creates target directory with debug directory inside it.
inside debug is the executable than you can run.
building cargo build for the first time also generates a new file Cargo.lock
this file keeps track of the exact versions of dependencies in your project.

cargo check
this command quickly checks your code to make sure it compiles but doesn't produce an executable.
Why would you not want an exe?
Often, cargo check is much faster than build, becuase it skips the step of proudcing an exe.

use `cargo build --release` when your project is finally ready for release. It compiles with optimizations.
this creates exe in target/release instead of target/debug.

