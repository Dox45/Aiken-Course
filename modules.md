# Modules
What are Modules in Aiken?
Earlier in our tutorial I mentioned about modules and how to import it in Aiken. The concept of modules is so important that we are have to dedicate a section to it. You see, the Aiken Standard library (where all pre-writtten modules can be found) can be very confusing owing to its arrangement. It is worth exploring and explaining to shed more about its use and how to not get lost in the vague explanations of the std library.
## Module basics
In Aiken, every file is a module. The module name corresponds to the file path.

```rust
// File: lib/myproject/utils.ak
// Module name: myproject/utils
```

To import a module, we typically use the "use" keyword. 

```rust
// File: myproject/utils.ak 
use myproject/utils

// you can also import specific functions
use myproject/utils.{add, double_add} 

//aiken also supports aliases
use myproject/utils as u
```
Aiken does not support relative imports "using ..." and so this means that every module to be imported has same common folder as the main module that is calling the imports
To demonstrate what we mean, let us create a simple module

Example:
```
`lib/
 ├── math/
 │    └── utils.ak 
 └── main.ak` 
 ```

This creates two modules:
```

-   `math/utils` → accessible as `math/utils`
    
-   `main` → the root module (entry point) 
```
Inside our math/utils.ak file we can write a simple maths function

```rust
pub fn add(a: Int, b: Int) -> Int {
  a + b
}

pub fn multiply(a: Int, b: Int) -> Int {
  a * b
}
```
Then in our main module "main.ak" file. We can import it as 

```rust
use math/utils as u //using u as alias

pub fn run() -> Int {
  let x = u.add(2, 3)
  let y = u.multiply(x, 10)
  y
}

```

To make the contetns of a module accessible to other modules, the functions are declared using the pub keyword, as in the case above.
Let us take a look at an example without the pub function
```rust
// math/utils.ask
fn add(a: Int, b: Int) -> Int {
  a + b
}

pub fn multiply(a: Int, b: Int) -> Int {
  a * b
}
```
Running this gives us the error below


```bash
 × While trying to make sense of your code...
  ╰─▶ I looked for 'add' in 'utils' but couldn't find it.
      
   ╭─[./lib/validators/placeholder.ak:7:12]
 5 │ 
 6 │ pub fn run() -> Int {
 7 │   let x = u.add(2, 3)
   ·            ──┬─
   ·              ╰── not exported by utils?
 8 │   let y = u.multiply(x, 10)
 9 │   y
   ╰────
  help: Did you forget to make this value public?
```

### Importing and Exporting Custom Types
What are Custom Types?
In Aiken, **custom types** let you define your own data structures — beyond the built-in ones like `Int`, `Bool`, or `String`.  
They describe _what kind of data_ your program works with, and help make your code safer, more readable, and easier to reason about.

Custom types are _immutable_ and _strongly typed_, meaning once you define and construct them, their structure and types cannot change.

Aiken supports several forms of custom types and they are defined with the `type` or `pub type` keyword.

### Records (Product Types)
Records are like structs in other languages; they are a collection of named fields
```rust
// Define a record type
pub type Person {
  name: ByteArray,
  age: Int,
  is_active: Bool,
}
```
Here, `Person` is both:

-   The **type name** (`Person`)
    
-   And the **constructor** (`Person { name:"Chima", age: 10, is_active: True }`)

 ```rust
// Create a record
pub fn create_person() -> Person {
  Person {
    name: "Alice",
    age: 30,
    is_active: True,
  }
}
```
Similary just like other types, a function can return a custom type as shown above.

To access the objects in the type, we use the dot notation.
```rust
// Access fields with dot notation
pub fn get_name(person: Person) -> ByteArray {
  person.name
}
//or

pub fn sayName() -> ByteArray {
  let boy = Person {name:"Chima", age:10, is_active:False }
  boy.name
  
}
//returns Chima. Also remember you can't use the let keyword outside a function scope.
```
### Enum (Variant) Types

An **enum** (or _sum type_) represents a value that can take one of several different _forms_.  
Each form (called a **constructor**) can hold different fields or no fields at all.

```rust
// Simple enum 
pub type Status { 
	Active 
	Inactive
	Pending
  } 

// Enum with associated data 
pub type Action {
	 Mint { quantity: Int, policy_id: ByteArray } 
	 Burn { quantity: Int, policy_id: ByteArray } 
	 Transfer { to: ByteArray, amount: Int } 
	 Cancel 
 } 

// Enum for Result-like patterns 
pub type ValidationResult { 
	Valid { message: ByteArray } 
	Invalid { err: ByteArray } 
}
```

### Type Aliases

You can also define **aliases** — alternative names for existing types.

```rust
pub type Address = ByteArray
```
As an aside, notice the difference between a **Record Type** and an **Enum**. When declaring the contents of the Record Type, the values are comma separated; on the other hand, for the Enum, there is no comma separation. This is because the Enum doesn't hold data types or fields; it contains the various forms that the type could assume. Although when these forms are declared, they can take in different data types separated by commas.

Just like every other module, types defined with the pub keyword can be exported or imported into another module
We would then proceed to create a type module, let's call it geometry
`types/geometry`
```
lib/
 ├── types/
 │   └── geometry.ak
 ├── validators/
 │   └── position.ak
 └── main.ak
 ```
let us add this content to it.
```rust

pub type Point {
  Point { x: Int, y: Int }
}

pub fn origin() -> Point {
  Point { x: 0, y: 0 }
}
```
Here:

-   The `pub type` makes the `Point` type visible to other modules.
    
-   The constructor `Point { ... }` can also be used outside if the type is public.
    
-   `origin()` is a helper function returning a `Point`.

You can then import it and use it inside your validator module as
```rust
use types/geometry

pub fn is_origin(p: geometry.Point) -> Bool {
  p.x == 0 && p.y == 0
}

pub fn demo() -> Bool {
  let o = geometry.origin()
  is_origin(o)
}
```

Remember:
You import the module using its namespace path (use types/geometry).

You reference the type as geometry.Point (qualified with the module name).

You can call the constructor or helper functions defined in geometry.

## Aiken Standard Library
The Aiken programming language provides the standard std-lib that contains prewritten modules that are used for a diverse range of functions.
Oftentimes developers get lost using the std-lib due to its complicated nature and not very descriptive pattern. 
Here is a brief guide on the std-lib

## Using the Standard Library (`std`)

Aiken comes with a comprehensive **standard library (`std`)** that provides ready-to-use modules for mathematics, collections, Cardano-specific types, cryptography, and more. These modules let you focus on smart contract logic instead of reimplementing common utilities. They're divided into two sections. The Aiken Specific Modules and the Cardano Specific Modules

**Repository:** https://github.com/aiken-lang/stdlib
**Website:** https://aiken-lang.github.io/stdlib

You can import a standard module directly using the `use` keyword:

```rust
use aiken/collection/list
use aiken/math
use cardano/transaction
``` 

Below is a brief overview of the main modules available in `std`:

###  Core Modules
| Module | Description |
|--------|--------------|
| `aiken` | Core language utilities and built-in helpers. |
| `aiken/cbor` | Encode and decode data in CBOR format. |
| `aiken/collection` | Basic collection interfaces and tools. |
| `aiken/collection/dict` | Dictionary (key-value map) utilities. |
| `aiken/collection/list` | Common list manipulation functions (map, fold, filter, etc). |
| `aiken/collection/pairs` | Functions for working with 2-element tuples (pairs). |
| `aiken/option` | Optional (maybe) type helpers — handle presence/absence safely. |
| `aiken/primitive` | Basic primitive operations (numbers, bytes, etc). |

---

### Math & Numbers
| Module | Description |
|--------|--------------|
| `aiken/math` | General math utilities. |
| `aiken/crypto/int224`, `aiken/int256` | Fixed-size integer operations. |
| `aiken/interval` | Interval arithmetic utilities. |
| `aiken/math/rational` | Rational number utilities (fractions). |

---

### Cryptography
| Module | Description |
|--------|--------------|
| `aiken/crypto` | Cryptographic primitives and utilities. |
| `aiken/crypto/bls12_381` | Elliptic curve pairing operations (used in advanced crypto). |
| `aiken/crypto/bls12_381/g1`, `aiken/crypto/bls12_381/g2` | Specific curve groups for BLS12-381. |
| `aiken/crypto/bls12_381/pairing` | Pairing-based cryptography functions. |
| `aiken/crypto/bls12_381/scalar` | Scalar field operations for cryptographic math. |

---

### Cardano-Specific Modules
| Module | Description |
|--------|--------------|
| `cardano/address` | Tools for working with Cardano addresses. |
| `cardano/address/credential` | Manage credentials and key hashes. |
| `cardano/assets` | Handle multi-asset values and tokens. |
| `cardano/certificate` | Access stake delegation and registration certificates. |
| `cardano/governance` | Work with governance-related data types. |
| `cardano/governance/protocol_parameters` | Inspect current protocol parameters. |
| `cardano/script_context` | Access transaction and script execution context. |
| `cardano/transaction` | Build and inspect transactions and their components. |
| `cardano/transaction/output_reference` | Reference UTXO outputs within a transaction. |
| `cardano/transaction/script_purpose` | Determine the purpose (spending, minting, etc.) of the script. |
| `cardano/governance/voter` | Manage governance voter credentials. |

---

### Utilities
| Module | Description |
|--------|--------------|
| `aiken/primitive/bytearray` | Work with raw byte arrays. |
| `aiken/primitve/int` | Integer utility functions. |
| `aiken/primitve/string` | String manipulation and formatting. |
| `aiken/collection/dict/strategy` | Reusable strategy patterns for common operations. |




### Example

```rust
use aiken/collection/list
use cardano/transaction

pub fn count_outputs(tx: transaction.Transaction) -> Int {
  list.length(tx.outputs)
}
```

The standard library evolves alongside the Aiken compiler—you can check the latest modules and functions on the [official stdlib GitHub page](https://github.com/aiken-lang/stdlib).
