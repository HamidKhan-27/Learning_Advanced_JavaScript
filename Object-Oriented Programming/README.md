# JavaScript — Topic 14: Object-Oriented Programming (OOP)

## Topics Covered

1. Objects, Properties & Methods
2. `this` Keyword
3. Classes & Constructors
4. The `new` Keyword
5. Constructor Functions
6. Prototypes & Prototype Chain
7. Inheritance (`extends`)
8. `super()` Keyword
9. Method Overriding & Polymorphism
10. Encapsulation & Private Fields
11. Getters & Setters
12. Static Methods
13. `instanceof`
14. Composition
15. Practical Bank Account Example

---

## 1. Objects, Properties & Methods

An object stores related data and functionality together.

```js
const person = {
  name: "Ali",
  age: 22,

  greet() {
    console.log("Hello " + this.name);
  },
};

person.greet(); // Hello Ali
```

- **Properties:** Data stored inside an object, such as `name` and `age`.
- **Methods:** Functions stored inside an object, such as `greet()`.

## 2. The `this` Keyword

`this` refers to the object associated with the current method call.

```js
const user = {
  name: "Hamid",

  showName() {
    console.log(this.name);
  },
};

user.showName(); // Hamid
```

Here, `this` refers to `user`.

**Important:** In a normal object method, `this` generally refers to the object used to call that method. Arrow functions do not create their own `this`; they inherit it from their surrounding scope.

## 3. Classes & Constructors

A class is a reusable blueprint for creating objects.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, ${this.name}`);
  }
}

const person1 = new Person("Ali", 22);
const person2 = new Person("Sara", 25);

person1.greet(); // Hello, Ali
person2.greet(); // Hello, Sara
```

- `class` defines a class.
- `constructor()` runs when a new object is created.
- `this.name` and `this.age` create instance properties.
- `new` creates an object using the class.

## 4. How Does `new` Work?

When we write:

```js
const person = new Person("Ali", 22);
```

JavaScript approximately performs these steps:

1. Creates a new empty object.
2. Links that object's prototype to `Person.prototype`.
3. Calls the constructor with `this` referring to the new object.
4. Returns the new object (unless the constructor explicitly returns another object).

This is why every object created with `new Person()` gets its own instance properties.

## 5. Constructor Functions

Before ES6 classes, constructor functions were commonly used to create objects.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function () {
  console.log(`Hello, ${this.name}`);
};

const person1 = new Person("Ali", 22);

person1.greet(); // Hello, Ali
```

The `new` keyword is important. Without it, a constructor function does not create an instance in the normal way.

## 6. Prototypes

JavaScript uses prototypes to share methods between objects.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return "Hello " + this.name;
  }
}

const person1 = new Person("Ali");
const person2 = new Person("Sara");

console.log(person1.greet()); // Hello Ali
console.log(person2.greet()); // Hello Sara
```

The `greet()` method is stored on `Person.prototype`, rather than being separately created for every instance.

### Prototype Chain

When JavaScript cannot find a property directly on an object, it checks that object's prototype, then continues up the prototype chain.

```js
const animal = {
  eat() {
    console.log("Eating");
  },
};

const dog = Object.create(animal);

dog.bark = function () {
  console.log("Barking");
};

dog.bark(); // Barking
dog.eat(); // Eating
```

`dog` does not own the `eat()` method, but it can access it through its prototype.

## 7. Inheritance — `extends`

Inheritance allows one class to use functionality from another class.

```js
class Animal {
  eat() {
    console.log("Animal is eating");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Dog is barking");
  }
}

const dog = new Dog();

dog.eat(); // Animal is eating
dog.bark(); // Dog is barking
```

- `Animal` is the parent class.
- `Dog` is the child class.
- `extends` establishes inheritance.

## 8. The `super()` Keyword

`super()` calls the parent class constructor from a child class constructor.

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

class Student extends Person {
  constructor(name, course) {
    super(name);
    this.course = course;
  }
}

const student = new Student("Ali", "JavaScript");

console.log(student.name); // Ali
console.log(student.course); // JavaScript
```

**Important:** In a derived class constructor, you must call `super()` before accessing `this`.

### Calling a Parent Method

`super.methodName()` calls a method from the parent class.

```js
class Animal {
  speak() {
    return "Animal";
  }
}

class Dog extends Animal {
  speak() {
    return super.speak() + " Dog";
  }
}

const dog = new Dog();

console.log(dog.speak()); // Animal Dog
```

## 9. Method Overriding & Polymorphism

### Method Overriding

When a child class defines a method with the same name as a parent method, it overrides that method.

```js
class Animal {
  speak() {
    return "Animal sound";
  }
}

class Dog extends Animal {
  speak() {
    return "Bark";
  }
}

const dog = new Dog();

console.log(dog.speak()); // Bark
```

### Polymorphism

Polymorphism means different objects can respond to the same method call in different ways.

```js
class Dog {
  speak() {
    return "Bark";
  }
}

class Cat {
  speak() {
    return "Meow";
  }
}

const animals = [new Dog(), new Cat()];

animals.forEach((animal) => {
  console.log(animal.speak());
});

// Bark
// Meow
```

Both objects use `speak()`, but each produces a different result.

## 10. Encapsulation & Private Fields

Encapsulation means keeping internal data protected and controlling how it is accessed or modified.

JavaScript supports private fields using `#`.

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
    }
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();

account.deposit(500);

console.log(account.getBalance()); // 500
// console.log(account.#balance);  // SyntaxError
```

**Important:**

- `#balance` is private.
- It cannot be accessed directly outside the class.
- Public methods can provide controlled access to private data.
- A private field must be declared inside the class body.

## 11. Getters & Setters

Getters and setters allow us to access and update properties using property syntax.

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    this._name = value.toUpperCase();
  }
}

const user = new User("Ali");

console.log(user.name); // Ali

user.name = "Sara";

console.log(user.name); // SARA
```

- `get` reads a value.
- `set` updates a value.
- We use `user.name`, not `user.name()`.

The underscore in `_name` is only a naming convention. It does not make the property truly private.

## 12. Static Methods

Static methods belong to the class itself, not to individual instances.

```js
class Calculator {
  static add(a, b) {
    return a + b;
  }
}

console.log(Calculator.add(2, 3)); // 5

const calculator = new Calculator();

// calculator.add(2, 3); // TypeError
```

**Important:** Call a static method using the class name.

## 13. `instanceof` Operator

The `instanceof` operator checks whether an object's prototype chain contains a particular class's prototype.

```js
class Animal {}

class Dog extends Animal {}

const dog = new Dog();

console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true
```

A `Dog` instance is also considered an `Animal` instance because `Dog` extends `Animal`.

## 14. Composition

Composition means building functionality by combining objects or smaller components instead of relying only on inheritance.

```js
const canWalk = {
  walk() {
    console.log("Walking");
  },
};

const canEat = {
  eat() {
    console.log("Eating");
  },
};

const person = Object.assign({}, canWalk, canEat);

person.walk(); // Walking
person.eat(); // Eating
```

Composition helps reuse functionality without creating a large inheritance hierarchy.

---

## 15. Practical Project — Bank Account

This example combines classes, constructors, private fields, getters, methods, validation and static methods.

```js
class BankAccount {
  #balance = 0;

  constructor(owner) {
    this.owner = owner;
  }

  deposit(amount) {
    if (amount <= 0) {
      return "Enter a valid amount";
    }

    this.#balance += amount;

    return `${amount} deposited successfully`;
  }

  withdraw(amount) {
    if (amount <= 0) {
      return "Enter a valid amount";
    }

    if (amount > this.#balance) {
      return "Insufficient balance";
    }

    this.#balance -= amount;

    return `${amount} withdrawn successfully`;
  }

  get balance() {
    return this.#balance;
  }

  static bankName() {
    return "My JavaScript Bank";
  }
}

const account = new BankAccount("Hamid");

console.log(account.deposit(5000));
console.log(account.withdraw(1000));

console.log(account.owner); // Hamid
console.log(account.balance); // 4000
console.log(BankAccount.bankName()); // My JavaScript Bank
```

### What This Project Demonstrates

- `class BankAccount` creates a reusable blueprint.
- `constructor()` initializes the account owner.
- `#balance` protects the account balance.
- `deposit()` adds money after validation.
- `withdraw()` checks the amount and available balance.
- `get balance()` provides controlled read access.
- `static bankName()` belongs to the class.

---

## Important OOP Revision Points

| Concept          | Main Purpose                                   |
| ---------------- | ---------------------------------------------- |
| Class            | Blueprint for creating objects                 |
| Constructor      | Initializes a new object                       |
| `this`           | Refers to the current method's calling context |
| `new`            | Creates an instance                            |
| Prototype        | Shares methods between objects                 |
| `extends`        | Establishes inheritance                        |
| `super()`        | Calls the parent constructor                   |
| `super.method()` | Calls a parent method                          |
| Overriding       | Child replaces a parent method                 |
| Polymorphism     | Same method call, different behavior           |
| `#private`       | Protects internal class data                   |
| Getter           | Reads a property                               |
| Setter           | Controls property updates                      |
| Static           | Method belongs to the class                    |
| `instanceof`     | Checks the prototype-chain relationship        |
| Composition      | Combines reusable functionality                |

## Final Key Differences

1. **Inheritance vs Composition:** Inheritance uses `extends`; composition combines functionality from separate objects or components.
2. **Method vs Static Method:** Instance methods are normally called on objects; static methods are called on the class.
3. **Getter vs Method:** A getter is accessed like a property; a normal method is called with parentheses.
4. **Public vs Private:** Public members can be accessed externally; private fields using `#` cannot.
5. **Overriding vs Polymorphism:** Overriding replaces an inherited method; polymorphism describes different objects responding differently to the same interface.

**Topic #14 Complete — Object-Oriented Programming (OOP)**

Basic Test: 5/5  
Hard Test: 8/8

Next Topic: **#15 — Asynchronous JavaScript**
