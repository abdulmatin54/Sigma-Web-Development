# Object
### In-Depth Notes on JavaScript `Object` Prototype and its Methods

The **Object** type is a fundamental data structure in JavaScript used to store keyed collections and complex entities. It serves as the base prototype for almost all objects in JavaScript. The object prototype is an essential part of JavaScript’s object-oriented nature, allowing object inheritance and the ability to define custom properties and methods.

### 1. **Object Constructor and Literal Syntax**
Objects in JavaScript can be created using either:
- **Object() Constructor**: 
  ```javascript
  const obj1 = new Object();
  ```
- **Object Literal**: 
  ```javascript
  const obj2 = { key: "value" };
  ```

### 2. **Object.prototype and Inheritance**
The **Object.prototype** is the parent of nearly all JavaScript objects. This means that all objects inherit methods and properties from `Object.prototype` unless explicitly overridden.

- **Important point**: Objects that do not inherit from `Object.prototype` are those with a `null` prototype. These are often special objects used in certain low-level scenarios (e.g., objects created using `Object.create(null)`).

### 3. **Inheritance Mechanism (Prototype Chain)**
JavaScript objects inherit properties and methods through a **prototype chain**. When you call a method or access a property on an object, JavaScript first looks for it within the object itself. If not found, it looks up the prototype chain.

For example:
```javascript
const obj = { a: 1 };
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("toString")); // false
```
Here, `hasOwnProperty` is defined in `Object.prototype`, which is why it's available to all objects.

### 4. **Modifying the `Object.prototype`**
Changes made to **Object.prototype** affect all objects that inherit from it (which is nearly every object in JavaScript). This can be a powerful mechanism but can also lead to unexpected results if properties or methods are overridden.

- **Caution**: You should be careful when modifying `Object.prototype`, as it can affect the behavior of all objects in your application, possibly leading to bugs that are difficult to trace.

### 5. **Common Object Methods**
- **`valueOf()`**, **`toString()`**, and **`toLocaleString()`**: These are common polymorphic methods that may be overridden by objects to customize their behavior.
  - **Example**: 
    ```javascript
    const obj = {
      valueOf() {
        return 42;
      }
    };
    console.log(obj + 0); // 42, calls valueOf() for implicit type conversion
    ```

- **`__proto__` Property**: This is a deprecated property that provides access to an object's prototype. Instead, you should use `Object.getPrototypeOf()` to get an object's prototype.
  ```javascript
  const obj = { a: 1 };
  console.log(Object.getPrototypeOf(obj)); // Object.prototype
  ```

- **`propertyIsEnumerable()`** and **`hasOwnProperty()`**: These are used to check properties on an object.
  - **Alternative**: Instead of calling these methods directly on an object, you can use their respective static alternatives, `Object.getOwnPropertyDescriptor()` and `Object.hasOwn()`.

### 6. **Override Danger**
If you define a method in your object with the same name as a method from `Object.prototype`, it may cause unexpected behavior. For example:
```javascript
const obj = {
  foo: 1,
  propertyIsEnumerable() {
    return false;
  },
};

console.log(obj.propertyIsEnumerable("foo")); // false; unexpected result
Object.prototype.propertyIsEnumerable.call(obj, "foo"); // true; expected result
```
In this example, the custom `propertyIsEnumerable` method on `obj` overrides the inherited `Object.prototype.propertyIsEnumerable()` method. Calling `propertyIsEnumerable("foo")` directly on the object produces an unexpected result, but using `Object.prototype.propertyIsEnumerable.call(obj, "foo")` gives the expected behavior.

### 7. **Deprecated Methods**
Some methods are deprecated and should be avoided:
- **`__defineGetter__()`**, **`__defineSetter__()`**, **`__lookupGetter__()`**, and **`__lookupSetter__()`**: These were used to define and retrieve getter/setter properties. Instead, you should use `Object.defineProperty()` and `Object.getOwnPropertyDescriptor()`.

### 8. **Static Alternatives**
Modern JavaScript utilities for working with objects are primarily static methods. Some common alternatives include:
- **`Object.defineProperty()`**: Adds or modifies a property on an object with specified descriptors.
- **`Object.getOwnPropertyDescriptor()`**: Returns the property descriptor for an object property.
- **`Object.getPrototypeOf()`**: Returns the prototype of an object.

### Summary
- **Use Object.prototype cautiously**: Modifying methods like `propertyIsEnumerable` or `hasOwnProperty` can have unintended consequences.
- **Use static methods**: Modern JavaScript encourages using static methods such as `Object.getPrototypeOf()` and `Object.defineProperty()` for better safety and clarity.
- **Understand inheritance**: The prototype chain allows object inheritance in JavaScript, enabling polymorphism but requiring care when modifying inherited behavior.

By understanding and utilizing these core concepts, you can work more effectively with JavaScript objects and avoid issues that arise from incorrectly modifying or calling prototype methods.

### In-Depth Notes on JavaScript: Deleting Properties, Null-Prototype Objects, and Their Use Cases

#### 1. **Deleting Properties from Objects**

In JavaScript, there is no built-in method like `Map.prototype.delete()` for deleting properties from regular objects. Instead, the `delete` operator is used to remove a property from an object.

- **Syntax**:
  ```javascript
  delete object.property;
  delete object['property'];
  ```

- **Example**:
  ```javascript
  const obj = { name: "John", age: 30 };
  console.log(obj); // { name: "John", age: 30 }
  
  delete obj.age; // deletes the 'age' property
  console.log(obj); // { name: "John" }
  ```

- **Note**: Using `delete` on an object property will remove that property, but it won't affect other properties or methods on the object. Additionally, the `delete` operator cannot remove non-configurable properties, which are properties defined with `Object.defineProperty()` and marked as non-configurable.

#### 2. **Null-Prototype Objects**

Most objects in JavaScript inherit from `Object.prototype`, which provides common methods like `toString()`, `hasOwnProperty()`, and others. However, you can create an object with a **null prototype**, meaning it does not inherit from `Object.prototype`. This can be useful in certain scenarios where you want to create a clean object without any inherited properties.

- **Creating a Null-Prototype Object**:
  - **Using `Object.create(null)`**:
    ```javascript
    const obj = Object.create(null);
    console.log(obj); // {}
    ```
  - **Using `__proto__: null`** (note the difference from the deprecated `__proto__`):
    ```javascript
    const obj2 = { __proto__: null };
    console.log(obj2); // {}
    ```

- **Behavior of Null-Prototype Objects**:
  An object with a null prototype will not have any of the standard methods or properties from `Object.prototype`, such as `toString()`, `hasOwnProperty()`, or `valueOf()`.

  **Example**:
  ```javascript
  const normalObj = {}; // create a normal object
  const nullProtoObj = Object.create(null); // create an object with "null" prototype

  console.log(`normalObj is: ${normalObj}`); // "normalObj is: [object Object]"
  console.log(`nullProtoObj is: ${nullProtoObj}`); // Error: Cannot convert object to primitive value
  ```

  **Important Points**:
  - **No inherited methods**: Methods like `valueOf()`, `hasOwnProperty()`, and `toString()` will not work on null-prototype objects, leading to errors.
  - **Debugging Difficulty**: When debugging objects with null prototypes, you might encounter issues where utility functions (like `console.log`) or object methods do not work as expected.

- **Workaround**:
  You can add the missing methods (e.g., `toString`) back manually:
  ```javascript
  nullProtoObj.toString = Object.prototype.toString;
  console.log(nullProtoObj.toString()); // "[object Object]"
  ```

#### 3. **Why Use Null-Prototype Objects?**

Null-prototype objects are often used in cases where you need a simple collection (like a map) without the overhead of inherited properties from `Object.prototype`. This can help avoid problems, especially in cases where object methods might conflict with the data stored within the object.

- **Avoiding Conflicts**:
  Objects that inherit from `Object.prototype` might have their methods overwritten by user-defined properties. For example, an object storing key-value pairs might inadvertently have a key named `hasOwnProperty`, leading to unexpected behavior:
  ```javascript
  const ages = { alice: 18, bob: 27 };
  
  function hasPerson(name) {
    return name in ages; // uses 'in' operator to check property existence
  }
  
  console.log(hasPerson("hasOwnProperty")); // true, because 'hasOwnProperty' is a property of Object.prototype
  ```

  Using a null-prototype object eliminates this risk:
  ```javascript
  const ages = Object.create(null);
  ages.alice = 18;
  ages.bob = 27;
  
  console.log(hasPerson("hasOwnProperty")); // false, because 'hasOwnProperty' is not a property of nullProtoObj
  ```

- **Prototype Pollution Attack Prevention**:
  Null-prototype objects prevent prototype pollution attacks. If an attacker were to add a property to `Object.prototype`, it would be accessible on every object in the program unless that object has a null prototype. This makes the system more secure.

  **Example of a prototype pollution attack**:
  ```javascript
  const user = {};
  
  // A malicious script:
  Object.prototype.authenticated = true;
  
  // Unexpected behavior:
  if (user.authenticated) { // This will always be true, leading to security issues
    // access confidential data
  }
  ```

  With null-prototype objects:
  ```javascript
  const user = Object.create(null);
  
  // No prototype inheritance, so 'authenticated' won't exist on user
  if (user.authenticated) { // This condition will never be true
    // access confidential data
  }
  ```

#### 4. **Common Use Cases for Null-Prototype Objects**

- **Maps**: Since null-prototype objects don't have the overhead of inherited methods, they are often used as a lightweight alternative to maps.
  
  Example:
  ```javascript
  const ages = Object.create(null);
  ages.alice = 18;
  ages.bob = 27;
  console.log(ages.alice); // 18
  ```

- **Ad-Hoc Key-Value Stores**: Null-prototype objects are often used for temporary data structures or ad-hoc key-value stores that do not require any inherited methods.

- **Preventing Prototype Pollution**: When you need to store user input or other data safely, null-prototype objects prevent issues with malicious additions to `Object.prototype`.

#### 5. **Reverting a Null-Prototype Object to a Regular Object**

If you need to turn a null-prototype object back into a regular object with `Object.prototype` inheritance, you can use `Object.setPrototypeOf()`:
```javascript
Object.setPrototypeOf(nullProtoObj, Object.prototype);
console.log(nullProtoObj.toString()); // "[object Object]"
```

#### Conclusion

- **Null-prototype objects** are a powerful tool for preventing inherited property conflicts, prototype pollution attacks, and other issues related to object methods.
- While useful in specific scenarios, **careful consideration is needed** when working with null-prototype objects, especially when adding new methods or properties to avoid conflicts with stored data.
- **Deleting properties** using the `delete` operator remains a simple and effective way to manage object properties.

# Object Coercsion:
### In-Depth Notes on Object Coercion in JavaScript

**Object Coercion** in JavaScript refers to the process where objects are automatically or explicitly converted to primitive values, typically in contexts where a primitive value is expected. This is crucial for understanding how JavaScript handles type conversion when performing operations involving objects and primitive types.

#### 1. **What is Coercion?**
Coercion is the automatic or implicit conversion of values from one type to another. JavaScript performs two types of coercion:
- **Implicit coercion**: JavaScript automatically converts types behind the scenes when required.
- **Explicit coercion**: You, as a developer, manually convert a value to another type using functions or operators.

#### 2. **How Object Coercion Works**
When an object is used in a context that requires a primitive value (such as comparison or concatenation), JavaScript coerces the object into a primitive value. This usually happens using two special methods:
- **`toString()`**: Converts an object to a string representation.
- **`valueOf()`**: Converts an object to its primitive value (usually used for numeric operations).

JavaScript will call these methods automatically when it needs to convert an object.

#### 3. **Coercion to Primitive Types**
JavaScript primarily coerces objects to two types of primitive values:
- **String**: When an object is used in string contexts, JavaScript tries to convert it to a string.
- **Number**: When an object is used in arithmetic contexts, JavaScript tries to convert it to a number.

#### 4. **`toString()` and `valueOf()` Methods**
- The `toString()` method is called when an object is involved in string concatenation or similar operations that require a string.
- The `valueOf()` method is called when an object is involved in numeric operations like addition or subtraction.

If both methods exist in the object, JavaScript will first check the `valueOf()` method. If `valueOf()` returns a non-primitive (i.e., an object), it will fallback to the `toString()` method.

#### 5. **Examples of Object Coercion**

##### Example 1: **Coercion to String**
When an object is used in a context where a string is expected (e.g., concatenation), JavaScript tries to convert the object to a string using the `toString()` method.

```javascript
const obj = { name: "Alice" };

console.log("Hello, " + obj);  // Output: "Hello, [object Object]"
```

In this case, the `toString()` method is implicitly called on the object, and since `toString()` for plain objects returns `[object Object]`, the result is `"Hello, [object Object]"`.

##### Example 2: **Coercion to Number**
When an object is used in a context where a number is expected (e.g., arithmetic operations), JavaScript tries to convert the object to a primitive number using the `valueOf()` method.

```javascript
const obj = {
  valueOf: function() {
    return 10;
  }
};

console.log(obj + 5);  // Output: 15
```

Here, `valueOf()` returns `10`, which is a primitive value, and the addition operation proceeds using that primitive number.

##### Example 3: **Coercion to String with `toString()` Method**
If the object has a `toString()` method explicitly defined, JavaScript will use that for string coercion.

```javascript
const obj = {
  toString: function() {
    return "Hello from object!";
  }
};

console.log("Message: " + obj);  // Output: "Message: Hello from object!"
```

The `toString()` method is explicitly called, converting the object into the string `"Hello from object!"`.

##### Example 4: **Fallback to `toString()` if `valueOf()` is an Object**
If the `valueOf()` method returns an object, JavaScript will then fall back to the `toString()` method for conversion.

```javascript
const obj = {
  valueOf: function() {
    return { name: "Bob" };  // Returns an object
  },
  toString: function() {
    return "Converted to string!";
  }
};

console.log(obj + 10);  // Output: "Converted to string!10"
```

In this case, `valueOf()` returns an object, so JavaScript calls `toString()` to get the string `"Converted to string!"`, and then concatenates the string with `10`.

#### 6. **Special Case: `NaN` and Objects in Numeric Contexts**
When objects are coerced to numbers and they cannot be converted to a meaningful numeric value, they are converted to `NaN` (Not-a-Number).

```javascript
const obj = {
  toString: function() {
    return "Not a number";
  }
};

console.log(obj * 2);  // Output: NaN
```

Here, since the `toString()` method returns a non-numeric string, the multiplication results in `NaN`.

#### 7. **Practical Implications of Object Coercion**
- **String concatenation**: Objects are automatically coerced to strings using `toString()`.
- **Arithmetic operations**: Objects are coerced to numbers using `valueOf()`.
- **Comparisons**: Objects are coerced to primitives for comparison, which can lead to unexpected behavior if objects do not have a proper `valueOf()` or `toString()` method.

#### 8. **Conclusion**
Object coercion in JavaScript is a powerful and sometimes confusing mechanism that allows objects to be converted into primitive values. By understanding how `toString()` and `valueOf()` work, and when they are called, you can gain more control over how objects behave in different contexts. It’s important to be mindful of the potential for unexpected results, especially when dealing with custom objects that don’t define these methods properly.

## Constructor
Here’s a note on how to create a constructor in JavaScript:

### Constructor in JavaScript

A constructor in JavaScript is a special function used to create and initialize objects. It’s typically used to set the initial values of an object's properties.

#### Syntax
```javascript
function ConstructorName() {
    // Initialize properties
    this.property1 = value1;
    this.property2 = value2;
}
```

#### Example
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const person1 = new Person("John", 30);
console.log(person1.name); // "John"
console.log(person1.age);  // 30
```

#### Key Points:
- **`this` keyword**: Inside the constructor, the `this` keyword refers to the new object being created.
- **`new` keyword**: When you use `new`, JavaScript creates a new empty object and links it to the constructor function, invoking the constructor to set the object's properties.
- **Constructor Functions**: Functions that initialize objects are called constructor functions. They are typically capitalized by convention to distinguish them from regular functions.

#### Using ES6 Class Syntax (Recommended):
```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

const person1 = new Person("Alice", 25);
console.log(person1.name); // "Alice"
console.log(person1.age);  // 25
```

### When to use a constructor:
- Use a constructor when you need to create multiple instances of an object with the same structure but different values.
- It's especially useful when creating classes in object-oriented programming (OOP).

This note should help you understand how constructors work in JavaScript. Let me know if you'd like more details!

## Static Methods:
### In-Depth Note on JavaScript Object Methods

Here’s an explanation of various `Object` methods and static methods in JavaScript with proper examples:

---

### **1. Static Methods**

Static methods are functions that belong to the class itself, not to instances of the class. They are called on the class itself, not objects created from it.

#### **Example:**
```javascript
class MyClass {
    static greet() {
        console.log("Hello, world!");
    }
}

MyClass.greet(); // Output: "Hello, world!"
```

---

### **2. Object.assign()**
- **Description**: Copies the values of all enumerable own properties from one or more source objects to a target object.
  
- **Syntax**:
```javascript
Object.assign(target, ...sources)
```

#### **Example**:
```javascript
const obj1 = { name: "Alice" };
const obj2 = { age: 25 };
const result = Object.assign({}, obj1, obj2);
console.log(result); // Output: { name: "Alice", age: 25 }
```

---

### **3. Object.create()**
- **Description**: Creates a new object with the specified prototype object and properties.
  
- **Syntax**:
```javascript
Object.create(proto, propertiesObject)
```

#### **Example**:
```javascript
const animal = { type: "Mammal" };
const dog = Object.create(animal);
dog.breed = "Labrador";
console.log(dog.type); // Output: "Mammal"
console.log(dog.breed); // Output: "Labrador"
```

---

### **4. Object.defineProperties()**
- **Description**: Adds the named properties described by the given descriptors to an object.
  
- **Syntax**:
```javascript
Object.defineProperties(obj, descriptors)
```

#### **Example**:
```javascript
const obj = {};
Object.defineProperties(obj, {
    name: { value: "Alice", writable: false },
    age: { value: 30, writable: true }
});
console.log(obj.name); // Output: "Alice"
```

---

### **5. Object.defineProperty()**
- **Description**: Adds a named property described by a given descriptor to an object.

- **Syntax**:
```javascript
Object.defineProperty(obj, prop, descriptor)
```

#### **Example**:
```javascript
const person = {};
Object.defineProperty(person, "age", {
    value: 25,
    writable: false
});
console.log(person.age); // Output: 25
person.age = 30;
console.log(person.age); // Output: 25 (value can't be changed)
```

---

### **6. Object.entries()**
- **Description**: Returns an array containing all of the `[key, value]` pairs of a given object's own enumerable string properties.
  
- **Syntax**:
```javascript
Object.entries(obj)
```

#### **Example**:
```javascript
const person = { name: "John", age: 30 };
console.log(Object.entries(person)); // Output: [ [ 'name', 'John' ], [ 'age', 30 ] ]
```

---

### **7. Object.freeze()**
- **Description**: Freezes an object, preventing other code from deleting or changing its properties.
  
- **Syntax**:
```javascript
Object.freeze(obj)
```

#### **Example**:
```javascript
const car = { make: "Toyota", model: "Corolla" };
Object.freeze(car);
car.model = "Camry"; // Changes won't take effect
console.log(car.model); // Output: "Corolla"
```

---

### **8. Object.fromEntries()**
- **Description**: Returns a new object from an iterable of `[key, value]` pairs (reverse of `Object.entries`).
  
- **Syntax**:
```javascript
Object.fromEntries(iterable)
```

#### **Example**:
```javascript
const entries = [["name", "Alice"], ["age", 25]];
const obj = Object.fromEntries(entries);
console.log(obj); // Output: { name: 'Alice', age: 25 }
```

---

### **9. Object.getOwnPropertyDescriptor()**
- **Description**: Returns a property descriptor for a named property on an object.
  
- **Syntax**:
```javascript
Object.getOwnPropertyDescriptor(obj, prop)
```

#### **Example**:
```javascript
const person = { name: "John" };
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// Output: { value: 'John', writable: true, enumerable: true, configurable: true }
```

---

### **10. Object.getOwnPropertyDescriptors()**
- **Description**: Returns an object containing all own property descriptors for an object.
  
- **Syntax**:
```javascript
Object.getOwnPropertyDescriptors(obj)
```

#### **Example**:
```javascript
const obj = { name: "Alice", age: 30 };
console.log(Object.getOwnPropertyDescriptors(obj));
// Output: 
// {
//     name: { value: 'Alice', writable: true, enumerable: true, configurable: true },
//     age: { value: 30, writable: true, enumerable: true, configurable: true }
// }
```

---

### **11. Object.getOwnPropertyNames()**
- **Description**: Returns an array containing the names of all of the given object's own enumerable and non-enumerable properties.
  
- **Syntax**:
```javascript
Object.getOwnPropertyNames(obj)
```

#### **Example**:
```javascript
const person = { name: "Alice" };
Object.defineProperty(person, "age", { value: 25, enumerable: false });
console.log(Object.getOwnPropertyNames(person)); // Output: [ 'name', 'age' ]
```

---

### **12. Object.getOwnPropertySymbols()**
- **Description**: Returns an array of all symbol properties found directly upon a given object.
  
- **Syntax**:
```javascript
Object.getOwnPropertySymbols(obj)
```

#### **Example**:
```javascript
const sym = Symbol("id");
const obj = { [sym]: 123 };
console.log(Object.getOwnPropertySymbols(obj)); // Output: [ Symbol(id) ]
```

---

### **13. Object.getPrototypeOf()**
- **Description**: Returns the prototype (internal `[[Prototype]]` property) of the specified object.
  
- **Syntax**:
```javascript
Object.getPrototypeOf(obj)
```

#### **Example**:
```javascript
const person = { name: "John" };
console.log(Object.getPrototypeOf(person)); // Output: {}
```

---

### **14. Object.groupBy()**
- **Description**: Groups elements of an iterable according to a provided callback function.
  
- **Syntax**:
```javascript
Object.groupBy(iterable, callback)
```

#### **Example**:
```javascript
const arr = [1.2, 2.4, 2.7, 3.5];
const result = Object.groupBy(arr, Math.floor);
console.log(result);
// Output: { '1': [ 1.2 ], '2': [ 2.4, 2.7 ], '3': [ 3.5 ] }
```

---

### **15. Object.hasOwn()**
- **Description**: Returns true if the specified object has the indicated property as its own property.
  
- **Syntax**:
```javascript
Object.hasOwn(obj, prop)
```

#### **Example**:
```javascript
const person = { name: "Alice" };
console.log(Object.hasOwn(person, "name")); // Output: true
console.log(Object.hasOwn(person, "age"));  // Output: false
```

---

### **16. Object.is()**
- **Description**: Compares if two values are the same value (similar to `===` but handles `NaN` differently).
  
- **Syntax**:
```javascript
Object.is(value1, value2)
```

#### **Example**:
```javascript
console.log(Object.is(25, 25)); // Output: true
console.log(Object.is(NaN, NaN)); // Output: true
```

---

### **17. Object.isExtensible()**
- **Description**: Determines if extending an object is allowed (i.e., if new properties can be added).
  
- **Syntax**:
```javascript
Object.isExtensible(obj)
```

#### **Example**:
```javascript
const person = {};
console.log(Object.isExtensible(person)); // Output: true
Object.preventExtensions(person);
console.log(Object.isExtensible(person)); // Output: false
```

---

### **18. Object.isFrozen()**
- **Description**: Determines if an object has been frozen.
  
- **Syntax**:
```javascript
Object.isFrozen(obj)
```

#### **Example**:
```javascript
const person = { name: "Alice" };
Object.freeze(person);
console.log(Object.isFrozen(person)); // Output: true
```

---

### **19. Object.isSealed()**
- **Description**: Determines if an object is sealed (i.e., its properties can't be deleted but can be modified).
  
- **Syntax**:
```javascript
Object.isSealed(obj)
```

#### **Example**:
```javascript
const person = { name: "Alice" };
Object.seal(person);
console.log(Object.isSealed(person)); // Output: true
```

---

### **20. Object.keys()**
- **Description**: Returns an array containing the names of all of the given object's own enumerable string properties.
  
- **Syntax**:
```javascript
Object.keys(obj)
```

#### **Example**:
```javascript
const person = { name: "Alice", age: 30 };
console.log(Object.keys(person)); // Output: ["name", "age"]
```

---

### **21. Object.preventExtensions()**
- **Description**: Prevents any extensions of an object, i.e., no new properties can be added to it.
  
- **Syntax**:
```javascript
Object.preventExtensions(obj)
```

#### **Example**:
```javascript
const obj = {};
Object.preventExtensions(obj);
obj.newProp = "Test";  // Won't be added
console.log(obj.newProp); // Output: undefined
```

---

### **22. Object.seal()**
- **Description**: Prevents other code from deleting properties of an object and marks all properties as non-configurable.
  
- **Syntax**:
```javascript
Object.seal(obj)
```

#### **Example**:
```javascript
const person = { name: "John" };
Object.seal(person);
delete person.name; // Won't work
console.log(person); // Output: { name: 'John' }
```

---

### **23. Object.setPrototypeOf()**
- **Description**: Sets the object's prototype (its internal `[[Prototype]]` property).
  
- **Syntax**:
```javascript
Object.setPrototypeOf(obj, prototype)
```

#### **Example**:
```javascript
const animal = { type: "Mammal" };
const dog = {};
Object.setPrototypeOf(dog, animal);
console.log(dog.type); // Output: "Mammal"
```

---

### **24. Object.values()**
- **Description**: Returns an array containing the values of all the given object's own enumerable string properties.
  
- **Syntax**:
```javascript
Object.values(obj)
```

#### **Example**:
```javascript
const person = { name: "Alice", age: 30 };
console.log(Object.values(person)); // Output: [ 'Alice', 30 ]
```

---

This covers all the listed methods with examples and their usages.

## Instance Method:
Here’s an in-depth note on **instance methods** with proper examples:

---

### **1. Object.prototype.__defineGetter__()** (Deprecated)

**Purpose**: Associates a function with a property, such that when the property is accessed, the function is executed and its return value is returned instead.

**Example**:
```javascript
let obj = {};
obj.__defineGetter__('name', function() {
  return 'Hello World';
});

console.log(obj.name);  // Output: "Hello World"
```

---

### **2. Object.prototype.__defineSetter__()** (Deprecated)

**Purpose**: Associates a function with a property, such that when the property is set, the function is executed to modify the property’s value.

**Example**:
```javascript
let obj = {};
obj.__defineSetter__('name', function(value) {
  this._name = value.toUpperCase();
});

obj.name = 'john';  // The setter modifies the value
console.log(obj._name);  // Output: "JOHN"
```

---

### **3. Object.prototype.__lookupGetter__()** (Deprecated)

**Purpose**: Returns the function bound as a getter to the specified property.

**Example**:
```javascript
let obj = {};
obj.__defineGetter__('name', function() {
  return 'Hello';
});

let getter = obj.__lookupGetter__('name');
console.log(getter());  // Output: "Hello"
```

---

### **4. Object.prototype.__lookupSetter__()** (Deprecated)

**Purpose**: Returns the function bound as a setter to the specified property.

**Example**:
```javascript
let obj = {};
obj.__defineSetter__('name', function(value) {
  this._name = value;
});

let setter = obj.__lookupSetter__('name');
setter('John');
console.log(obj._name);  // Output: "John"
```

---

### **5. Object.prototype.hasOwnProperty()**

**Purpose**: Returns a boolean indicating whether the object has the specified property as its own (not inherited) property.

**Example**:
```javascript
let obj = { name: 'John' };

console.log(obj.hasOwnProperty('name'));  // Output: true
console.log(obj.hasOwnProperty('age'));   // Output: false
```

---

### **6. Object.prototype.isPrototypeOf()**

**Purpose**: Returns a boolean indicating whether the object this method is called upon is part of the prototype chain of the specified object.

**Example**:
```javascript
function Person() {}
let person = new Person();
console.log(Person.prototype.isPrototypeOf(person));  // Output: true
```

---

### **7. Object.prototype.propertyIsEnumerable()**

**Purpose**: Returns a boolean indicating whether the specified property is the object's enumerable own property.

**Example**:
```javascript
let obj = { name: 'John' };
console.log(obj.propertyIsEnumerable('name'));  // Output: true

Object.defineProperty(obj, 'age', {
  value: 25,
  enumerable: false
});

console.log(obj.propertyIsEnumerable('age'));   // Output: false
```

---

### **8. Object.prototype.toLocaleString()**

**Purpose**: Calls the `toString()` method, but may return a localized string depending on the environment and locale.

**Example**:
```javascript
let obj = {
  value: 12345.6789,
  toLocaleString() {
    return this.value.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
  }
};

console.log(obj.toLocaleString());  // Output: "$12,345.68"
```

---

### **9. Object.prototype.toString()**

**Purpose**: Returns a string representation of the object. If not overwritten, it returns a string indicating the type of object.

**Example**:
```javascript
let obj = { name: 'John' };
console.log(obj.toString());  // Output: "[object Object]"

let arr = [1, 2, 3];
console.log(arr.toString());  // Output: "1,2,3"
```

---

### **10. Object.prototype.valueOf()**

**Purpose**: Returns the primitive value of the object.

**Example**:
```javascript
let obj = { name: 'John', valueOf() { return 42; } };
console.log(obj.valueOf());  // Output: 42

let numObj = new Number(100);
console.log(numObj.valueOf());  // Output: 100
```

---

These are the instance methods of `Object.prototype` with detailed explanations and examples. The deprecated methods (`__defineGetter__`, `__defineSetter__`, `__lookupGetter__`, and `__lookupSetter__`) should generally be avoided in modern JavaScript, as they are no longer recommended for use.

## Example
Here’s an in-depth note on **constructing empty objects**, **using `Object()` constructor to turn primitives into objects**, and **modifying Object prototypes** with examples:

---

### **1. Constructing Empty Objects**

You can create empty objects using the `Object()` constructor with different arguments.

- **Example 1**: Creating an empty object using `new Object()`
  ```javascript
  const o1 = new Object();
  console.log(o1);  // Output: {}
  ```

- **Example 2**: Creating an empty object with `undefined`
  ```javascript
  const o2 = new Object(undefined);
  console.log(o2);  // Output: {}  (undefined does not affect the object creation)
  ```

- **Example 3**: Creating an empty object with `null`
  ```javascript
  const o3 = new Object(null);
  console.log(o3);  // Output: {}  (null does not affect the object creation)
  ```

In each case, an empty object is created. The value of the argument (`undefined` or `null`) does not affect the result because these values are not used to initialize any properties in the object.

---

### **2. Using Object() Constructor to Turn Primitives into Objects**

The `Object()` constructor can also be used to convert primitive values (like boolean, number, string) into their respective object wrappers. This is particularly useful for dealing with the primitive types as objects (e.g., `Boolean`, `Number`, and `String`).

- **Example 1**: Creating a `Boolean` object
  ```javascript
  const o1 = new Object(true);
  console.log(o1);  // Output: Boolean { true }
  console.log(o1 instanceof Boolean);  // Output: true
  ```

  - The primitive `true` is wrapped inside a `Boolean` object.

- **Example 2**: Creating a `BigInt` object
  ```javascript
  const o2 = new Object(1n);
  console.log(o2);  // Output: 1n (BigInt is treated as a primitive in this case)
  console.log(o2 instanceof BigInt);  // Output: false (BigInt cannot be an object directly)
  ```

  - Note that `BigInt` cannot be used with `new` as a constructor to create an object. The object wrapper for `BigInt` does not exist. It remains a primitive type.

---

### **3. Modifying Object Prototypes**

Modifying the prototype property of built-in constructors (such as `Object.prototype`, `Function.prototype`, `Node.prototype`) allows altering the behavior of existing methods. However, **modifying prototypes of built-in objects** is generally considered **bad practice** because it risks breaking compatibility with future versions of JavaScript.

#### **Example 1: Modifying `Object.prototype.valueOf`**

In this example, we modify the `valueOf` method to check for a custom property `-prop-value`. If the property exists, we return its value; otherwise, we call the default behavior of `valueOf`.

```javascript
// Save current behavior of valueOf method
const current = Object.prototype.valueOf;

// Modify Object.prototype.valueOf
Object.prototype.valueOf = function (...args) {
  if (Object.hasOwn(this, "-prop-value")) {
    return this["-prop-value"];  // Return custom property value
  } else {
    // Fall back to the default behavior
    return current.apply(this, args);
  }
};

// Example usage:
const obj1 = { "-prop-value": 42 };
console.log(obj1.valueOf());  // Output: 42  (Custom property)

const obj2 = {};
console.log(obj2.valueOf());  // Output: {}   (Default valueOf behavior)
```

### **Explanation**:

- **Step 1**: We save the current `valueOf` method to a variable `current`.
- **Step 2**: We override `Object.prototype.valueOf` with a new function. The function checks if the object has a custom `-prop-value` property.
  - If it does, it returns that value.
  - If not, it falls back to the default behavior using `apply()`, calling the original `valueOf` method.

---

### **Warning About Modifying Prototypes**:
Modifying the prototype of built-in constructors like `Object.prototype` can cause conflicts with other code or future updates to JavaScript. **It's considered bad practice** because:

- It can lead to unexpected behavior in other parts of the code.
- It may break compatibility with future JavaScript versions.
- The changes might affect all objects globally, leading to bugs that are hard to track down.

If you need to add custom functionality, consider **creating your own functions** or using **composition** instead of altering built-in prototypes.

---

### **Conclusion**:

1. The `Object()` constructor can create empty objects or wrap primitive values as their object counterparts.
2. Modifying built-in prototypes like `Object.prototype` is powerful but should be done cautiously. It's best to use this pattern with caution and ensure that you aren't introducing unintended side effects.