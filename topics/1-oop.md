# 1. OOP

## What is OOP

Object-Oriented Programming organizes code around **objects** — bundles of data (attributes/state) and behavior (methods) — instead of a sequence of functions acting on separate data (procedural style). Goal: model real-world entities directly, keep related data and logic together, and make code modular, reusable, and easier to reason about as it grows.

**Why not just a struct?** A struct/record can hold multiple fields of different types (`int`, `string`, `double`, ...), but it **cannot define functions on itself**. E.g. in a chess struct you can't give a knight piece its own `move()` — the behavior has to live somewhere else, disconnected from the data. OOP fixes this by letting a class own both the data and the functions that act on it.

## Class vs Object

- **Class** — blueprint. Defines what data and methods instances will have. Costs no memory until instantiated.
- **Object** — an instance of a class. Real entity in memory, with its own copy of the state, that can call the class's methods.

Example: `Car` is a class; `bmw`, `audi` are objects (instances) of it.

```ts
class Car {
  name: string;
  constructor(name: string) { this.name = name; }
}
const bmw = new Car("BMW");
const audi = new Car("Audi");
console.log(bmw.name, audi.name);
// Output: BMW Audi
```

```java
class Car {
    String name;
    Car(String name) { this.name = name; }
}
Car bmw = new Car("BMW");
Car audi = new Car("Audi");
System.out.println(bmw.name + " " + audi.name);
// Output: BMW Audi
```

```csharp
class Car {
    public string Name;
    public Car(string name) { Name = name; }
}
var bmw = new Car("BMW");
var audi = new Car("Audi");
Console.WriteLine($"{bmw.Name} {audi.Name}");
// Output: BMW Audi
```

## Four pillars

### Encapsulation
Bundling data + the methods that operate on it into one unit, and hiding internal state behind a controlled interface (getters/setters) instead of exposing raw fields. Real-world analogy: a capsule/pill — contents sealed inside, you interact with the outer shell only. Purpose: protect invariants, control how state changes.

```ts
class Employee {
  #salary: number; // private field
  constructor(salary: number) { this.#salary = salary; }
  getSalary(): number { return this.#salary; }
  setSalary(s: number): void { if (s > 0) this.#salary = s; }
}
const emp = new Employee(50000);
emp.setSalary(60000);
console.log(emp.getSalary());
// Output: 60000
```

```java
class Employee {
    private double salary;
    Employee(double salary) { this.salary = salary; }
    public double getSalary() { return salary; }
    public void setSalary(double s) { if (s > 0) salary = s; }
}
Employee emp = new Employee(50000);
emp.setSalary(60000);
System.out.println(emp.getSalary());
// Output: 60000.0
```

```csharp
class Employee {
    private double salary;
    public Employee(double salary) { this.salary = salary; }
    public double GetSalary() => salary;
    public void SetSalary(double s) { if (s > 0) salary = s; }
}
var emp = new Employee(50000);
emp.SetSalary(60000);
Console.WriteLine(emp.GetSalary());
// Output: 60000
```

Getter/setter naming: **getting methods** retrieve information (`getSalary()`), **setting methods** change it (`setSalary()`). Two things fall out of this:
- **Read-only attribute** — define a getter but skip the setter, and the attribute can only be read from outside, never changed.
- **Keeping dependent attributes in sync** — a setter can update more than one field at once (e.g. `setSalary()` also recalculating `tax`), which raw field access could never do consistently.

**Information hiding** — the broader reason encapsulation matters: don't let external classes reach in and edit an object's attributes directly, especially in large programs. Each piece of code should work through methods, not rely on another class's internals — that's what keeps a big codebase from turning into a tangle where every change breaks something unrelated.

### Abstraction
Hiding implementation details, exposing only the essential operations. Real-world analogy: an ATM or coffee machine — you press a button, you don't see the internal circuitry. Achieved via abstract classes / interfaces.

```ts
abstract class PaymentMethod {
  abstract pay(amount: number): void;
}
class CardPayment extends PaymentMethod {
  pay(amount: number): void { console.log(`Charged $${amount} to card`); }
}
new CardPayment().pay(50);
// Output: Charged $50 to card
```

```java
abstract class PaymentMethod {
    abstract void pay(double amount);
}
class CardPayment extends PaymentMethod {
    void pay(double amount) { System.out.println("Charged " + amount + " to card"); }
}
new CardPayment().pay(50);
// Output: Charged 50.0 to card
```

```csharp
abstract class PaymentMethod {
    public abstract void Pay(double amount);
}
class CardPayment : PaymentMethod {
    public override void Pay(double amount) => Console.WriteLine($"Charged {amount} to card");
}
new CardPayment().Pay(50);
// Output: Charged 50 to card
```

**Interface vs implementation** — the *interface* is how classes talk to each other (the methods each one exposes); the *implementation* is how those methods are actually coded, and it should stay hidden. A chess `King` and `Knight` have completely different internal move logic, but both expose a `move()` method — same interface shape, different implementation underneath.

Why it matters: if classes reach into each other's internals directly, they get entangled — one change ripples outward and breaks things far away. An interface is a fixed point of contact between classes, so each side can be developed and changed independently as long as the interface itself doesn't change.

### Inheritance
One class (child/derived, a.k.a. **subclass**) acquires the properties and methods of another (parent/base, a.k.a. **superclass**) — an "is-a" relationship. Enables code reuse and method overriding. Real-world analogy: `Animal` is the base class; `Dog`, `Cat`, `Cow` are derived classes that inherit shared behavior and override what differs.

A hierarchy isn't limited to one level — it's common to see several layers stacked (e.g. `Item` → `Weapon`/`Tool` → `Sword`/`Club` → more specific subtypes), forming a whole web of superclass/subclass relationships, not just a single parent-child pair.

```ts
class Animal {
  speak(): string { return "..."; }
}
class Dog extends Animal {
  speak(): string { return "Bark"; }
}
console.log(new Dog().speak());
// Output: Bark
```

```java
class Animal {
    String speak() { return "..."; }
}
class Dog extends Animal {
    String speak() { return "Bark"; }
}
System.out.println(new Dog().speak());
// Output: Bark
```

```csharp
class Animal {
    public virtual string Speak() => "...";
}
class Dog : Animal {
    public override string Speak() => "Bark";
}
Console.WriteLine(new Dog().Speak());
// Output: Bark
```

**Access modifiers** — control which classes can reach a given member:

| Modifier | Accessible from |
|---|---|
| Public | anywhere in the program |
| Protected | the defining class + its subclasses |
| Private | only the defining class itself |

Example on a `Food` → `Fruit`/`Vegetables` → `Apple`/`Orange`/`Broccoli` hierarchy: a **public** member on `Food` is visible everywhere, including `Apple`. A **private** member on `Fruit` is visible only inside `Fruit` — not from `Food`, not from `Apple`. A **protected** member on `Fruit` is visible from `Fruit` and from `Apple`/`Orange` (its subclasses), just not from unrelated branches like `Broccoli`.

### Polymorphism
"Many forms" — the same method/interface produces different behavior depending on the object invoking it. Two flavors: compile-time (overloading) and runtime (overriding). Real-world analogy: calling `speak()` on different animals — `Dog` barks, `Cat` meows, `Cow` moos — same call, different result per object.

```ts
const animals: Animal[] = [new Dog(), new Cat(), new Cow()];
animals.forEach(a => console.log(a.speak()));
// Output:
// Bark
// Meow
// Moo
// — resolved per-object at runtime
```

```java
Animal[] animals = { new Dog(), new Cat(), new Cow() };
for (Animal a : animals) System.out.println(a.speak());
// Output:
// Bark
// Meow
// Moo
// — dynamic dispatch
```

```csharp
Animal[] animals = { new Dog(), new Cat(), new Cow() };
foreach (var a in animals) Console.WriteLine(a.Speak());
// Output:
// Bark
// Meow
// Moo
// — dynamic dispatch
```

(Same idea, classic car flavor: `Car.drive()` and a `sportsCar extends Car` that overrides `drive()` — calling `.drive()` on a `Car` instance runs `Car`'s version, calling it on a `sportsCar` instance runs the override. Which implementation runs is decided by the object's actual type at runtime, not by the variable's declared type.)

**Static polymorphism (overloading)** — multiple methods, same name, same class, different signatures. Three ways a signature can differ:
- number of parameters
- types of parameters
- order of parameters

TS/JS have no real runtime overloading — only one function body ever exists, so "overloads" are just declared signatures dispatching to shared code via `typeof`/argument checks. C# and Java compile each overload to a genuinely separate method, picked at compile time by the argument types.

```ts
class Car {
  drive(spd: number, dest: string): void;
  drive(spd: number, dist: number): void;
  drive(spd: number, arg: string | number): void {
    console.log(typeof arg === "string" ? `Driving to ${arg} at ${spd}` : `Driving ${arg}km at ${spd}`);
  }
}
new Car().drive(30, "School");
// Output: Driving to School at 30
```

```java
class Car {
    void drive(int spd, String dest) { System.out.println("Driving to " + dest + " at " + spd); }
    void drive(int spd, int dist) { System.out.println("Driving " + dist + "km at " + spd); }
}
Car myCar = new Car();
myCar.drive(30, "School"); // compiler picks drive(int, String) based on argument types
// Output: Driving to School at 30
```

```csharp
class Car {
    public void Drive(int spd, string dest) => Console.WriteLine($"Driving to {dest} at {spd}");
    public void Drive(int spd, int dist) => Console.WriteLine($"Driving {dist}km at {spd}");
}
var myCar = new Car();
myCar.Drive(30, "School"); // compiler picks Drive(int, string) based on argument types
// Output: Driving to School at 30
```
Pitfall: if two overloads' parameter lists are ambiguous or too similar, passing the wrong argument can silently match a *different* overload instead of erroring — always double check which signature you're actually calling.

**Overloading vs overriding**

| | Overloading | Overriding |
|---|---|---|
| Binding | Compile-time (static) | Runtime (dynamic) |
| Signature | Must differ | Must be identical |
| Scope | Same class | Parent → child |
| Return type | Can differ | Must be same or covariant |

**Abstract class vs interface**
- Abstract class: can have state, constructors, implemented methods. Single inheritance (in Java/C#).
- Interface: contract only (pre-Java 8). Multiple implementation allowed.
- Rule of thumb: abstract class = "is-a" with shared code; interface = "can-do" capability.

**Other terms**
- **Static** members belong to the class, not the instance. No `this`.
- **Composition over inheritance** — favour has-a; inheritance couples subclass to parent internals.
- **Coupling** low = good. **Cohesion** high = good.
- **Association / Aggregation / Composition** — plain link / has-a with independent lifetime / has-a with dependent lifetime (delete parent → delete child).
- **Method hiding** — a static method redeclared in a subclass hides, does not override.
- **Constructor chaining** — child constructor implicitly calls parent's no-arg constructor first.
- **Shallow copy** copies references; **deep copy** copies the objects too.

**SOLID**
- **S**ingle Responsibility — one reason to change.
- **O**pen/Closed — open for extension, closed for modification.
- **L**iskov Substitution — subtype must be usable wherever base type is.
- **I**nterface Segregation — many small interfaces > one fat one.
- **D**ependency Inversion — depend on abstractions, not concretions.

## Advantages / Limitations of OOP

**Advantages**
- **Modularity** — program splits into independent, self-contained classes.
- **Easy maintenance** — changes stay local, minimize ripple effects.
- **Improved readability** — well-structured classes are easier to follow.
- **Scalability** — new features added without rewriting existing structure.
- **Faster development / reusability** — inheritance and composition reuse existing code.
- **Better testing** — classes can be tested in isolation.
- **Real-world modeling** — objects map intuitively to real entities.

**Limitations**
- **Steeper learning curve** — pillars + patterns take time to internalize.
- **Overhead for simple programs** — unnecessary structure for small scripts.
- **Design complexity** — class hierarchies need careful upfront planning.
- **Higher memory use** — many objects cost more than plain data structures/functions.

## Further Practice

- [Sanfoundry — 1000 OOP MCQ Questions & Answers](https://www.sanfoundry.com/1000-object-oriented-programming-oops-questions-answers/)
