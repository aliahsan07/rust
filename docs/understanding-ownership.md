Ownership is Rust's most unique feature and it enables Rust to make memory safety guarantees without needing a garbage collector. 

All programs have to manage the way the use a computer's memory while running. Some languages have garbage collectors, in others the programmer must allocate and free memory. 
Rust uses a third mechanism. Memory is managed through a system of ownership with a set or rules that the compiler checks at compile time. It doesn't slow your program down. 

Ownership rules:
1. Each value in Rust has a variable that's called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope the value is dropped. 

The memory is automatically returned once the variable that owns it goes out of scope. 

```
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
```

When a variable goes out of scope, Rust calls a special function `drop`. 

Issue of double freeing:
```
    let s1 = String::from("hello");
    let s2 = s1;
```

To ensure memory safety, there’s one more detail to what happens in this situation in Rust. Instead of trying to copy the allocated memory, Rust considers s1 to no longer be valid and, therefore, Rust doesn’t need to free anything when s1 goes out of scope. 


If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called clone
```
    let s1 = String::from("hello");
    let s2 = s1.clone();
```

Stack-Only Data: Copy
types such as integers that have a known size at compile time are stored entirely on the stack, so copies of the actual values are quick to make.


<strong>References and Borrowing</strong>
Issue with returning a value from a function to be used after transfer of ownership is annoying. The better way is references. 
```
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

These ampersands are references, and they allow you to refer to some value without taking ownership of it.
When functions have references as parameters instead of the actual values, we won’t need to return the values in order to give back ownership, because we never had ownership.

We call having references as function parameters borrowing. 

Just as variables are immutable by default, so are references. We’re not allowed to modify something we have a reference to.


Mutable referernces:
```
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

But mutable references have one big restriction: you can have only one mutable reference to a particular piece of data in a particular scope. This code will fail:
```
fn main() {
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);
}
```
The benefit of having this restriction is that Rust can prevent data races at compile time.

use curly brackets to create a new scope, allowing for multiple mutable references, just not simultaneous ones:
```
fn main() {
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;
}
```

Dangling Pointers
The Rust compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.


<strong>The Slice Type</strong>
Another data type that does not have ownership is the slice. Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.


String Slices
```
fn main() {
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
}
```
if we have an immutable reference to something, we cannot also take a mutable reference. Because clear needs to truncate the String, it needs to get a mutable reference. 

String literals are slices

They're handy to pass to functions.
```
fn first_word(s: &String) -> &str {

fn first_word(s: &str) -> &str {    // better since its using slice instead of reference to String

```
If we have a string slice, we can pass that directly. If we have a String, we can pass a slice of the entire String. Defining a function to take a string slice instead of a reference to a String makes our API more general and useful without losing any functionalit