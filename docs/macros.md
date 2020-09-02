macro refers to a family of features in Rust:
- declarative macros with macro_rules!
- three kinds of procedural macros:
  - Custom #[derive] macros that specify code added with the derive attribute used on structs and enums
  - attribute-like macros that define custom attributes usable on any item
  - Function-like macros that look like function calls but operate on the tokens specified as their argument

Fundamentally, macros are a way of writing code that writes other code, which is known as metaprogramming. 

Metaprogramming is useful for reducing the amount of code you have to write and maintain, which is also one of the roles of functions. However, macros have some additional powers that functions don’t.
A function signature must declare the number and type of parameters the function has.
Macros, on the other hand, can take a variable number of parameters: we can call println!("hello") with one argument or println!("hello {}", name) with two arguments.
The downside to implementing a macro instead of a function is that macro definitions are more complex than function definitions because you’re writing Rust code that writes Rust code. Due to this indirection, macro definitions are generally more difficult to read, understand, and maintain than function definitions.

Declarative macros (for General Metaprogramming)
At their core, declarative macros allow you to write something similar to a Rust match expression.
Macros compare a value to patterns that are associated with particular code: in this situation, the value is the literal Rust source code passed to the macro; the patterns are compared with the structure of that source code; and the code associated with each pattern, when matched, replaces the code passed to the macro. This all happens during compilation.

Procedural macros 

The second form of macros is procedural macros, which act more like functions (and are a type of procedure). Procedural macros accept some code as an input, operate on that code, and produce some code as an output rather than matching against patterns and replacing the code with other code as declarative macros do.

When creating procedural macros, the definitions must reside in their own crate with a special crate type.

```
use proc_macro;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
}
```
The function that defines a procedural macro takes a TokenStream as an input and produces a TokenStream as an output.
The TokenStream type is defined by the proc_macro crate that is included with Rust and represents a sequence of tokens. 