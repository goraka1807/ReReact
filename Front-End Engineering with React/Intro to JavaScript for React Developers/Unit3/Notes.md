# JavaScript Classes, Getters and Setters

A **class** is like a blueprint for creating objects that have similar **properties and behaviors**.

For example, if we need multiple dogs, instead of creating each dog object manually, we can create a `Dog` class and use it to create many dogs.

---

## Creating a Class

### Basic Syntax

```js
class Dog {
    constructor(name) {
        this.name = name;
    }
}
```

The `constructor()` is automatically called when we create a new object from the class.

```js
let dog1 = new Dog("Spot");
let dog2 = new Dog("Buddy");

console.log(dog1.name); // Spot
console.log(dog2.name); // Buddy
```

### Important Terms

- `class` → creates the blueprint
- `constructor()` → sets up the object
- `new` → creates a new object from the class
- `this` → refers to the **current object/instance**

For example:

```js
this.name = name;
```

means:

> "Set the `name` property of this particular object."

---

# Class Methods

Methods are functions that belong to the class and define what the objects **can do**.

```js
class Dog {
    constructor(name) {
        this.name = name;
    }

    bark() {
        return `${this.name} says woof!`;
    }
}

let myDog = new Dog("Spot");

console.log(myDog.bark());
// Spot says woof!
```

We call the method using:

```js
myDog.bark();
```

### Useful Class Method Pattern

```js
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, ${this.name}`;
    }

    rename(newName) {
        this.name = newName;
    }
}

let user = new User("John");

console.log(user.greet()); // Hello, John

user.rename("Mike");

console.log(user.greet()); // Hello, Mike
```

### Remember

Inside a class:

```js
this.name
```

refers to the property belonging to the **current object**.

---

# Getters

***Getter*** is just like getting something

**GET -> READ**

A **getter** lets us access a method like it is a normal property.

We use the `get` keyword.

### Syntax

```js
get propertyName() {
    return this._propertyName;
}
```

Example:

```JS
class Dog {
    constructor(name) {
        this._name = name;
    }

    get name() {
        return this._name;
    }
}

let myDog = new Dog("Spot");

console.log(myDog.name); // Spot
```

Notice that we ** use parentheses**:

```
myDog.name
```

and not:

```
myDog.name()
```

That's because a getter behaves like a property.

---

# Setters

Setter is setting something.

**SETTER -> UPDATE**

A **setter** lets us control what happens when a property is changed.

We use the `set` keyword.

### Syntax

```JS
set propertyName(value) {
    // validate or modify value
}
```

Example:

```JS
class Dog {
    constructor(name) {
        this._name = name;
    }

    get name() {
        return this._name;
    }

    set name(value) {
        if (value.length > 0) {
            this._name = value;
        }
    }
}

let myDog = new Dog("Spot");

console.log(myDog.name); // Spot

myDog.name = "Buddy";

console.log(myDog.name); // Buddy
```

The setter automatically runs when we do:

```
myDog.name = "Buddy";
```

---

## Why Use Getters and Setters?

The main benefit is that we can **control access to our properties**.

For example, we don't want an empty name:

```JS
class Dog {
    constructor(name) {
        this._name = name;
    }

    get name() {
        return this._name;
    }

    set name(value) {
        if (value.trim() !== "") {
            this._name = value;
        } else {
            console.log("Name cannot be empty");
        }
    }
}

let dog = new Dog("Spot");

dog.name = "";

console.log(dog.name); // Spot
```

The setter prevents the invalid value from being stored.

---

# Why `_name`?

You will often see:

```
this._name = name;
```

The `_` is a **naming convention** that usually means:

> "This is an internal property. Access it through the getter/setter."

It does **not** make the property truly private.

```
this._name
```

is still accessible from outside the class.

Modern JavaScript also has actual private fields using `#`, which is a separate topic.

---

# Getter vs Setter

|Getter|Setter|
|---|---|
|Uses `get`|Uses `set`|
|Used when **reading** a property|Used when **changing** a property|
|Returns a value|Receives a value|
|`dog.name`|`dog.name = "Buddy"`|
|Can control how a value is read|Can validate/change a value before storing|

### Easy Way to Remember

```
dog.name
```

→ **getter**

```
dog.name = "Buddy"
```

→ **setter**

---

# A Useful Class Example

Putting everything together:

```JS
class User {
    constructor(name, age) {
        this._name = name;
        this.age = age;
    }

    get name() {
        return this._name;
    }

    set name(value) {
        if (value.trim() !== "") {
            this._name = value;
        }
    }

    greet() {
        return `Hello, ${this.name}!`;
    }

    isAdult() {
        return this.age >= 18;
    }
}

let user = new User("John", 25);

console.log(user.name);      // John
console.log(user.greet());   // Hello, John!
console.log(user.isAdult()); // true

user.name = "Mike";

console.log(user.name);      // Mike
```

Here:

- `constructor()` → initializes the object
- `get name()` → controls reading `name`
- `set name()` → controls changing `name`
- `greet()` → class behavior
- `isAdult()` → another useful behavior

---

# Common Mistakes

### 1. Calling a getter like a function

❌ Wrong:

```
console.log(user.name());
```

✅ Correct:

```
console.log(user.name);
```

---

### 2. Forgetting `new`

❌ Wrong:

```
let user = User("John");
```

✅ Correct:

```
let user = new User("John");
```

---

### 3. Confusing the parameter with `this`

```
constructor(name) {
    this.name = name;
}
```

Here:

- `name` → value passed into the constructor
- `this.name` → property belonging to the current object

For example:

```
new User("John");
```

means:

```
name      → "John"
this.name → "John"
```

---

# Quick Revision

1. A **class** is a blueprint for creating objects.
2. `constructor()` runs automatically when using `new`.
3. `new` creates an instance of a class.
4. `this` refers to the current object.
5. **Methods** define what an object can do.
6. A **getter** controls how a property is read.
7. A **setter** controls what happens when a property is changed.
8. Getters are used like properties: `user.name`.
9. Setters are triggered by assignments: `user.name = "Mike"`.
10. `_name` is commonly used for an internal property behind a getter/setter.