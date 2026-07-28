# 1. OOP

## What is OOP

Object-Oriented Programming organizes code around **objects** — bundles of data (attributes/state) and behavior (methods) — instead of a sequence of functions acting on separate data (procedural style). Goal: model real-world entities directly, keep related data and logic together, and make code modular, reusable, and easier to reason about as it grows.

**Why not just a struct?** A struct/record can hold multiple fields of different types (`int`, `string`, `double`, ...), but it **cannot define functions on itself**. E.g. in a chess struct you can't give a knight piece its own `move()` — the behavior has to live somewhere else, disconnected from the data. OOP fixes this by letting a class own both the data and the functions that act on it.

**Pure vs partial (hybrid) vs non-OOP languages**

| | Meaning | Examples |
|---|---|---|
| **Pure OOP** | Everything is an object — no standalone primitives, no functions outside a class | Smalltalk, Ruby, Eiffel |
| **Partial / hybrid OOP** | Supports classes/objects fully, but also allows primitives and/or procedural or functional style outside classes | Java, C#, C++, Python, TS/JS |
| **Non-OOP (procedural)** | No classes/objects at all — just functions operating on data | C, Pascal |

Java is a common trick question — it's often called "pure OOP" but isn't: it has primitive types (`int`, `boolean`, `char`, ...) that are **not objects**, and `static` methods that don't belong to any instance. Ruby is closer to pure — even a number like `5` is an object you can call methods on (`5.times { ... }`).

TS/JS are prototype-based under the hood (not class-based like Java/C#, even though `class` syntax was added later as sugar), and freely mix OOP with plain functions/objects — squarely hybrid, same bucket as Python and C++.

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

**What "interface" actually means (OOP sense vs TS sense)** — this word does double duty and it's worth separating the two:

- **OOP/language sense** — an `interface` is a named contract: a set of method signatures with no bodies. A class must explicitly declare `implements InterfaceName` to satisfy it (checked at compile time). This is **nominal typing** — the relationship exists only because the class said so, not because the shape happens to match.

```ts
interface Shape {
  area(): number;
}
class Circle implements Shape {
  constructor(private r: number) {}
  area(): number { return Math.PI * this.r * this.r; }
}
```

```java
interface Shape {
    double area();
}
class Circle implements Shape {
    private double r;
    Circle(double r) { this.r = r; }
    public double area() { return Math.PI * r * r; }
}
```

```csharp
interface IShape {
    double Area();
}
class Circle : IShape {
    private double r;
    public Circle(double r) { r = r; }
    public double Area() => Math.PI * r * r;
}
```

- **TS-specific sense** — TS `interface` is broader than that. It's a **compile-time-only** type description — fully erased from the emitted JS, zero runtime cost — and it can describe *any* shape: a plain object, a function signature, an array-like, not just something a class implements.

```ts
interface Point { x: number; y: number; } // no class at all, just a shape
function distance(p: Point) { return Math.sqrt(p.x ** 2 + p.y ** 2); }
distance({ x: 3, y: 4 }); // works — plain object, never declared "implements Point"
```

TS also uses **structural typing**: any object matching the shape satisfies the interface, no explicit `implements` needed (the `distance` call above never mentions `Point` by name). That's the opposite of Java/C#'s nominal typing, where a class must explicitly opt in.

Bridge: when a TS class writes `class Circle implements Shape`, that usage lines up with the OOP sense above. When `interface` just types a plain object or function parameter (like `Point`), that's the TS-only sense — there's no equivalent concept in Java/C#, since neither language lets you type-check a class-less object against a named contract.

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

**Dynamic polymorphism (overriding)** — child class redefines a method it inherited from its parent, keeping the **exact same signature** (name, params, return type/covariant). Resolved at runtime based on the object's actual type, not the variable's declared type — this is "dynamic dispatch". Requires an inheritance relationship (no parent/child, no overriding).

This is exactly what `Dog extends Animal` did earlier (line ~148): `Animal.speak()` returns `"..."`, `Dog.speak()` overrides it to return `"Bark"` — same method name and signature, different behavior per class, decided at runtime.

```ts
class Dog extends Animal {
  speak(): string { return "Bark"; } // overrides Animal.speak() — TS has no override keyword, just re-declare
}
```

```java
class Dog extends Animal {
    @Override
    String speak() { return "Bark"; } // @Override optional but catches typos/signature mismatches at compile time
}
```

```csharp
class Dog : Animal {
    public override string Speak() => "Bark"; // override keyword mandatory — parent method must be marked virtual
}
```

Gotcha: Java's `@Override` is optional (just a compiler safety net); C#'s `override` is mandatory and the parent method must be explicitly `virtual` (or `abstract`) or it won't compile. TS/JS have no override keyword at all — a subclass method with the same name silently overrides, no compiler check.

**Overloading vs overriding**

| | Overloading | Overriding |
|---|---|---|
| Binding | Compile-time (static) | Runtime (dynamic) |
| Signature | Must differ | Must be identical |
| Scope | Same class | Parent → child |
| Return type | Can differ | Must be same or covariant |

**Abstract class vs interface**

Both exist because they solve different problems. **Abstract class** = shared code + state + "is-a" — use it when subclasses have common data/behavior worth inheriting, but a class can only extend *one* abstract class (no multiple inheritance in Java/C#). **Interface** = pure capability contract + "can-do" — a class can implement *many* interfaces, so interfaces are how a class picks up several unrelated capabilities without the single-parent limit.

```ts
abstract class Bird {
  age: number = 0;
  eat(): void { console.log("Eating seeds"); } // shared implemented method
}
interface Flyable { fly(): void; }
interface Swimmable { swim(): void; }
class Duck extends Bird implements Flyable, Swimmable {
  fly(): void { console.log("Flying"); }
  swim(): void { console.log("Swimming"); }
}
```

`Duck` inherits `eat()`/`age` from one abstract class, but implements two interfaces at once — that combination (one base class, many interfaces) is exactly why the split exists.

- Abstract class: can have state, constructors, implemented methods. Single inheritance (in Java/C#).
- Interface: contract only (pre-Java 8). Multiple implementation allowed.
- Java 8+ / C# 8+ note: interfaces can now have **default method bodies** (a fallback implementation), which blurs the line a bit — but interfaces still can't hold state (fields). That's the one distinction abstract classes keep that interfaces never got.
- Rule of thumb: abstract class = "is-a" with shared code; interface = "can-do" capability.

**Other terms**

- **Static** — members that belong to the *class* itself, not to any one instance. No `this`/instance needed to call one; accessed via the class name directly.
  ```ts
  class MathUtil {
    static square(n: number): number { return n * n; }
  }
  MathUtil.square(5); // no `new MathUtil()` needed
  ```

- **Composition over inheritance** — prefer building a class out of other objects it *has* (has-a) rather than making it *extend* something (is-a), because inheritance couples the subclass tightly to the parent's internals — change the parent, and every subclass risks breaking. E.g. `class Car { engine: Engine }` (composition, `Car` has-a `Engine`) is more flexible than `class Car extends Engine` (inheritance would be wrong here anyway — a car isn't a kind of engine).

- **Coupling** — how much one class depends on/knows about another's internals. Low coupling = good: classes can change independently. Tight coupling = a change in one class forces changes in another.
- **Cohesion** — how focused a single class's responsibilities are. High cohesion = good: a class does one clear job (see Single Responsibility below). Low cohesion = a class doing unrelated things is harder to understand, test, and reuse.

- **Association / Aggregation / Composition** — three strengths of "one class relates to another", discriminated by lifetime dependency:
  - *Association* — plain link, no ownership. E.g. `Teacher` and `Student`: each can exist without the other, they just reference each other.
  - *Aggregation* — has-a, but *independent* lifetime. E.g. `Car` has an `Engine`, but the `Engine` object could be built before the car and swapped into another car — it doesn't die with the car.
  - *Composition* — has-a, but *dependent* lifetime. E.g. `House` has `Room`s — delete the `House`, the `Room`s go with it; a `Room` doesn't make sense existing outside some house.

- **Method hiding** — a `static` method redeclared in a subclass *hides* the parent's version instead of overriding it: which one runs is picked by the **declared/reference type** at compile time, not the actual object type — the opposite of overriding's runtime dispatch.
  ```java
  class Animal { static String kind() { return "Animal"; } }
  class Dog extends Animal { static String kind() { return "Dog"; } }
  Animal a = new Dog();
  System.out.println(a.kind());
  // Output: Animal — resolved by the reference type (Animal), NOT the actual object (Dog)
  // Contrast: instance method overriding (see Polymorphism above) would print "Dog" here.
  ```

- **Constructor chaining** — a child constructor implicitly calls the parent's no-arg constructor first (via an invisible `super()`/`base()` at the top), unless it explicitly calls a specific parent constructor itself — so the parent is always fully initialized before the child's own constructor body runs.

- **Shallow copy vs deep copy** — a shallow copy duplicates the top-level object but copies nested object fields *by reference*; a deep copy recursively duplicates those nested objects too.
  ```ts
  const original = { name: "Bob", address: { city: "NYC" } };
  const shallow = { ...original };
  shallow.address.city = "LA";
  console.log(original.address.city);
  // Output: LA — shallow copy shared the nested `address` object, so mutating via the copy leaked back
  ```

**SOLID**

- **S**ingle Responsibility — a class should have only **one reason to change**. E.g. a `Report` class that both formats text *and* saves the file to disk has two reasons to change (formatting logic, storage logic) — split it into `ReportFormatter` + `ReportSaver` so each changes independently.
- **O**pen/Closed — classes should be **open for extension, closed for modification**. E.g. the `PaymentMethod` hierarchy from Abstraction above: adding a new payment type means writing a new `class CryptoPayment extends PaymentMethod`, never editing `CardPayment`'s existing `pay()` body.
- **L**iskov Substitution — a subtype must be usable **anywhere** the base type is, without breaking correctness. Classic violation: `class Square extends Rectangle` that overrides `setWidth()` to also change height (to stay square) — any code that sets width and height independently on a `Rectangle` now breaks silently when handed a `Square`.
- **I**nterface Segregation — prefer **many small interfaces over one fat one**, so implementers aren't forced to support methods they don't need. E.g. one bloated `Worker` interface with `work()` + `eat()` forces a `RobotWorker` to implement a meaningless `eat()` — split into separate `Workable` and `Eatable` interfaces instead.
- **D**ependency Inversion — depend on **abstractions, not concrete implementations**. E.g. a class that does `new MySQLDatabase()` directly is locked to MySQL forever; depending on a `Database` interface (implemented by `MySQLDatabase`, `PostgresDatabase`, ...) lets the concrete implementation swap out without touching the dependent class.

## Advantages / Limitations of OOP

**Advantages**
- **Modularity** — a program splits into independent, self-contained classes, each owning its own data + behavior (Encapsulation), so a class can be understood, changed, or replaced without reading the rest of the program.
- **Easy maintenance** — because state is encapsulated and accessed only through methods (information hiding), changes to one class's internals stay local instead of rippling out to every place that used to reach in directly.
- **Improved readability** — classes model real entities with named methods (`car.drive()` vs loose functions passing a car struct around), so code reads closer to the problem domain it's modeling.
- **Scalability** — Open/Closed in practice: new behavior gets added via new subclasses/interfaces rather than editing shared code, so growing the codebase doesn't mean destabilizing what already works.
- **Faster development / reusability** — Inheritance and Composition let new classes reuse existing, already-tested code instead of rewriting it from scratch.
- **Better testing** — since each class encapsulates its own state and exposes a defined interface, it can be instantiated and tested in isolation, without standing up the whole program around it.
- **Real-world modeling** — bundling data + behavior together (the core OOP idea) maps naturally onto real entities (a `Car` that can `drive()`), making the design easier to reason about than parallel structs and free functions.

**Limitations**
- **Steeper learning curve** — the four pillars plus supporting patterns (SOLID, abstract-class-vs-interface, association/aggregation/composition) are a lot of interlocking vocabulary and judgment calls to internalize before they click.
- **Overhead for simple programs** — a script that transforms some data once doesn't need classes, encapsulation, or hierarchies — the ceremony (constructors, getters/setters, interfaces) costs more than it returns at that scale.
- **Design complexity** — class hierarchies must be planned before most code is written; getting the hierarchy wrong early (e.g. a Liskov Substitution violation baked into the design) is expensive to unwind later since other classes come to depend on it.
- **Higher memory use** — each object carries its own copy of instance state (vs plain functions operating on shared/passed data), and deep hierarchies plus vtables/dynamic dispatch add per-object overhead that procedural code doesn't pay.

## Further Practice

- [Sanfoundry — 1000 OOP MCQ Questions & Answers](https://www.sanfoundry.com/1000-object-oriented-programming-oops-questions-answers/)
