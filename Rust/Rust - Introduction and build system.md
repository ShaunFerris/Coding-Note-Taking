#rust 

Rust is a language that tries it's best to marry high-level developer ergonomics with the low level control of systems languages like C. 

## Installation
To install Rust on a Unix machine, you can run this curl command:
```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```
This will also install cargo, the rust build system.

## Hello world
Rust is a compiled language, so to make a simple hello world script, create a new file in a projects hello-world directory and name it "main.rs". In that file we can execute the hello world print with the following:
```rust
fn main() {
	println!("Hello World");
}
```
Now in a terminal window you can compile the script and then run the output by running rustc, the rust compiler. Note that this is different to running the build system, cargo, which will be covered next:
```bash
rustc main.rs
./main
```
Note in the above code the basics of Rust syntax. fn defines the function, and the hello world string is printed using a macro, denoted by the `!`. Ommitting the exclamation would call the `println` function instead of the macro. The differences between rust functions and macros will be covered in future. Finally note the semi-colon, in Rust this denotes the end of an expression.

## Cargo
Cargo is the Rust build system, which manages dependancies project wide settings and the compiling of builds and outputs.

Cargo comes with  Rust when installed as explained above. You can check your version of cargo with:
```bash
cargo --version
```

### Scaffolding a project with cargo
