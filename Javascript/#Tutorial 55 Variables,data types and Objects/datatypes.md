# JavaScript data types and data structures.

### **Dynamic and Weak Typing**

#### **Dynamic Typing:**
In dynamically typed languages, the type of a variable is determined at runtime, not at compile time. This means you don't have to declare the type of a variable explicitly; the language interpreter infers the type based on the value assigned to it. 

**Key Points:**
- The type is associated with the value, not the variable.
- Type errors occur during runtime.
- Variables can change types during execution.

**Example:**
```javascript
let x = 10;  // Initially a number
console.log(typeof x);  // Output: number

x = "Hello";  // Reassigned to a string
console.log(typeof x);  // Output: string

x = [1, 2, 3];  // Now a list
console.log(typeof x);  // Output: object
```

Here, `x` started as a number, then changed to a string, and later to an array. In dynamically typed languages, this is allowed.

#### **Weak Typing:**
In weakly typed languages, variables can be implicitly converted between different types without an explicit cast or conversion. This can lead to unexpected results if the language automatically converts types in ways you don't intend.

**Key Points:**
- Implicit type conversion occurs.
- Type errors might be less obvious because of automatic coercion.
- Some operations can behave unpredictably if types are not properly handled.

**Example:**
```javascript
let a = "5";
let b = 10;

let result = a + b;  // Implicit type conversion (string + number)
console.log(result);  // Output: "510" (string concatenation, not addition)

let c = "5" - 1;  
console.log(c);  // Output: 4 (automatic type conversion to number)
```

In this example:
- When adding `a` (a string) and `b` (a number), JavaScript automatically converts the number to a string and performs string concatenation.
- However, with subtraction (`-`), JavaScript converts the string `"5"` to a number and performs numeric subtraction.

#### **Summary:**
- **Dynamic Typing** allows variables to change types during execution, with type checking done at runtime.
- **Weak Typing** involves implicit type conversions, which can lead to unintended behavior if types are not handled carefully.

These features are typical in languages like JavaScript, Python, and PHP, offering flexibility but requiring developers to be cautious about unintended type conversions and runtime errors.

# Primitive Values
### **Primitive Values in JavaScript**

#### **Definition:**
Primitive values are the most basic data types in JavaScript that are immutable (their values cannot be changed once they are created). These types include numbers, strings, booleans, null, undefined, and symbols. They represent single values and are not objects or arrays.

#### **Characteristics of Primitive Values:**
1. **Immutability:** Once a primitive value is assigned to a variable, it cannot be altered.
2. **Pass-by-Value:** When a primitive value is assigned to a new variable or passed to a function, a copy of the value is created. Any changes made to the new variable or in the function do not affect the original value.
3. **No Properties or Methods:** Primitive values do not have properties or methods. However, JavaScript automatically wraps them in their corresponding object types when accessing properties or methods, but this does not modify the original value.

#### **Types of Primitive Values:**

1. **Number:**
   Represents both integer and floating-point numbers.
   
   **Example:**
   ```javascript
   let num1 = 10;  // Integer
   let num2 = 3.14;  // Floating-point number
   console.log(num1);  // Output: 10
   console.log(num2);  // Output: 3.14
   ```

   In JavaScript, numbers are represented in double-precision 64-bit format (IEEE 754).

2. **String:**
   Represents a sequence of characters enclosed in single quotes, double quotes, or backticks.

   **Example:**
   ```javascript
   let str1 = "Hello, World!";
   let str2 = 'JavaScript';
   let str3 = `Template literals`  // Can include expressions
   console.log(str1);  // Output: Hello, World!
   console.log(str2);  // Output: JavaScript
   console.log(str3);  // Output: Template literals
   ```

   Strings are immutable, meaning their values cannot be changed after they are created.

3. **Boolean:**
   Represents a logical entity, either `true` or `false`.

   **Example:**
   ```javascript
   let isActive = true;
   let isAvailable = false;
   console.log(isActive);  // Output: true
   console.log(isAvailable);  // Output: false
   ```

   Booleans are often used for conditional statements.

4. **Undefined:**
   A primitive value assigned to a variable that has been declared but not initialized.

   **Example:**
   ```javascript
   let uninitialized;
   console.log(uninitialized);  // Output: undefined
   ```

   If a variable is declared without a value, its initial value is `undefined`.
   

5. **Null:**
   Represents the intentional absence of any value or object. It is often used to indicate that a variable should not contain any object.

   **Example:**
   ```javascript
   let obj = null;
   console.log(obj);  // Output: null
   ```

   `null` is intentionally assigned, unlike `undefined`, which is the default state of uninitialized variables.

6. **Symbol (Introduced in ES6):**
   A unique and immutable data type used for creating unique identifiers. Symbols are often used in object properties for privacy or to avoid name collisions.

   **Example:**
   ```javascript
   const sym1 = Symbol('description');
   const sym2 = Symbol('description');
   console.log(sym1 === sym2);  // Output: false (Symbols are unique)
   ```

   Symbols are often used in situations where uniqueness is required, such as object keys.
   ### Symbol in JavaScript

#### Overview
`Symbol` is a built-in object in JavaScript, and its constructor returns a unique symbol primitive, known as a **Symbol value**. These symbols are used primarily for unique property keys on objects, ensuring no collision with other property names. Symbols can also be used for **weak encapsulation**, hiding data behind symbols to prevent accidental exposure.

#### Symbol Creation
A symbol can be created using the `Symbol()` function, and it's unique every time you call it, even if the description is the same.

##### Examples:
```javascript
const sym1 = Symbol(); // Unique Symbol
const sym2 = Symbol("foo"); // Symbol with description
const sym3 = Symbol("foo"); // A different Symbol even with same description
console.log(sym2 === sym3); // false
```

Note that `Symbol()` does not coerce the provided string into the symbol value. It creates a new and unique symbol every time.

You cannot create a symbol using the `new` keyword:
```javascript
const sym = new Symbol(); // TypeError
```
However, you can convert a symbol to an object using `Object()`:
```javascript
const sym = Symbol("foo");
const symObj = Object(sym);
console.log(typeof symObj); // "object"
```

#### Global Symbol Registry
Symbols created by `Symbol()` are guaranteed to be unique, but you can create a **global symbol registry** using `Symbol.for()`. This method ensures that the same symbol can be shared across different parts of a program or across different files.

##### Examples:
```javascript
const globalSym1 = Symbol.for("uniqueKey");
const globalSym2 = Symbol.for("uniqueKey");

console.log(globalSym1 === globalSym2); // true, they refer to the same symbol

const globalSym3 = Symbol.for("anotherKey");
console.log(Symbol.keyFor(globalSym3)); // "anotherKey"
```

The `Symbol.for()` method registers the symbol in the global registry and can be retrieved using `Symbol.keyFor()`.

#### Well-Known Symbols
**Well-known symbols** are predefined symbols in JavaScript used to enable certain customizations or operations within JavaScript objects.

Examples include:
- **`Symbol.iterator`**: Used to define a default iterator method for an object.
- **`Symbol.asyncIterator`**: Used to define a default async iterator for an object.
- **`Symbol.hasInstance`**: Determines if an object is an instance of a constructor.
- **`Symbol.toPrimitive`**: Used to convert an object to a primitive value.

##### Example of Well-Known Symbols:
```javascript
class MyClass {
  [Symbol.hasInstance](obj) {
    return obj instanceof MyClass;
  }
}

console.log({} instanceof MyClass); // false
console.log(new MyClass() instanceof MyClass); // true
```

#### Symbol Properties and Methods

- **`Symbol.prototype.description`**: A read-only string containing the description of the symbol.
- **`Symbol.prototype.toString()`**: Returns a string representing the symbol's description.
- **`Symbol.prototype.valueOf()`**: Returns the symbol itself.
- **`Symbol.prototype[Symbol.toPrimitive]()`**: Returns the symbol as a primitive.

#### Using Symbols in Objects
Symbols can be used as unique keys for object properties. These properties will be invisible to typical object property access methods like `for...in` or `JSON.stringify()`.

##### Example:
```javascript
const obj = {};
const symKey = Symbol("key");
obj[symKey] = "value";

console.log(obj[symKey]); // "value"
```

`for...in` iteration and `Object.getOwnPropertyNames()` will not return Symbol properties, but `Object.getOwnPropertySymbols()` will.

#### Symbols in `JSON.stringify()`
Symbol-keyed properties are ignored during serialization with `JSON.stringify()`.

##### Example:
```javascript
const obj = { [Symbol("foo")]: "bar" };
console.log(JSON.stringify(obj)); // "{}"
```

#### Summary
- **Symbols** are unique identifiers used primarily as property keys in objects to ensure uniqueness and avoid collisions.
- **Global registry**: Use `Symbol.for()` to create symbols that can be shared across different files and code blocks.
- **Well-known symbols**: These allow customizing language operations (e.g., iterators, async iterators).
- **Symbol properties** are hidden from typical access mechanisms like `JSON.stringify()`, and they are not enumerable in `for...in` loops.

### Examples of Symbols:
```javascript
// Symbol creation
const sym1 = Symbol("description");
const sym2 = Symbol("description");

// Comparing symbols
console.log(sym1 === sym2); // false

// Using symbol as object key
const obj = {};
const uniqueKey = Symbol("key");
obj[uniqueKey] = "symbol value";
console.log(obj[uniqueKey]); // "symbol value"

// Well-known symbol usage in an object
const iterableObj = {
  [Symbol.iterator]: function() {
    let i = 0;
    return {
      next: () => {
        if (i < 3) return { value: i++, done: false };
        return { done: true };
      }
    };
  }
};

for (const value of iterableObj) {
  console.log(value); // 0, 1, 2
}
```

In conclusion, **Symbols** provide a way to create unique identifiers and avoid property name collisions in JavaScript. They also serve a more advanced role in customizing how objects interact with JavaScript operations.

#### **Primitive Value Operations:**
1. **Numbers:**
   - Can be involved in mathematical operations like addition, subtraction, multiplication, and division.
   - Operations with numbers do not mutate the original value.

   **Example:**
   ```javascript
   let a = 5;
   let b = 10;
   let result = a + b;  // 15
   console.log(result);  // Output: 15
   ```

   ### **In-depth Notes on JavaScript `Number` Type**

#### **Overview**
In JavaScript, the `Number` type represents a double-precision 64-bit binary format value following the IEEE 754 standard. It is primarily used to store both integer and floating-point numbers. However, there are limitations on its precision and the range of values it can safely represent.

#### **Number Ranges and Limits**
- **Minimum Positive Value (`Number.MIN_VALUE`)**:
  The smallest positive number that JavaScript can represent, approximately **5e-324**. Numbers smaller than this are treated as **0**.

- **Maximum Value (`Number.MAX_VALUE`)**:
  The largest positive number that JavaScript can represent, approximately **1.8e308**. Numbers larger than this are converted to **+Infinity**.

- **Minimum Negative Value (`-Number.MAX_VALUE`)**:
  The largest negative number that JavaScript can represent, approximately **-1.8e308**. Numbers smaller than this are converted to **-Infinity**.

- **Smallest Negative Value (`-Number.MIN_VALUE`)**:
  The smallest negative number that JavaScript can represent, approximately **-5e-324**. Numbers larger than this are converted to **-0**.

#### **Safe Integer Range**
The `Number` type can safely represent integers only within the range:
- **Minimum Safe Integer (`Number.MIN_SAFE_INTEGER`)**: **-(2^53 - 1) = -9007199254740991**
- **Maximum Safe Integer (`Number.MAX_SAFE_INTEGER`)**: **2^53 - 1 = 9007199254740991**

Numbers outside this range cannot be represented accurately as integers and may be approximated as floating-point numbers.

- **Example:**
  ```javascript
  console.log(Number.isSafeInteger(9007199254740991)); // true
  console.log(Number.isSafeInteger(9007199254740992)); // false
  ```

#### **Special Number Values**
- **Infinity & -Infinity**
  JavaScript has two special values: **+Infinity** and **-Infinity**. These represent the concept of mathematical infinity and negative infinity.
  - Positive values greater than `Number.MAX_VALUE` are converted to **+Infinity**.
  - Negative values smaller than `-Number.MAX_VALUE` are converted to **-Infinity**.

  ```javascript
  console.log(1 / 0); // Infinity
  console.log(-1 / 0); // -Infinity
  ```

- **Zero Representation (+0 and -0)**
  Both **+0** and **-0** are treated as `0`, and they behave the same in most cases.
  - However, differences become evident when dividing by 0.

  ```javascript
  console.log(42 / +0); // Infinity
  console.log(42 / -0); // -Infinity
  console.log(+0 === -0); // true
  ```

- **NaN (Not a Number)**
  **NaN** is a special value used to represent the result of an invalid mathematical operation. It is unique in that it is not equal to itself.

  ```javascript
  console.log(0 / 0); // NaN
  console.log(NaN === NaN); // false
  ```

#### **Bitwise Operations**
- When using **bitwise operators** (`&`, `|`, `^`, `<<`, `>>`, etc.), the operands are first converted to **32-bit integers**. This conversion can lead to unexpected results when working with large integers or floating-point values.

  **Example of bitwise operations:**
  ```javascript
  let a = 5.5; // gets converted to 5
  let b = 3;
  console.log(a & b); // 1 (5 & 3 => 00000101 & 00000011 => 00000001)
  ```

  Bitwise operations may also be used to represent sets of Boolean values within a single number using **bit masking**, although this is considered a poor practice due to its low readability and maintainability. Instead, other data structures (e.g., arrays, objects) should be used to represent Booleans.

#### **Automatic Type Conversion**
JavaScript automatically converts numbers outside the representable range:
- **Greater than `Number.MAX_VALUE`** becomes **+Infinity**.
- **Smaller than `Number.MIN_VALUE`** becomes **0**.
- **Smaller than `-Number.MAX_VALUE`** becomes **-Infinity**.
- **Greater than `-Number.MIN_VALUE`** becomes **-0**.

  **Example:**
  ```javascript
  let largeNumber = 1e309;
  console.log(largeNumber); // +Infinity

  let smallNumber = 1e-400;
  console.log(smallNumber); // 0
  ```

#### **Checking Safe Integer**
You can check if a number is a "safe" integer (i.e., within the safe integer range) using `Number.isSafeInteger()`:
```javascript
console.log(Number.isSafeInteger(42)); // true
console.log(Number.isSafeInteger(1e18)); // false
```

#### **Summary**
- The `Number` type in JavaScript follows IEEE 754 standard for floating-point arithmetic.
- It has specific limits for safe integer representation.
- Special values like **Infinity**, **-Infinity**, **NaN**, **+0**, and **-0** exist for handling edge cases.
- Bitwise operators convert numbers to 32-bit integers, which can cause precision loss for large values.
- **Bit masking** and bitwise operators for representing Boolean sets are discouraged for readability purposes.

This is an essential overview for handling numeric values, edge cases, and arithmetic in JavaScript.

2. **Strings:**
   - Strings can be concatenated using the `+` operator.
   - Like numbers, strings are immutable. Modifying a string results in a new string.

   **Example:**
   ```javascript
   let firstName = "John";
   let lastName = "Doe";
   let fullName = firstName + " " + lastName;
   console.log(fullName);  // Output: John Doe
   ```
   ### **In-depth Notes on JavaScript `String` Type**

#### **Overview**
The `String` type in JavaScript represents textual data. Internally, JavaScript strings are encoded as a sequence of **16-bit unsigned integer values**, which are UTF-16 code units. A string is essentially a collection of these code units, where each character occupies a position in the string. The string index starts from `0` for the first character, `1` for the second character, and so on.

#### **String Length and Encoding**
- The **length** of a string is determined by the number of UTF-16 code units it contains, not necessarily the actual number of Unicode characters. This distinction can matter in certain cases, especially when dealing with characters that require more than one UTF-16 code unit (such as emoji or certain special characters).

- **Example**:
  ```javascript
  const str = 'abc';
  console.log(str.length);  // 3, because there are three UTF-16 code units
  ```

- Some Unicode characters (like emoji or characters from certain languages) might require multiple 16-bit units (surrogate pairs), so they could be counted as more than one unit.

#### **String Immutability**
Strings in JavaScript are **immutable**, meaning once a string is created, it cannot be changed. Any operations that appear to modify a string, such as using a method to change its content, will instead create a new string based on the operation's result. The original string remains unchanged.

- **Example**:
  ```javascript
  let str = 'Hello';
  str[0] = 'h'; // This will not work
  console.log(str); // 'Hello' — The string remains unchanged
  ```

#### **String Methods**
Various methods are available to manipulate strings and extract data, but all of them create a new string.

1. **`substring()`**: Extracts a portion of a string, returning a new string.
   ```javascript
   let str = 'Hello World';
   let sub = str.substring(0, 5);
   console.log(sub); // 'Hello'
   ```

2. **Concatenation**: Strings can be concatenated using either the `+` operator or the `concat()` method.
   ```javascript
   let str1 = 'Hello';
   let str2 = ' World';
   console.log(str1 + str2); // 'Hello World'

   // or using concat()
   console.log(str1.concat(str2)); // 'Hello World'
   ```

3. **String Transformation**: Methods like `toLowerCase()` and `toUpperCase()` return a new string with the content in the desired case.
   ```javascript
   let str = 'Hello World';
   console.log(str.toLowerCase()); // 'hello world'
   console.log(str.toUpperCase()); // 'HELLO WORLD'
   ```

4. **`split()`**: Turns a string into an array by splitting it based on a separator.
   ```javascript
   let str = 'apple,banana,cherry';
   let arr = str.split(',');
   console.log(arr); // ['apple', 'banana', 'cherry']
   ```

#### **Beware of "Stringly-Typed" Code**
JavaScript allows the use of strings in many APIs and data representations, but this can lead to issues if strings are used to represent complex data structures. This practice is known as **stringly-typed** code. While it may be convenient in the short term, it can create significant problems over time.

1. **Easy to Build Strings with Concatenation**: It's straightforward to concatenate strings, especially when building URLs or queries. However, when strings are used to represent complex structures, it can lead to bugs and data inconsistencies.
   
2. **Debugging Strings**: Debugging strings can be easier because what is printed or logged is exactly what is in the string. However, representing non-textual data (e.g., lists or objects) as strings may obfuscate the intent of the code.

3. **Separation or Escape Characters**: Using a string to represent a complex structure (e.g., a list) often requires a separator or escape character. This can introduce errors when data includes the separator, breaking the structure.

   **Example (Bad Practice)**:
   ```javascript
   // Storing a list as a string with commas as separators
   let list = 'apple,banana,cherry';
   
   // If the list contains an item with a comma
   list = 'apple,banana,orange,grape,fruit,orange,apple';

   // This approach is error-prone if commas are used in actual data
   ```

#### **Best Practices**
- **Strings for Textual Data**: Strings should be used primarily for textual data — text, labels, and simple values.
  
- **Avoid Stringly-Typed Data Structures**: Instead of representing complex data structures with strings, use JavaScript's built-in types like **arrays** or **objects**. For example, use an array to represent a list of items rather than concatenating the items into a string.
  
  **Better Practice Example**:
  ```javascript
  // Use an array for a list of fruits
  let fruits = ['apple', 'banana', 'cherry'];
  
  // You can still join them to form a string if needed
  let fruitStr = fruits.join(',');
  console.log(fruitStr); // 'apple,banana,cherry'
  ```

- **Parse Strings When Necessary**: If you must use a string to represent complex data, ensure that you parse and handle the string properly (e.g., by splitting or using `JSON.parse()` for JSON data).

#### **Example of Complex Data Representation**
Instead of using strings to represent complex data, use structured types like arrays or objects.

**Bad Practice (Stringly-Typed)**:
```javascript
let user = "John;25;true";
```
In this case, you're encoding data about a user (name, age, and active status) into a string, which can become error-prone when parsing the string.

**Better Practice (Object)**:
```javascript
let user = { name: "John", age: 25, isActive: true };
```
With this approach, the data is easier to access, modify, and maintain.

#### **Conclusion**
The `String` type in JavaScript is an essential part of the language for handling textual data. It provides a variety of methods for manipulating and working with text, but developers should be cautious about using strings to represent complex data structures. Instead, use strings for simple textual data and employ arrays, objects, and other suitable abstractions for more complex data representations to avoid bugs and maintenance issues.

3. **Booleans:**
   - Booleans are commonly used in logical expressions and conditional statements.

   **Example:**
   ```javascript
   let age = 20;
   let isAdult = age >= 18;
   console.log(isAdult);  // Output: true
   ```

   ### **In-depth Notes on JavaScript `BigInt` Type**

#### **Overview**
The `BigInt` type is a numeric primitive in JavaScript that allows for the representation and manipulation of integers with arbitrary magnitude. This type was introduced to handle situations where integers exceed the limits of the `Number` type, which is constrained by the IEEE 754 double-precision floating-point format. `BigInt` can safely represent integers that are larger than `Number.MAX_SAFE_INTEGER` (which is **9007199254740991**), allowing for the handling of arbitrarily large numbers without loss of precision.

#### **Creating BigInts**
A `BigInt` can be created in two primary ways:
1. **Appending `n` to an integer**:
   ```javascript
   const x = 12345678901234567890n;  // BigInt literal
   ```

2. **Using the `BigInt()` function**:
   ```javascript
   const y = BigInt(12345678901234567890);  // Using the BigInt() constructor
   ```

   - **Note**: The `BigInt` constructor can be used with numeric strings or numbers, but the argument passed must be an integer.

   ```javascript
   const z = BigInt("12345678901234567890");  // String argument
   ```

#### **Comparison with `Number` Type**
- **Safe Integer Limits**: The `Number` type can only safely represent integers within the range of `Number.MIN_SAFE_INTEGER` to `Number.MAX_SAFE_INTEGER`, which is approximately **±9 quadrillion**. Any integer larger than this will suffer from precision loss.
- **BigInt Handling**: `BigInt` can store integers far beyond this range without any precision issues. For instance, values like **9007199254740992n** (greater than `Number.MAX_SAFE_INTEGER`) can be stored in a `BigInt`.

- **Example:**
  ```javascript
  // BigInt example
  const x = BigInt(Number.MAX_SAFE_INTEGER); // 9007199254740991n
  console.log(x + 1n); // 9007199254740992n

  // Number example
  console.log(Number.MAX_SAFE_INTEGER + 1); // 9007199254740992 (loss of precision)
  ```

- In the example above, adding `1n` to a `BigInt` correctly increments the value, while the same operation with a `Number` results in precision loss.

#### **Operations with BigInt**
Most arithmetic operations, including addition (`+`), multiplication (`*`), subtraction (`-`), exponentiation (`**`), and modulus (`%`), are supported with `BigInt`:
```javascript
const a = 10n;
const b = 3n;
console.log(a + b); // 13n
console.log(a * b); // 30n
console.log(a - b); // 7n
console.log(a ** b); // 1000n
console.log(a % b); // 1n
```

- **Exception**: The **bitwise shift operator (`>>>`)** is not allowed with `BigInt` because it is defined only for 32-bit signed integers.

#### **Comparison Between BigInt and Number**
- **Strict Equality**: A `BigInt` is not strictly equal to a `Number` with the same mathematical value. Even if a `BigInt` represents the same number as a `Number`, the two types are considered different. For example:
  ```javascript
  const x = 1n;
  const y = 1;
  console.log(x === y); // false
  ```
  The `===` operator checks both the type and value, and since the types differ (BigInt vs. Number), it returns `false`.

- **Loose Equality**: However, `BigInt` and `Number` can be loosely equal (with `==`), which means their values may match despite different types:
  ```javascript
  console.log(x == y); // true
  ```

#### **Limitations of BigInt**
- **No Fractional Numbers**: Unlike the `Number` type, `BigInt` cannot represent fractional values (decimals). It can only represent whole integers.
  ```javascript
  const z = 5.5n; // TypeError: Cannot convert a floating point number to BigInt
  ```

- **Arithmetic with Mixed Types**: Mixing `BigInt` with regular `Number` values in arithmetic operations will result in a `TypeError`. For example:
  ```javascript
  const a = 10n;
  const b = 5;
  console.log(a + b); // TypeError: Cannot mix BigInt and other types
  ```

  To perform operations between a `BigInt` and a `Number`, both values must be of the same type. You can convert the `Number` to a `BigInt` using `BigInt()`:
  ```javascript
  console.log(a + BigInt(b)); // 15n
  ```

#### **Use Cases for BigInt**
`BigInt` is useful when you need to work with very large integers that exceed the safe limits of the `Number` type. This can occur in various scenarios, such as:
- Cryptography (working with large prime numbers)
- Financial applications (handling large sums of money in certain currencies)
- Scientific computing where extremely large integers are required

#### **Summary**
- `BigInt` allows you to safely store and work with integers that are larger than the `Number.MAX_SAFE_INTEGER` limit.
- It can represent arbitrarily large integers without losing precision, but it **cannot** represent fractional numbers.
- You can use most arithmetic operators (`+`, `-`, `*`, `/`, etc.), but **bitwise shift operations (`>>>`)** are not supported.
- `BigInt` and `Number` are distinct types and cannot be mixed in expressions directly.
- When performing operations with mixed types, you need to explicitly convert one type to match the other.

By understanding these points, you can use `BigInt` effectively for large integer operations in JavaScript.

#### **Pass-by-Value Behavior:**
In JavaScript, primitive values are passed by value. This means that when a primitive value is passed to a function or assigned to another variable, the actual value is copied, and the original value remains unchanged.

**Example:**
```javascript
let num1 = 10;

function modifyValue(x) {
  x = 20;  // This modifies only the local copy
}

modifyValue(num1);
console.log(num1);  // Output: 10 (original value is unaffected)
```

In the above example, the original `num1` value remains `10` because the function `modifyValue` only modifies a copy of the value passed to it.

#### **Comparison of Primitive Values:**

- **Equality (==):**
  The `==` operator checks for value equality, converting types if necessary (type coercion).

  **Example:**
  ```javascript
  let a = "5";
  let b = 5;
  console.log(a == b);  // Output: true (type coercion)
  ```

- **Strict Equality (===):**
  The `===` operator checks for both value and type equality without type coercion.

  **Example:**
  ```javascript
  let a = "5";
  let b = 5;
  console.log(a === b);  // Output: false (no type coercion)
  ```

#### **Conclusion:**
- **Primitive values** are fundamental types that are immutable and hold a single value. They include `number`, `string`, `boolean`, `null`, `undefined`, and `symbol`.
- **Pass-by-value** behavior means that copying primitive values doesn't alter the original data, making them efficient for use in operations and function calls.
- **Immutability** ensures that primitive values cannot be changed directly, preventing unwanted side effects in your programs.

By understanding primitive values and their properties, you can write more predictable and bug-free code in JavaScript.



# Objects
In JavaScript, an **object** is a collection of key-value pairs, where each key is a string (or symbol), and the value can be any type of data. Objects are used to store and manage data. Unlike primitive types (like numbers or strings), objects are mutable, meaning their properties can be changed after they are created. Functions are a special kind of object that can also be called to execute code.

## Properties
### Objects in JavaScript: In-Depth Detailed Notes

**Definition of Objects:**
In JavaScript, an **object** is a collection of properties, where each property is defined by a key-value pair. Objects allow you to store data and manage it in a structured way. Objects are mutable, meaning their values can be changed after they are created. Functions are also objects in JavaScript, with the special ability to be callable (i.e., executed).

### 1. **Properties of Objects**
An object can be considered a collection of properties, each identified by a key, which can be a string or symbol. The values associated with these keys can be of any data type, including other objects.

#### **Property Types:**
There are two types of properties in an object:
- **Data Properties**
- **Accessor Properties**

**Property Keys:**  
- Typically strings or symbols, although other types (such as numbers) are implicitly converted to strings.

**Property Values:**  
- Can be any data type, including other objects. This allows you to build complex nested data structures using objects.

### 2. **Data Properties**
A **data property** links a key to a value and has the following attributes:

- **value**: The value associated with the property. Can be any JavaScript value.
- **writable**: A boolean that indicates whether the property can be modified. If set to `false`, the property is read-only.
- **enumerable**: A boolean that determines if the property can be enumerated in a `for...in` loop.
- **configurable**: A boolean that indicates if the property can be deleted or its attributes changed (like converting it into an accessor property).

**Example:**
```javascript
const person = {
  name: "John",
  age: 30
};

// Accessing the properties
console.log(person.name); // "John"
console.log(person.age);  // 30

// Modifying a property
person.age = 31;
console.log(person.age);  // 31
```

### 3. **Accessor Properties**
An **accessor property** uses getter and setter functions to retrieve or store values, instead of directly associating a key with a value. These properties have the following attributes:

- **get**: A function that retrieves the property value when accessed.
- **set**: A function that is called when a new value is assigned to the property.
- **enumerable**: A boolean that determines if the property can be enumerated in a `for...in` loop.
- **configurable**: A boolean that specifies if the property can be deleted or changed to a data property.

**Example:**
```javascript
const person = {
  firstName: "John",
  lastName: "Doe",
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
};

// Getter and Setter with Object.defineProperty
Object.defineProperty(person, "age", {
  get() {
    return this._age;
  },
  set(value) {
    this._age = value;
  },
  enumerable: true,
  configurable: true
});

// Using getter and setter
person.age = 25;
console.log(person.age);  // 25
```

### 4. **Prototype Chain and [[Prototype]]**
Every JavaScript object has a hidden internal property called `[[Prototype]]`. This property points to another object or `null`. When you try to access a property of an object, if the property does not exist on that object, JavaScript will look for it in the prototype chain (the object's prototype).

**Example:**
```javascript
const animal = {
  eat() {
    console.log("Eating...");
  }
};

const dog = Object.create(animal);
dog.bark = function() {
  console.log("Barking...");
};

dog.bark();  // "Barking..."
dog.eat();   // "Eating..." (inherited from animal)
```

### 5. **Maps and Sets**
Objects are often used for key-value pairs, but **Maps** and **Sets** offer more optimized data structures for storing and managing key-value associations.

#### **Map**
A **Map** is a collection of key-value pairs where keys can be any data type, including objects. Maps maintain the order of insertion.

**Example:**
```javascript
const map = new Map();
map.set("name", "John");
map.set("age", 30);
console.log(map.get("name")); // "John"
```

#### **Set**
A **Set** is a collection of unique values. It allows you to store values without duplication.

**Example:**
```javascript
const set = new Set();
set.add(1);
set.add(2);
set.add(2);  // Duplicate values are ignored
console.log(set); // Set { 1, 2 }
```

#### **WeakMap and WeakSet**
These are special types of collections where the keys (for WeakMap) or the values (for WeakSet) can be garbage-collected when they are no longer in use. They are used for memory optimization.

### 6. **Arrays and Typed Arrays**
Arrays in JavaScript are special types of objects designed for ordered lists, where elements are indexed by integers.

#### **Arrays**
An array is an object with an integer-based index and a `length` property. Arrays also come with built-in methods to manipulate data, like `push()`, `pop()`, and `indexOf()`.

**Example:**
```javascript
const arr = [10, 20, 30];
arr.push(40);
console.log(arr);  // [10, 20, 30, 40]
```

#### **Typed Arrays**
A **Typed Array** is an array-like object that represents a view over an underlying binary data buffer. Examples include `Int8Array`, `Float32Array`, and so on. Typed Arrays are used for handling raw binary data.

**Example:**
```javascript
const buffer = new ArrayBuffer(16); // 16 bytes buffer
const int32View = new Int32Array(buffer);
int32View[0] = 42;
console.log(int32View[0]);  // 42
```

### 7. **JSON (JavaScript Object Notation)**
JSON is a lightweight data-interchange format derived from JavaScript. It represents data as a string in key-value pairs, making it easy to transfer data between systems.

**Example:**
```javascript
const person = { name: "John", age: 30 };
const jsonString = JSON.stringify(person); // Convert object to JSON string
console.log(jsonString);  // '{"name":"John","age":30}'

const parsedObject = JSON.parse(jsonString); // Convert JSON string back to object
console.log(parsedObject);  // { name: "John", age: 30 }
```

### 8. **Standard Library Objects**
JavaScript comes with a variety of built-in objects that provide functionality for common tasks. Some common built-in objects are:
- `Math`: For mathematical operations
- `Date`: For handling date and time
- `RegExp`: For regular expressions

### Summary of Key Concepts:
- **Objects**: A collection of key-value pairs, where the key is usually a string or symbol, and the value can be any data type, including other objects.
- **Data Properties**: Store a value directly, with attributes like `writable`, `enumerable`, and `configurable`.
- **Accessor Properties**: Use getter and setter functions to interact with the values.
- **Prototype**: Every object has a prototype, from which it can inherit properties and methods.
- **Maps/Sets**: Specialized collections that store key-value pairs or unique values, respectively, with optimized performance.
- **Arrays**: Special objects for storing ordered data with integer keys.
- **JSON**: A lightweight data format for transferring data between systems.

These concepts are foundational for understanding JavaScript and building robust, complex applications.

# Type Coercion
### Type Coercion in JavaScript

JavaScript is a weakly typed language, meaning that values of different types can be automatically converted (coerced) into compatible types when required. Type coercion occurs in a variety of contexts, depending on the operation being performed.

### 1. **Primitive Coercion**
Primitive coercion happens when a primitive value (string, number, BigInt, etc.) is expected, but JavaScript has flexibility in which type it allows. This is common in scenarios where a string, number, or BigInt are equally acceptable.

#### Example of Primitive Coercion:
- **Date Constructor:**
  The `Date()` constructor can accept a string or number to create a date.
  ```javascript
  let date1 = new Date("2025-01-01");
  let date2 = new Date(2025, 0, 1); // year, month, day
  console.log(date1, date2); // Output: Thu Jan 01 2025... (Date objects)
  ```

- **The `+` Operator:**
  The `+` operator behaves differently based on the operand types.
  - If one operand is a string, string concatenation is performed.
  - If both operands are numbers, numeric addition occurs.
  ```javascript
  let num = 5;
  let str = "5";
  console.log(num + str); // Output: "55" (string concatenation)

  let a = 3;
  let b = 7;
  console.log(a + b); // Output: 10 (numeric addition)
  ```

- **Equality (`==`) Operator:**
  The `==` operator compares two operands for equality, and when one is an object and the other a primitive, JavaScript will convert the object to a primitive.
  ```javascript
  let obj = { a: 1 };
  let str = "[object Object]";
  console.log(obj == str); // Output: true (object coerced to string)
  ```

#### Converting Objects to Primitives:
Objects are not primitives, so JavaScript needs to convert them to primitive values when necessary. It does this by calling methods in the following order:
1. **[Symbol.toPrimitive]()** (if present)
2. **valueOf()** (if present)
3. **toString()** (if needed)

#### Example of Object to Primitive Coercion:
```javascript
let obj = {
  valueOf() {
    return 5;
  },
  toString() {
    return "object";
  }
};

console.log(obj + 10); // Output: 15 (valueOf() is used)
```

If the `valueOf()` method returns an object, `toString()` is called instead:
```javascript
console.log({} + []); // Output: "[object Object]" (toString() is called)
```

### 2. **Numeric Coercion**
JavaScript has two numeric types: `Number` and `BigInt`. When the language expects a number or BigInt, it performs numeric coercion.

#### Example of Numeric Coercion:
- **Unary plus (`+`) operator:** 
  Converts the operand to a number.
  ```javascript
  let strNum = "10";
  console.log(+strNum); // Output: 10 (string converted to number)
  ```

- **Arithmetic Operations:**
  Numeric operations such as addition, subtraction, and multiplication convert operands to numbers or BigInts as required.
  ```javascript
  let str = "5";
  console.log(str * 2); // Output: 10 (string coerced to number)
  ```

### 3. **Coercion of Other Data Types**

- **String Coercion:**  
  When JavaScript expects a string, it will convert other types to a string using the `toString()` method.
  ```javascript
  let obj = { a: 1 };
  console.log(String(obj)); // Output: "[object Object]" (toString() called)
  ```

- **Boolean Coercion:**  
  JavaScript converts values to boolean values in conditional statements, using truthy and falsy values.
  ```javascript
  let truthyValue = "Hello";
  let falsyValue = 0;
  console.log(Boolean(truthyValue)); // Output: true
  console.log(Boolean(falsyValue)); // Output: false
  ```

- **Object Coercion:**  
  Objects are converted to primitive values (using `valueOf()` or `toString()`).
  ```javascript
  let obj = { name: "John" };
  console.log(obj + 10); // Output: "[object Object]10" (object coerced to string)
  ```

### 4. **[Symbol.toPrimitive]() Method**

The `[Symbol.toPrimitive]()` method is a special method that JavaScript uses to convert an object to a primitive value. This method takes a hint (either "string", "number", or "default") and returns a primitive value.

#### Example of [Symbol.toPrimitive]:
```javascript
let obj = {
  [Symbol.toPrimitive](hint) {
    if (hint === "string") {
      return "This is a string";
    }
    if (hint === "number") {
      return 42;
    }
    return "default";
  }
};

console.log(+obj); // Output: 42 (numeric coercion)
console.log(`${obj}`); // Output: "This is a string" (string coercion)
```

### 5. **Coercion Process Summary**

There are three main types of coercion when converting objects to primitives:
1. **Primitive Coercion:**  
   `[Symbol.toPrimitive]("default") → valueOf() → toString()`
   
2. **Numeric Coercion:**  
   `[Symbol.toPrimitive]("number") → valueOf() → toString()`

3. **String Coercion:**  
   `[Symbol.toPrimitive]("string") → toString() → valueOf()`

If a custom `[Symbol.toPrimitive]()` method is present, it always takes precedence in the conversion process.

### Conclusion

JavaScript’s type coercion system is flexible and allows values of different types to be converted automatically, depending on the operation being performed. Understanding the coercion rules is crucial for writing bug-free code, especially when working with operators and data types that can involve implicit type conversions.