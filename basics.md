# Getting started with aiken fundamentals
What is aiken to begin with? 
Aiken is a strongly-typed, functional programming language designed for writing secure and composable smart contracts on Cardano. It is inspired by ML-family languages (like OCaml and Haskell) but with a simpler, more ergonomic syntax tailored to Plutus smart contract development. This is big English for Aiken is a new language that makes writing smart contract on Cardano easier.  Aiken doesn't run on its own, but rather compiles to plutus core. This means simple operations such as printing a variable are naturally not allowed to work on aiken. Since aiken works more like a transpiler than an actual programming language.

### Modules
Each aiken file is considered a module and ends with the extension `.ak`.
To follow along and properly get started with aiken, (assuming you've installed aiken properly from the previous tutorial) you'd have to initialize a new aiken project.

### Creating a new project on Aiken
To create a new project on aiken we use `aiken new owner/project_name` as the syntax. 
```markdown
aiken new {owner}/projectname
```
Example:
```bash
aiken new dox/aikenapp
```
Then cd into aikenapp
```
cd aikenapp
```
You should have a directory that looks like this. 
 - `├──` aikenapp
    - `└──` .github
    - `└──`README.md
    - `└──` env
    - `└──`lib
    - `└──`aiken.toml
    - `└──`validators
	    - `└──`placeholder.ak
	
### How Aiken Projects are Organized

### Folder Organization

Aiken organizes source code into two main categories (based on the main documentation):

-   **Library Code**: Stored in the lib folder. This includes reusable modules and functions for your project.
-   **Application Code**: Stored in the validators folder. This contains on-chain validators (smart contract logic) for Cardano.

After compiling your project, Aiken generates a plutus.json file, known as a Plutus blueprint. This interoperable document summarizes your project and includes:

-   Compiled code for each validator (more on this later).
-   Hash digests for use in Cardano addresses.

### Configuration

Every Aiken project includes an aiken.toml file at its root. This file holds project metadata and lists dependencies. Below is an example with explanations:

**aiken.toml**

```toml
# [Optional - required if 'members' is present] 
# Project name in the format {organisation}/{repository}  
name  =  "foo/bar"  

# [Optional - required if 'members' is present]  
# Project version, ideally following semantic versioning (e.g., 1.0.0)  
version  =  "1.0.0"  

# [Optional]  
# License name, preferably Apache-2.0 or MPL-2.0 for open-source projects  
licence  =  "Apache-2.0"  

# [Optional]  
# A brief description of your project  
description  =  "A next-level DeFi platform"  

# [Optional]  
# List of folders containing Aiken projects (used for workspaces)
members  =  ["."]  

# [Optional]  
# Repository details for documentation  [repository]  
platform  =  "github"  

# Currently supports 'github' only  
user  =  "aiken-lang"  

# Username or organization  
project  =  "stdlib"  

# Repository name  
# [Optional]  
# Dependencies, managed via `aiken packages` (avoid manual edits)
[[dependencies]]  
source  =  "github"  

# Currently supports 'github' only  
name  =  "aiken-lang/stdlib"  

# Format: {organisation}/{repository}  
version  =  "main"  

# Branch, tag, or commit``

```

Now we have this aside, let's get back to modules.
Modules are declared with the use keyword and follows a simple directory-based hierarchy.
```rust
use my_project/somemodule
```
This allows us to import and work with something called the aiken standard library (more on this later). Standard library modules can be found in the [stdlib](https://aiken-lang.github.io/stdlib/) and allow us to use predefined modules. They follow the same format
```rust
use std/module
```
If you check in the placeholder folder of the generated aikenapp, you'd see a module called `placeholder.ak`. Inside the module you'd find a sample placeholder smart contract. At the top of the file you'd see various different stdlib modules that are available for use.

### Types
Just like any other language, aiken supports various data types. The following are a list of data types supported by aiken.

-**Primitive Types** :  Int, Bool, ByteArray(String), Uint
-**Tuples** : (Int, Bool)
-**Custom Types** : (aka Algebraic Data Types (ADT))

To assign/bind  a data type in aiken, we use the `let` keyword. 
Example:
```
let someNumber = 10
```
We can as well define our own custom types this way.
```rust
type Result {
  Ok(Int)
  Err(ByteArray)
}
```
The above code is a  **custom data type** named Result using a **variant type** (also known as an algebraic data type or tagged union).

### Code Explanation

`type Result { Ok(Int) Err(ByteArray) }`

The code defines a **custom type** named Result with two **constructors**: Ok and Err. Here's what each part means:

-   **type Result**: This declares a new custom type called Result. It’s a user-defined type that can represent a value in one of two forms, defined by its constructors.
-   **Ok(Int)**: This is a constructor named Ok that takes a single argument of type Int. A value of Result can be constructed as Ok(n) where n is an integer, representing a successful outcome with an associated integer value.
-   **Err(ByteArray)**: This is a constructor named Err that takes a single argument of type String(ByteArray). A value of Result can be constructed as Err(msg) where msg is a string, representing a failure with an associated error message. Note that strings are called ByteArrays in Aiken.

Custom types like Result are commonly used to model data that can take multiple forms, especially in functional programming and smart contract development. This Result type is a classic pattern for handling success and failure cases, similar to Result types in languages like Rust or Haskell.

#### Creating Values

You can create values of the Result type using its constructors. For example:

```rust
let success = Ok(42)
// Creates a Result value with the Ok variant, holding the integer 42 
```
```rust
let failure = Err("Something went wrong") // Creates a Result value with the Err variant, holding a string`
```
Both success and failure are values of type Result, but they use different constructors.
Variables can be enforced to have specific data types as well.
```rust
let x : Int = 10;
let x: ByteArray = "hello world";
let x: Bool = True;
```
You can also declare a variable that enforces a specific custom data type
```rust
let x : Result = Ok(10);
```

At this point you're probably wondering how to run these examples. The bad news is that we can't right now. At least not at this point. Recall that aiken is just like a transpiler but with so many other rules enforced to allow it compile to plutus core. Before we start running these files, let us visit one more concept.

### Functions
In Aiken, functions are a core building block for writing modular, reusable, and type-safe code, especially for Cardano smart contracts. Remember we defined aiken as functional programming language? This means that most of what you'd be writing in Aiken will be functions. 

###  Functions in Aiken

Functions in Aiken are **pure**, first-class, and expression-based, following the principles of functional programming. This means:

-   **Pure**: Functions produce the same output for the same input and have no side effects (unless explicitly using tools like trace for debugging).
-   **First-Class**: Functions can be passed as arguments, returned from other functions, or stored in variables.
-   **Expression-Based**: Functions always return a value, and their body is a single expression (which may include nested expressions like let or when).

Functions are used to define logic, manipulate data, and implement smart contract validators. Aiken’s type system ensures functions are safe and predictable, which is critical for on-chain code.

----------

### Declaring a Function

To declare a function in Aiken, you use the fn keyword, followed by the function name, parameters (if any), and the function body. The basic syntax is:

```rust
fn function_name(parameter1: Type1, parameter2: Type2) -> ReturnType {
	 expression 
 }
```

-   **fn**: Keyword to define a function.
-   **function_name**: The name of the function (lowercase by convention).
-   **parameter1: Type1, parameter2: Type2**: Parameters with their types, separated by commas, enclosed in parentheses.
-   **-> ReturnType**: The return type of the function, preceded by an arrow. This is optional if the type can be inferred.
-   **{ expression }**: The function body, which is a single expression that evaluates to the return value.

#### Example: Simple add Function


```rust 
fn add(a: Int, b: Int) -> Int { 
	a + b
 }
```

-   Name: add
-   Parameters: a (type Int), b (type Int)
-   Return Type: Int
-   Body: a + b (returns the sum of a and b)

You can call this function like:

```rust
let result = add(2, 3) // result is 5
```
### Function Parameters

Parameters define the inputs a function accepts. Here’s how they work in Aiken:

-   **Typed Parameters**: Each parameter must have an explicit type (e.g., Int, String, or a custom type). Aiken’s type system ensures parameters are used correctly.
-   **Multiple Parameters**: Parameters are listed in parentheses, separated by commas.
-   **No Parameters**: If a function takes no parameters, use empty parentheses ().

#### Example: No Parameters

```rust 
fn greet() -> String { "Hello, Aiken!" }
```

Calling it:
```rust
let message = greet() // message is "Hello, Aiken!"
```
### Return Types

-   **Explicit Return Types**: You can specify the return type after ->. This is optional if Aiken’s type inference can determine it automatically.
-   **Implicit Return**: The last expression in the function body is automatically returned. There’s no return keyword in Aiken (unlike imperative languages).
-   **Any Valid Type**: Return types can be basic types (Int, ByteArray(String), Bool), custom types, or even function types (for higher-order functions).

There's probably something I should explain here. 
In Aiken, code blocks (such as function bodies) must return an explicit result
in the form of an expression. While assignments are technically speaking expressions, they aren't allowed to be the last expression of a function because they convey a different meaning, and this could be error-prone. 

#### Example: Implicit Return

```rust
fn is_positive(n: Int) -> Bool { n > 0 }
```

-   Returns True if n is positive, False otherwise.
-   The expression n > 0 is the implicit return value.

#### Example: Omitting Return Type

If the return type is obvious, you can omit it:

```rust 
fn square(n: Int) { n * n }
```

Aiken infers the return type as Int.

----------

### Function Body

The function body is a **single expression** that evaluates to the return value. You can use:

-   **Literals**: E.g., 42, "hello", True.
-   **Operators**: E.g., +, *, &&.
-   **Let Bindings**: To define intermediate values.
-   **Pattern Matching (more on this later)**: With when to handle different cases.
-   **Function Calls**: To compose logic.

#### Example: Using Let Bindings

```rust
fn hypotenuse(a: Int, b: Int) -> Int { 
	let a_squared = a * a 
	let b_squared = b * b
	a_squared + b_squared
  }
```

-   Uses let to compute intermediate values.
-   Returns a_squared + b_squared.

At this point we are probably ready to run your first Aiken module. (Not that you weren't ready before, but we felt that taking this step by step would be helpful.)
Inside your placeholder.ak file, delete any sample file inside and add this code
```rust
//the custom type we declared
type Result {
  Ok(Int)
  Err(ByteArray)
}


fn main() {
  let x: ByteArray = "Hello" // enforcing custom data type with let-bindings
  let y: Result = Ok(10)
  let result1 = Ok(42)
  let result2 = Err("Failed")
  let result3 = add(5,6)
  trace(result2) // we will discuss trace in a short time
  trace(result1)

}

fn add(a: Int, b: Int) -> Int {
  a + b
}

fn square(n: Int) { n * n }
```

Open up your terminal (I assume you're in the aikenapp or similar where you project lives directory)
run the command
```bash
$ aiken check
```
You're likely to get this as a result
```markdown
 ⚠ I found an unused private function: square
    ╭─[./validators/placeholder.ak:25:1]
 24 │ 
 25 │ fn square(n: Int) { n * n }
    · ────────┬────────
    ·         ╰── unused (private) function
    ╰────
  help: Perhaps your forgot to make it public using the pub keyword?
        Otherwise, you might want to get rid of it altogether.

      Summary 0 errors, 5 warnings
```
Let us explain. First, did you notice how aiken gives a nice and clean compilation error? That's admirable. It makes it easy to understand where the issue is coming from so that you can effectively tackle it. The above shows that we have 0 errors with 5 warnings.
The warnings stem from the declaration of the functions. By default any function declared in aiken without the `pub` keyword defaults to a private function and aiken throws this warning. There is also another warning there but we'd get to it in a min. When we add the pub keyword to the functions see how it gives less warnings .
```rust
//the custom type we declared
type Result {
  Ok(Int)
  Err(ByteArray)
}


pub fn main() {
  let x: ByteArray = "Hello" // enforcing custom data type with let-bindings
  let y: Result = Ok(10)
  let result1 = Ok(42)
  let result2 = Err("Failed")
  let result3 = add(5,6)
  trace(result2)
  trace(result1)

}

pub fn add(a: Int, b: Int) -> Int {
  a + b
}

pub fn square(n: Int) { n * n }
```
Here are the new terminal outputs

```markdown
  ⚠ I came across an unused variable: y
    ╭─[./validators/placeholder.ak:10:7]
  9 │   let x: ByteArray = "Hello" // enforcing custom data type with let-bindings
 10 │   let y: Result = Ok(10)
    ·       ┬
    ·       ╰── unused identifier
 11 │   let result1 = Ok(42)
    ╰────
  help: No big deal, but you might want to remove it or use a discard _y to
        get rid of that warning.
        
        You should also know that, unlike in typical imperative languages,
        unused let-bindings are fully_ignored in Aiken.
        They will not produce any side-effect (such as error calls). Programs
        with or without unused variables are semantically equivalent.
        
        If you do want to enforce some side-effects, use expect with a discard
        _y instead of let.
        

      Summary 0 errors, 3 warnings
```
### Public vs. Private Functions

-   **Private by Default**: Functions are private to their module unless explicitly marked as public.
-   **Public Functions**: Add the pub keyword to make a function accessible outside the module.

#### Example

```rust
// Private function 
fn internal_helper(x: Int) -> Int { x + 1 } 

// Public function 
pub fn increment(x: Int) -> Int { internal_helper(x) }`
```
-   internal_helper is only accessible within the module.
-   increment can be imported and used by other modules.

Alright now let's head on to the other errors and closely look at what it says.
```markdown
 let y: Result = Ok(10)
    ·       ┬
    ·       ╰── unused identifier
 11 │   let result1 = Ok(42)
    ╰────
  help: No big deal, but you might want to remove it or use a discard _y to
        get rid of that warning.
        
        You should also know that, unlike in typical imperative languages,
        unused let-bindings are fully_ignored in Aiken.
        They will not produce any side-effect (such as error calls). Programs
        with or without unused variables are semantically equivalent.
```
So looking at this carefully we notice that the warnings are about some unused variables in the code. specifically `let y, let x and let result3`
The reason is because these variables are not later used anywhere in the scope. We can easily get rid of these errors by returning a trace of the variables. 

```rust
//the custom type we declared
type Result {
  Ok(Int)
  Err(ByteArray)
}


pub fn main() {
  let x: ByteArray = "Hello" // enforcing custom data type with let-bindings
  let y: Result = Ok(10)
  let result1 = Ok(42)
  let result2 = Err("Failed")
  let result3 = add(5,6)
  trace(result2)
  trace(result1)
  trace(x)
  trace(y)
  trace(result3)

}

pub fn add(a: Int, b: Int) -> Int {
  a + b
}

pub fn square(n: Int) { n * n }
```

Now we see we do not have any errors left.
```markdown
Compiling ace/aikenapp 0.0.0 (.)
    Compiling aiken-lang/stdlib v2.2.0 (./build/packages/aiken-lang-stdlib)
   Collecting all tests scenarios across all modules
```
 That's good. You've probably gotten a hang of a few things by now, if not take time to digest all of these. Let us proceed.

### Higher Order Functions
Aiken supports higher-order functions, meaning functions can take other functions as parameters or return functions. 
```rust
fn apply_twice(f: fn(Int) -> Int, x: Int) -> Int {
  f(f(x))
}
fn double(x: Int) -> Int {
  x * 2
}

fn return_result() {
  let result = apply_twice(double, 5)
  trace(result)
}
 // result is 20 (double(double(5)) = double(10) = 20)
```
-   apply_twice takes a function f (which maps Int to Int) and applies it twice to its inputs. 
- Note that the expressions inside the function bodies don't end with a semi-colon. This is something to note as it will give an error if included.

```markdown
Compiling ace/aikenapp 0.0.0 (.)
        Error aiken::parser

  × While parsing files...
  ╰─▶ I found an unexpected token '";"'.
      
    ╭─[./validators/placeholder.ak:15:38]
 13 │ 
 14 │ fn return_result() {
 15 │   let result = apply_twice(double, 5);
    ·                                      ┬
    ·                                      ╰── 
 16 │   trace(result)
 17 │ }
    ╰────
  help: Try removing it!

      Summary 1 error, 0 warnings
```
#### Returning a function 
Another feature that comes with Aiken's functional nature is the composablity of functions. A function can compound and return accumulated inputs.
```rust
fn adder(n: Int) -> fn(Int) -> Int {
  fn(x: Int) -> Int {
    x + n
  }
}

fn return_result() {
  let add_five = adder(5)
  let result = add_five(3) // this will return 8
  trace(result)
}
```
### Anonymous Functions (Lambdas)

Aiken supports anonymous functions (lambdas) for concise, inline function definitions. The syntax is:
```markdown
fn(parameter) -> ReturnType { expression }
```
example
```rust
let  double = fn(x: Int) -> Int  { x * 2 }

let result = double(5) // result is 10

```
You'd usually implement it inside a function like this.
```rust
fn return_result() {
  let double = fn(x: Int) -> Int { x * 2 }
  let result = double(5) // result is 10
  trace(result)
}
```
This also means that the lambda functions are often used with higher-order functions
```rust
let result = apply_twice(fn(x: Int) -> Int { x * 2 }, 5) // result is 20
```
#### A brief on Trace
To be honest, the concept of a trace is kind of hard to grasp. There are also a number of ways to define traces. But first of what is a trace? 
There is no single definition of a trace. It's a lot of things. But most especially it is somewhat a debugging tool like javascripts console.log except that it doesn't really log anything you see unless during tests
Traces are also used for fast prototyping in the sense of using them as return statements for functions and also as placeholder values. They are your greatest ally when coding aiken. Although we won't be talking much about it right now, we will dwell on it when we talk about testing.

[previous](tutorial.md) [next](modules.md)
