# 2. Design Patterns

Reusable, named solutions to problems that keep recurring in OOP design. Knowing the name matters as much as knowing the shape — it lets you say "this needs a Strategy" instead of re-deriving the structure from scratch every time. Three main groups: **Creational** (how objects get made), **Structural** (how objects/classes compose), **Behavioural** (how objects communicate/change).

## What even is a "design pattern"?

Say you're building a notification system. It starts simple: one function that sends an email. Then the boss wants SMS too, so an `if (type === "sms")` gets bolted on. Then push notifications — another `else if`. Six months later that one function is a wall of conditionals, and every time a new notification type shows up, someone has to go back into that same fragile function and risk breaking the other three types while they're at it.

This exact mess — "I need to swap in new behavior without touching the code that already works" — comes up constantly, in every language, in every kind of app. Enough people hit it enough times that a *standard fix* emerged: instead of one function with a growing pile of conditionals, each notification type gets its own small class with the same method name (`send()`), and the calling code just calls whichever one it's handed, no `if` chain at all. That standard fix has a name — **Strategy** — and it's one of the 21 patterns below.

That's what a design pattern actually is: **a recurring problem shape, paired with a battle-tested solution shape, that's been given a name.** It is *not* a library you install or a snippet you copy-paste — there's no `import Strategy from "patterns"`. You re-build the shape yourself, out of your own classes, every single time you use it. What you're reusing is the *idea*, not the code.

Why bother naming them at all? Two reasons. First, speed of communication — telling a teammate "just make this a Strategy" is a lot faster than re-explaining "make each option its own class with a shared method name" from scratch. Second, and more useful day to day: once you recognize the *shape* of a problem, you can reach for the fix before you've written yourself into the if-chain mess in the first place.

The patterns below sort into four rough families, in plain terms:
- **Creational** — patterns about *how objects get born*. (Constructing things cleanly instead of `new`-ing them all over the place in ways that get tangled.)
- **Structural** — patterns about *how objects/classes fit together*, like picking the right Lego pieces so two things that don't quite match can still connect.
- **Behavioural** — patterns about *how objects talk to each other and change over time*.
- **Architectural** — zoomed-out patterns for how a whole app's layers are organized, rather than how one class relates to another.

**How to read each entry below:** every pattern starts with a one-line plain-English "In short" — read just that line first and move on if it's already familiar. After that comes the precise technical problem it solves, a real-world analogy, working code in three languages showing the same shape, and (where relevant) a "Gotcha" — a well-known way people misuse that exact pattern.

## Creational

### Singleton
In short: make sure a class can only ever be created once, so everyone shares that one instance.

Ensures exactly one instance of a class exists and gives one global access point to it — needed when having a second instance would cause real problems (e.g. two DB connection pools or two loggers silently diverging in state). Analogy: a country has one government — you don't "instantiate" a second one.

```ts
class Singleton {
  private static instance: Singleton;
  private constructor() {}
  static getInstance(): Singleton {
    if (!Singleton.instance) Singleton.instance = new Singleton();
    return Singleton.instance;
  }
}
const a = Singleton.getInstance();
const b = Singleton.getInstance();
console.log(a === b);
// Output: true
```

```java
class Singleton {
    private static Singleton instance;
    private Singleton() {}
    static Singleton getInstance() {
        if (instance == null) instance = new Singleton();
        return instance;
    }
}
Singleton a = Singleton.getInstance();
Singleton b = Singleton.getInstance();
System.out.println(a == b);
// Output: true
```

```csharp
class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton GetInstance() {
        instance ??= new Singleton();
        return instance;
    }
}
var a = Singleton.GetInstance();
var b = Singleton.GetInstance();
Console.WriteLine(a == b);
// Output: True
```

Gotcha: overused Singleton is disguised global state — it hides a class's real dependencies and makes unit testing harder, since you can't easily swap in a mock instance.

### Factory Method
In short: let subclasses decide exactly what object gets created, instead of hardcoding it in one place.

A base class defines *that* an object gets created, but leaves *which concrete class* to instantiate up to the subclass, via overriding. Analogy: a restaurant chain's central menu says "make a dessert" — each branch decides whether that means cake or ice cream.

```ts
abstract class Dialog {
  abstract createButton(): string;
  render(): void { console.log(`Rendering with ${this.createButton()}`); }
}
class WindowsDialog extends Dialog {
  createButton(): string { return "Windows button"; }
}
new WindowsDialog().render();
// Output: Rendering with Windows button
```

```java
abstract class Dialog {
    abstract String createButton();
    void render() { System.out.println("Rendering with " + createButton()); }
}
class WindowsDialog extends Dialog {
    String createButton() { return "Windows button"; }
}
new WindowsDialog().render();
// Output: Rendering with Windows button
```

```csharp
abstract class Dialog {
    public abstract string CreateButton();
    public void Render() => Console.WriteLine($"Rendering with {CreateButton()}");
}
class WindowsDialog : Dialog {
    public override string CreateButton() => "Windows button";
}
new WindowsDialog().Render();
// Output: Rendering with Windows button
```

Distinguish from Abstract Factory below: Factory Method makes **one** product via subclassing; Abstract Factory makes **families** of related products via composition.

### Abstract Factory
In short: create a matching set of related objects together, so you never end up mixing incompatible pieces.

Creates families of related objects that must match each other (e.g. a dark-theme button must pair with a dark-theme checkbox, never a light one) without the caller knowing which family it's using. Analogy: ordering a combo meal — burger+fries+drink all come from the same matched set, you don't mix combos.

```ts
interface Button { render(): string; }
interface Checkbox { render(): string; }
interface UIFactory { createButton(): Button; createCheckbox(): Checkbox; }
class DarkFactory implements UIFactory {
  createButton(): Button { return { render: () => "dark button" }; }
  createCheckbox(): Checkbox { return { render: () => "dark checkbox" }; }
}
const factory: UIFactory = new DarkFactory();
console.log(factory.createButton().render(), factory.createCheckbox().render());
// Output: dark button dark checkbox
```

```java
interface Button { String render(); }
interface Checkbox { String render(); }
interface UIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
class DarkFactory implements UIFactory {
    public Button createButton() { return () -> "dark button"; }
    public Checkbox createCheckbox() { return () -> "dark checkbox"; }
}
UIFactory factory = new DarkFactory();
System.out.println(factory.createButton().render() + " " + factory.createCheckbox().render());
// Output: dark button dark checkbox
```

```csharp
interface IButton { string Render(); }
interface ICheckbox { string Render(); }
interface IUIFactory {
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}
class DarkButton : IButton { public string Render() => "dark button"; }
class DarkCheckbox : ICheckbox { public string Render() => "dark checkbox"; }
class DarkFactory : IUIFactory {
    public IButton CreateButton() => new DarkButton();
    public ICheckbox CreateCheckbox() => new DarkCheckbox();
}
IUIFactory factory = new DarkFactory();
Console.WriteLine($"{factory.CreateButton().Render()} {factory.CreateCheckbox().Render()}");
// Output: dark button dark checkbox
```

Gotcha: adding a new product type (e.g. a `Slider`) to the family means editing **every** factory implementation — the opposite tradeoff of Factory Method, which makes adding new products easy but doesn't group them into families.

### Builder
In short: build a complicated object one step at a time instead of via one giant constructor call.

Constructs a complex object step by step instead of via a constructor with many optional args ("telescoping constructor" — unreadable and error-prone past 3-4 params). Analogy: ordering a custom sandwich — built step by step (bread, then filling, then sauce), not specified in one giant order line.

```ts
class Pizza {
  constructor(public toppings: string[], public size: string) {}
}
class PizzaBuilder {
  private toppings: string[] = [];
  private size = "medium";
  addTopping(t: string): this { this.toppings.push(t); return this; }
  setSize(s: string): this { this.size = s; return this; }
  build(): Pizza { return new Pizza(this.toppings, this.size); }
}
const pizza = new PizzaBuilder().addTopping("cheese").addTopping("olives").setSize("large").build();
console.log(pizza.size, pizza.toppings.join(","));
// Output: large cheese,olives
```

```java
class Pizza {
    List<String> toppings; String size;
    Pizza(List<String> toppings, String size) { this.toppings = toppings; this.size = size; }
}
class PizzaBuilder {
    private List<String> toppings = new ArrayList<>();
    private String size = "medium";
    PizzaBuilder addTopping(String t) { toppings.add(t); return this; }
    PizzaBuilder setSize(String s) { size = s; return this; }
    Pizza build() { return new Pizza(toppings, size); }
}
Pizza pizza = new PizzaBuilder().addTopping("cheese").addTopping("olives").setSize("large").build();
System.out.println(pizza.size + " " + String.join(",", pizza.toppings));
// Output: large cheese,olives
```

```csharp
class Pizza {
    public List<string> Toppings; public string Size;
    public Pizza(List<string> toppings, string size) { Toppings = toppings; Size = size; }
}
class PizzaBuilder {
    private List<string> toppings = new();
    private string size = "medium";
    public PizzaBuilder AddTopping(string t) { toppings.Add(t); return this; }
    public PizzaBuilder SetSize(string s) { size = s; return this; }
    public Pizza Build() => new Pizza(toppings, size);
}
var pizza = new PizzaBuilder().AddTopping("cheese").AddTopping("olives").SetSize("large").Build();
Console.WriteLine($"{pizza.Size} {string.Join(",", pizza.Toppings)}");
// Output: large cheese,olives
```

### Prototype
In short: make a new object by copying an existing one instead of building it from zero.

Creates a new object by cloning an existing, already-configured instance instead of building it from scratch. Analogy: photocopying a filled-out form instead of writing a blank one from memory each time.

```ts
class Enemy {
  constructor(public type: string, public health: number, public damage: number) {}
  clone(): Enemy { return new Enemy(this.type, this.health, this.damage); }
}
const goblinTemplate = new Enemy("Goblin", 30, 5);
const goblin2 = goblinTemplate.clone();
goblin2.health = 25; // tweak the copy without touching the template
console.log(goblinTemplate.health, goblin2.health);
// Output: 30 25
```

```java
class Enemy {
    String type; int health, damage;
    Enemy(String type, int health, int damage) { this.type = type; this.health = health; this.damage = damage; }
    Enemy copy() { return new Enemy(type, health, damage); }
}
Enemy goblinTemplate = new Enemy("Goblin", 30, 5);
Enemy goblin2 = goblinTemplate.copy();
goblin2.health = 25;
System.out.println(goblinTemplate.health + " " + goblin2.health);
// Output: 30 25
```

```csharp
class Enemy {
    public string Type; public int Health, Damage;
    public Enemy(string type, int health, int damage) { Type = type; Health = health; Damage = damage; }
    public Enemy Clone() => new Enemy(Type, Health, Damage);
}
var goblinTemplate = new Enemy("Goblin", 30, 5);
var goblin2 = goblinTemplate.Clone();
goblin2.Health = 25;
Console.WriteLine($"{goblinTemplate.Health} {goblin2.Health}");
// Output: 30 25
```

Gotcha: a naive clone is a **shallow copy** (see 1-oop.md's shallow vs deep copy) — if `Enemy` held a nested mutable object (e.g. an `Inventory`), cloning would share that reference unless deep-cloned too.

## Structural

### Adapter
In short: make one thing's interface fit what another thing expects, without changing either.

Converts one interface into another that the caller expects, without changing either side's actual behavior — used when you can't modify a legacy API but need it to match a new interface. Analogy: a physical power-plug adapter — doesn't change what the socket or the plug do, just bridges the shape mismatch.

```ts
class LegacyPrinter {
  printOld(text: string): void { console.log(`[legacy] ${text}`); }
}
interface Printer { print(text: string): void; }
class PrinterAdapter implements Printer {
  constructor(private legacy: LegacyPrinter) {}
  print(text: string): void { this.legacy.printOld(text); }
}
const printer: Printer = new PrinterAdapter(new LegacyPrinter());
printer.print("Hello");
// Output: [legacy] Hello
```

```java
class LegacyPrinter {
    void printOld(String text) { System.out.println("[legacy] " + text); }
}
interface Printer { void print(String text); }
class PrinterAdapter implements Printer {
    private LegacyPrinter legacy;
    PrinterAdapter(LegacyPrinter legacy) { this.legacy = legacy; }
    public void print(String text) { legacy.printOld(text); }
}
Printer printer = new PrinterAdapter(new LegacyPrinter());
printer.print("Hello");
// Output: [legacy] Hello
```

```csharp
class LegacyPrinter {
    public void PrintOld(string text) => Console.WriteLine($"[legacy] {text}");
}
interface IPrinter { void Print(string text); }
class PrinterAdapter : IPrinter {
    private LegacyPrinter legacy;
    public PrinterAdapter(LegacyPrinter legacy) { this.legacy = legacy; }
    public void Print(string text) => legacy.PrintOld(text);
}
IPrinter printer = new PrinterAdapter(new LegacyPrinter());
printer.Print("Hello");
// Output: [legacy] Hello
```

Gotcha: Adapter only reshapes the interface — it never changes what the wrapped class actually does. If the behavior itself needs to change, that's Decorator's job, not Adapter's.

### Decorator
In short: add extra behavior to a single object by wrapping it, without touching its original code.

Adds behavior to an individual object at runtime by wrapping it, avoiding a combinatorial explosion of subclasses (Coffee, Coffee+Milk, Coffee+Sugar, Coffee+Milk+Sugar, ... only gets worse with more options). Analogy: Express/Koa middleware — each layer wraps the request handler, adding behavior before/after, without the core handler knowing.

```ts
interface Coffee { cost(): number; }
class SimpleCoffee implements Coffee { cost(): number { return 2; } }
class MilkDecorator implements Coffee {
  constructor(private coffee: Coffee) {}
  cost(): number { return this.coffee.cost() + 0.5; }
}
const drink = new MilkDecorator(new SimpleCoffee());
console.log(drink.cost());
// Output: 2.5
```

```java
interface Coffee { double cost(); }
class SimpleCoffee implements Coffee {
    public double cost() { return 2; }
}
class MilkDecorator implements Coffee {
    private Coffee coffee;
    MilkDecorator(Coffee coffee) { this.coffee = coffee; }
    public double cost() { return coffee.cost() + 0.5; }
}
Coffee drink = new MilkDecorator(new SimpleCoffee());
System.out.println(drink.cost());
// Output: 2.5
```

```csharp
interface ICoffee { double Cost(); }
class SimpleCoffee : ICoffee { public double Cost() => 2; }
class MilkDecorator : ICoffee {
    private ICoffee coffee;
    public MilkDecorator(ICoffee coffee) { this.coffee = coffee; }
    public double Cost() => coffee.Cost() + 0.5;
}
ICoffee drink = new MilkDecorator(new SimpleCoffee());
Console.WriteLine(drink.Cost());
// Output: 2.5
```

Gotcha: stacking many decorators makes the call chain hard to trace — debugging means stepping through a dozen thin wrapper layers to find where a value actually changed.

### Facade
In short: give people one simple button that hides a complicated set of steps underneath.

Provides one simplified entry point over a complex subsystem with many moving parts and a specific setup order, so callers don't need to learn every part just to get the common result. Analogy: a car's ignition button — one press hides starting the engine, fuel pump, ECU, etc.

```ts
class Projector { turnOn(): void { console.log("Projector on"); } }
class SoundSystem { turnOn(): void { console.log("Sound on"); } }
class HomeTheaterFacade {
  constructor(private projector: Projector, private sound: SoundSystem) {}
  watchMovie(): void { this.projector.turnOn(); this.sound.turnOn(); console.log("Enjoy the movie"); }
}
new HomeTheaterFacade(new Projector(), new SoundSystem()).watchMovie();
// Output:
// Projector on
// Sound on
// Enjoy the movie
```

```java
class Projector { void turnOn() { System.out.println("Projector on"); } }
class SoundSystem { void turnOn() { System.out.println("Sound on"); } }
class HomeTheaterFacade {
    private Projector projector; private SoundSystem sound;
    HomeTheaterFacade(Projector p, SoundSystem s) { projector = p; sound = s; }
    void watchMovie() {
        projector.turnOn(); sound.turnOn();
        System.out.println("Enjoy the movie");
    }
}
new HomeTheaterFacade(new Projector(), new SoundSystem()).watchMovie();
// Output:
// Projector on
// Sound on
// Enjoy the movie
```

```csharp
class Projector { public void TurnOn() => Console.WriteLine("Projector on"); }
class SoundSystem { public void TurnOn() => Console.WriteLine("Sound on"); }
class HomeTheaterFacade {
    private Projector projector; private SoundSystem sound;
    public HomeTheaterFacade(Projector p, SoundSystem s) { projector = p; sound = s; }
    public void WatchMovie() {
        projector.TurnOn(); sound.TurnOn();
        Console.WriteLine("Enjoy the movie");
    }
}
new HomeTheaterFacade(new Projector(), new SoundSystem()).WatchMovie();
// Output:
// Projector on
// Sound on
// Enjoy the movie
```

### Proxy
In short: put a stand-in in front of the real object that controls or delays access to it.

A stand-in object that controls access to a real object — delaying expensive creation until actually needed (lazy loading), checking permissions before delegating, or caching results. Analogy: a receptionist — screens/handles requests before deciding whether to bother the real person inside.

```ts
interface Image { display(): void; }
class RealImage implements Image {
  constructor(private file: string) { console.log(`Loading ${file} from disk`); }
  display(): void { console.log(`Displaying ${this.file}`); }
}
class ImageProxy implements Image {
  private real?: RealImage;
  constructor(private file: string) {}
  display(): void {
    if (!this.real) this.real = new RealImage(this.file); // loaded only on first use
    this.real.display();
  }
}
const img = new ImageProxy("photo.png");
img.display();
// Output:
// Loading photo.png from disk
// Displaying photo.png
```

```java
interface Image { void display(); }
class RealImage implements Image {
    private String file;
    RealImage(String file) { this.file = file; System.out.println("Loading " + file + " from disk"); }
    public void display() { System.out.println("Displaying " + file); }
}
class ImageProxy implements Image {
    private RealImage real; private String file;
    ImageProxy(String file) { this.file = file; }
    public void display() {
        if (real == null) real = new RealImage(file);
        real.display();
    }
}
Image img = new ImageProxy("photo.png");
img.display();
// Output:
// Loading photo.png from disk
// Displaying photo.png
```

```csharp
interface IImage { void Display(); }
class RealImage : IImage {
    private string file;
    public RealImage(string file) { this.file = file; Console.WriteLine($"Loading {file} from disk"); }
    public void Display() => Console.WriteLine($"Displaying {file}");
}
class ImageProxy : IImage {
    private RealImage real; private string file;
    public ImageProxy(string file) { this.file = file; }
    public void Display() {
        real ??= new RealImage(file);
        real.Display();
    }
}
IImage img = new ImageProxy("photo.png");
img.Display();
// Output:
// Loading photo.png from disk
// Displaying photo.png
```

### Composite
In short: treat one item and a whole group of items the exact same way in your code.

Lets client code treat a single object and a group of objects the same way, so it never has to special-case "is this a leaf or a group?". Analogy: a filesystem tree — a folder's size is just the sum of whatever it contains, file or folder alike.

```ts
interface FileSystemItem { getSize(): number; }
class FileItem implements FileSystemItem {
  constructor(private size: number) {}
  getSize(): number { return this.size; }
}
class Folder implements FileSystemItem {
  private items: FileSystemItem[] = [];
  add(item: FileSystemItem): void { this.items.push(item); }
  getSize(): number { return this.items.reduce((sum, i) => sum + i.getSize(), 0); }
}
const folder = new Folder();
folder.add(new FileItem(100));
folder.add(new FileItem(250));
console.log(folder.getSize());
// Output: 350
```

```java
interface FileSystemItem { int getSize(); }
class FileItem implements FileSystemItem {
    private int size;
    FileItem(int size) { this.size = size; }
    public int getSize() { return size; }
}
class Folder implements FileSystemItem {
    private List<FileSystemItem> items = new ArrayList<>();
    void add(FileSystemItem item) { items.add(item); }
    public int getSize() { return items.stream().mapToInt(FileSystemItem::getSize).sum(); }
}
Folder folder = new Folder();
folder.add(new FileItem(100));
folder.add(new FileItem(250));
System.out.println(folder.getSize());
// Output: 350
```

```csharp
interface IFileSystemItem { int GetSize(); }
class FileItem : IFileSystemItem {
    private int size;
    public FileItem(int size) { this.size = size; }
    public int GetSize() => size;
}
class Folder : IFileSystemItem {
    private List<IFileSystemItem> items = new();
    public void Add(IFileSystemItem item) => items.Add(item);
    public int GetSize() => items.Sum(i => i.GetSize());
}
var folder = new Folder();
folder.Add(new FileItem(100));
folder.Add(new FileItem(250));
Console.WriteLine(folder.GetSize());
// Output: 350
```

### Bridge
In short: keep "what something is" and "how it's actually done" as two separate, swappable pieces.

Decouples an abstraction from its implementation so both can vary independently — avoids the class explosion of `Shape` types × `Renderer` types (`VectorCircle`, `RasterCircle`, `VectorSquare`, `RasterSquare`, ...) that pure inheritance would cause. Analogy: a TV remote (abstraction) works with any TV brand (implementation) as long as both honor the same connecting interface — no different remote needed per brand.

```ts
interface Renderer { renderShape(name: string): void; }
class VectorRenderer implements Renderer {
  renderShape(name: string): void { console.log(`Drawing ${name} as vectors`); }
}
abstract class Shape {
  constructor(protected renderer: Renderer) {}
  abstract draw(): void;
}
class Circle extends Shape {
  draw(): void { this.renderer.renderShape("circle"); }
}
new Circle(new VectorRenderer()).draw();
// Output: Drawing circle as vectors
```

```java
interface Renderer { void renderShape(String name); }
class VectorRenderer implements Renderer {
    public void renderShape(String name) { System.out.println("Drawing " + name + " as vectors"); }
}
abstract class Shape {
    protected Renderer renderer;
    Shape(Renderer renderer) { this.renderer = renderer; }
    abstract void draw();
}
class Circle extends Shape {
    Circle(Renderer renderer) { super(renderer); }
    void draw() { renderer.renderShape("circle"); }
}
new Circle(new VectorRenderer()).draw();
// Output: Drawing circle as vectors
```

```csharp
interface IRenderer { void RenderShape(string name); }
class VectorRenderer : IRenderer {
    public void RenderShape(string name) => Console.WriteLine($"Drawing {name} as vectors");
}
abstract class Shape {
    protected IRenderer renderer;
    protected Shape(IRenderer renderer) { this.renderer = renderer; }
    public abstract void Draw();
}
class Circle : Shape {
    public Circle(IRenderer renderer) : base(renderer) {}
    public override void Draw() => renderer.RenderShape("circle");
}
new Circle(new VectorRenderer()).Draw();
// Output: Drawing circle as vectors
```

Gotcha: easy to confuse with Adapter — Adapter is retrofitted after the fact to reconcile two existing incompatible interfaces; Bridge is designed upfront so abstraction and implementation are separate hierarchies from the start.

### Flyweight
In short: share the data that's identical across many objects, instead of duplicating it in each one.

Shares common, rarely-changing state across many similar objects to cut memory — needed when creating huge numbers of objects that would otherwise each carry their own copy of the same data (e.g. thousands of trees in a rendered forest). Analogy: a print shop keeps one master stencil (shared) and only tracks where each stamped copy goes (unique per-instance data).

```ts
class TreeType { // shared/intrinsic state
  constructor(public name: string, public texture: string) {}
}
class TreeFactory {
  private static types = new Map<string, TreeType>();
  static getType(name: string, texture: string): TreeType {
    const key = `${name}-${texture}`;
    if (!this.types.has(key)) this.types.set(key, new TreeType(name, texture));
    return this.types.get(key)!;
  }
}
class Tree { // unique/extrinsic state: just position
  constructor(public x: number, public y: number, public type: TreeType) {}
}
const oak = TreeFactory.getType("Oak", "oak.png");
const t1 = new Tree(10, 20, oak);
const t2 = new Tree(30, 40, oak);
console.log(t1.type === t2.type);
// Output: true — both trees share the same TreeType instance
```

```java
class TreeType {
    String name, texture;
    TreeType(String name, String texture) { this.name = name; this.texture = texture; }
}
class TreeFactory {
    static Map<String, TreeType> types = new HashMap<>();
    static TreeType getType(String name, String texture) {
        return types.computeIfAbsent(name + "-" + texture, k -> new TreeType(name, texture));
    }
}
class Tree {
    int x, y; TreeType type;
    Tree(int x, int y, TreeType type) { this.x = x; this.y = y; this.type = type; }
}
TreeType oak = TreeFactory.getType("Oak", "oak.png");
Tree t1 = new Tree(10, 20, oak);
Tree t2 = new Tree(30, 40, oak);
System.out.println(t1.type == t2.type);
// Output: true
```

```csharp
class TreeType {
    public string Name, Texture;
    public TreeType(string name, string texture) { Name = name; Texture = texture; }
}
class TreeFactory {
    static Dictionary<string, TreeType> types = new();
    public static TreeType GetType(string name, string texture) {
        var key = $"{name}-{texture}";
        if (!types.ContainsKey(key)) types[key] = new TreeType(name, texture);
        return types[key];
    }
}
class Tree {
    public int X, Y; public TreeType Type;
    public Tree(int x, int y, TreeType type) { X = x; Y = y; Type = type; }
}
var oak = TreeFactory.GetType("Oak", "oak.png");
var t1 = new Tree(10, 20, oak);
var t2 = new Tree(30, 40, oak);
Console.WriteLine(t1.Type == t2.Type);
// Output: True
```

## Behavioural

### Observer
In short: let many things get notified automatically whenever one thing changes.

Notifies many dependents automatically when one object's state changes, without that object needing to know who they are individually. Analogy: a newsletter — the publisher doesn't know subscribers individually, it just broadcasts. Powers event emitters and pub/sub systems.

```ts
type Callback = (data: string) => void;
class Subject {
  private observers: Callback[] = [];
  subscribe(cb: Callback): void { this.observers.push(cb); }
  notify(data: string): void { this.observers.forEach(cb => cb(data)); }
}
const subject = new Subject();
subject.subscribe(data => console.log(`Widget A got: ${data}`));
subject.subscribe(data => console.log(`Widget B got: ${data}`));
subject.notify("state changed");
// Output:
// Widget A got: state changed
// Widget B got: state changed
```

```java
interface Observer { void update(String data); }
class Subject {
    private List<Observer> observers = new ArrayList<>();
    void subscribe(Observer o) { observers.add(o); }
    void notifyObservers(String data) { for (Observer o : observers) o.update(data); }
}
Subject subject = new Subject();
subject.subscribe(data -> System.out.println("Widget A got: " + data));
subject.subscribe(data -> System.out.println("Widget B got: " + data));
subject.notifyObservers("state changed");
// Output:
// Widget A got: state changed
// Widget B got: state changed
```

```csharp
class Subject {
    private List<Action<string>> observers = new();
    public void Subscribe(Action<string> cb) => observers.Add(cb);
    public void Notify(string data) { foreach (var cb in observers) cb(data); }
}
var subject = new Subject();
subject.Subscribe(data => Console.WriteLine($"Widget A got: {data}"));
subject.Subscribe(data => Console.WriteLine($"Widget B got: {data}"));
subject.Notify("state changed");
// Output:
// Widget A got: state changed
// Widget B got: state changed
```

Gotcha: too many observers, or chains where a notify triggers another notify, make it hard to trace what ultimately caused a given update — a common source of hard-to-debug pub/sub systems.

### Strategy
In short: make an algorithm swappable, so you can plug in a different one without rewriting the caller.

Makes an algorithm swappable at runtime by encapsulating each variant behind a common interface, instead of an if/else or switch scattered through the caller. Analogy: a maps app's "fastest" vs "shortest" vs "avoid tolls" mode — same trip, different strategy plugged in. Cross-ref: this is polymorphism (1-oop.md) applied specifically to interchangeable algorithms.

```ts
interface SortStrategy { sort(data: number[]): number[]; }
class AscendingSort implements SortStrategy {
  sort(data: number[]): number[] { return [...data].sort((a, b) => a - b); }
}
class SortContext {
  constructor(private strategy: SortStrategy) {}
  execute(data: number[]): number[] { return this.strategy.sort(data); }
}
console.log(new SortContext(new AscendingSort()).execute([3, 1, 2]));
// Output: [ 1, 2, 3 ]
```

```java
interface SortStrategy { int[] sort(int[] data); }
class AscendingSort implements SortStrategy {
    public int[] sort(int[] data) { int[] copy = data.clone(); Arrays.sort(copy); return copy; }
}
class SortContext {
    private SortStrategy strategy;
    SortContext(SortStrategy strategy) { this.strategy = strategy; }
    int[] execute(int[] data) { return strategy.sort(data); }
}
System.out.println(Arrays.toString(new SortContext(new AscendingSort()).execute(new int[]{3,1,2})));
// Output: [1, 2, 3]
```

```csharp
interface ISortStrategy { int[] Sort(int[] data); }
class AscendingSort : ISortStrategy {
    public int[] Sort(int[] data) { var copy = (int[])data.Clone(); Array.Sort(copy); return copy; }
}
class SortContext {
    private ISortStrategy strategy;
    public SortContext(ISortStrategy strategy) { this.strategy = strategy; }
    public int[] Execute(int[] data) => strategy.Sort(data);
}
Console.WriteLine(string.Join(",", new SortContext(new AscendingSort()).Execute(new[]{3,1,2})));
// Output: 1,2,3
```

### Command
In short: turn an action into an object, so it can be stored, queued, or undone later.

Encapsulates a request as an object, so it can be queued, logged, undone, or handed off — instead of directly calling a method the moment the action happens. Analogy: a restaurant order slip — the waiter doesn't cook on the spot, the request becomes a ticket that can be queued, handed off, or voided.

```ts
interface Command { execute(): void; undo(): void; }
class Light {
  on(): void { console.log("Light on"); }
  off(): void { console.log("Light off"); }
}
class LightOnCommand implements Command {
  constructor(private light: Light) {}
  execute(): void { this.light.on(); }
  undo(): void { this.light.off(); }
}
const cmd = new LightOnCommand(new Light());
cmd.execute();
cmd.undo();
// Output:
// Light on
// Light off
```

```java
interface Command { void execute(); void undo(); }
class Light {
    void on() { System.out.println("Light on"); }
    void off() { System.out.println("Light off"); }
}
class LightOnCommand implements Command {
    private Light light;
    LightOnCommand(Light light) { this.light = light; }
    public void execute() { light.on(); }
    public void undo() { light.off(); }
}
Command cmd = new LightOnCommand(new Light());
cmd.execute();
cmd.undo();
// Output:
// Light on
// Light off
```

```csharp
interface ICommand { void Execute(); void Undo(); }
class Light {
    public void On() => Console.WriteLine("Light on");
    public void Off() => Console.WriteLine("Light off");
}
class LightOnCommand : ICommand {
    private Light light;
    public LightOnCommand(Light light) { this.light = light; }
    public void Execute() => light.On();
    public void Undo() => light.Off();
}
ICommand cmd = new LightOnCommand(new Light());
cmd.Execute();
cmd.Undo();
// Output:
// Light on
// Light off
```

### Iterator
In short: step through a collection's items one at a time without needing to know how it's stored inside.

Lets client code walk through a collection's elements one by one without knowing (or caring) whether it's backed by an array, linked list, or something else. Analogy: a TV remote's channel-up button — you don't need to know how channels are stored internally, just "next".

```ts
class NumberCollection {
  constructor(private items: number[]) {}
  [Symbol.iterator]() {
    let i = 0;
    const items = this.items;
    return { next: () => i < items.length ? { value: items[i++], done: false } : { value: undefined, done: true } };
  }
}
for (const n of new NumberCollection([10, 20, 30])) console.log(n);
// Output:
// 10
// 20
// 30
```

```java
class NumberCollection implements Iterable<Integer> {
    private List<Integer> items;
    NumberCollection(List<Integer> items) { this.items = items; }
    public Iterator<Integer> iterator() { return items.iterator(); } // hides whatever items actually is
}
for (int n : new NumberCollection(List.of(10, 20, 30))) System.out.println(n);
// Output:
// 10
// 20
// 30
```

```csharp
class NumberCollection : IEnumerable<int> {
    private List<int> items;
    public NumberCollection(List<int> items) { this.items = items; }
    public IEnumerator<int> GetEnumerator() => items.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
foreach (var n in new NumberCollection(new List<int>{10, 20, 30})) Console.WriteLine(n);
// Output:
// 10
// 20
// 30
```

### Template Method
In short: lock down the overall steps of a process, but let subclasses fill in a few of those steps differently.

Fixes an algorithm's overall skeleton in a base class, deferring individual steps to subclasses via overriding — avoids copy-pasting the whole algorithm just to change a couple of steps. Analogy: a recipe template — "preheat, cook, serve" is fixed, but what "cook" means differs per dish.

```ts
abstract class Game {
  play(): void { this.initialize(); this.startPlay(); this.endPlay(); } // fixed skeleton
  protected abstract initialize(): void;
  protected abstract startPlay(): void;
  protected abstract endPlay(): void;
}
class Chess extends Game {
  protected initialize(): void { console.log("Chess: set up board"); }
  protected startPlay(): void { console.log("Chess: white moves first"); }
  protected endPlay(): void { console.log("Chess: checkmate"); }
}
new Chess().play();
// Output:
// Chess: set up board
// Chess: white moves first
// Chess: checkmate
```

```java
abstract class Game {
    final void play() { initialize(); startPlay(); endPlay(); }
    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();
}
class Chess extends Game {
    void initialize() { System.out.println("Chess: set up board"); }
    void startPlay() { System.out.println("Chess: white moves first"); }
    void endPlay() { System.out.println("Chess: checkmate"); }
}
new Chess().play();
// Output:
// Chess: set up board
// Chess: white moves first
// Chess: checkmate
```

```csharp
abstract class Game {
    public void Play() { Initialize(); StartPlay(); EndPlay(); }
    protected abstract void Initialize();
    protected abstract void StartPlay();
    protected abstract void EndPlay();
}
class Chess : Game {
    protected override void Initialize() => Console.WriteLine("Chess: set up board");
    protected override void StartPlay() => Console.WriteLine("Chess: white moves first");
    protected override void EndPlay() => Console.WriteLine("Chess: checkmate");
}
new Chess().Play();
// Output:
// Chess: set up board
// Chess: white moves first
// Chess: checkmate
```

### State
In short: let an object behave differently depending on what "mode" it's currently in.

Changes an object's behavior based on internal state by delegating to a state object, avoiding a growing if/else or switch on a status flag as states/transitions multiply. Analogy: a traffic light — what it "does" when it changes depends entirely on which light (state) is currently active.

```ts
interface LightState { next(light: TrafficLight): void; name(): string; }
class RedState implements LightState {
  name(): string { return "Red"; }
  next(light: TrafficLight): void { light.setState(new GreenState()); }
}
class GreenState implements LightState {
  name(): string { return "Green"; }
  next(light: TrafficLight): void { light.setState(new RedState()); }
}
class TrafficLight {
  private state: LightState = new RedState();
  setState(s: LightState): void { this.state = s; }
  change(): void { console.log(`Was ${this.state.name()}`); this.state.next(this); console.log(`Now ${this.state.name()}`); }
}
new TrafficLight().change();
// Output:
// Was Red
// Now Green
```

```java
interface LightState { String name(); void next(TrafficLight light); }
class RedState implements LightState {
    public String name() { return "Red"; }
    public void next(TrafficLight light) { light.setState(new GreenState()); }
}
class GreenState implements LightState {
    public String name() { return "Green"; }
    public void next(TrafficLight light) { light.setState(new RedState()); }
}
class TrafficLight {
    private LightState state = new RedState();
    void setState(LightState s) { state = s; }
    void change() {
        System.out.println("Was " + state.name());
        state.next(this);
        System.out.println("Now " + state.name());
    }
}
new TrafficLight().change();
// Output:
// Was Red
// Now Green
```

```csharp
interface ILightState { string Name(); void Next(TrafficLight light); }
class RedState : ILightState {
    public string Name() => "Red";
    public void Next(TrafficLight light) => light.SetState(new GreenState());
}
class GreenState : ILightState {
    public string Name() => "Green";
    public void Next(TrafficLight light) => light.SetState(new RedState());
}
class TrafficLight {
    private ILightState state = new RedState();
    public void SetState(ILightState s) => state = s;
    public void Change() {
        Console.WriteLine($"Was {state.Name()}");
        state.Next(this);
        Console.WriteLine($"Now {state.Name()}");
    }
}
new TrafficLight().Change();
// Output:
// Was Red
// Now Green
```

### Chain of Responsibility
In short: pass a task down a line of handlers until someone who can deal with it takes it.

Passes a request along a chain of handlers until one of them handles it, so the sender doesn't need to know which handler will ultimately deal with it. Analogy: tech support escalation — L1 tries first, passes to L2 if it can't resolve, L2 passes to L3.

```ts
abstract class SupportHandler {
  protected next?: SupportHandler;
  setNext(handler: SupportHandler): SupportHandler { this.next = handler; return handler; }
  abstract handle(level: number): void;
}
class L1Handler extends SupportHandler {
  handle(level: number): void {
    if (level <= 1) console.log("L1 resolved it");
    else if (this.next) this.next.handle(level);
  }
}
class L2Handler extends SupportHandler {
  handle(level: number): void { console.log("L2 resolved it"); }
}
const l1 = new L1Handler();
l1.setNext(new L2Handler());
l1.handle(2);
// Output: L2 resolved it
```

```java
abstract class SupportHandler {
    protected SupportHandler next;
    SupportHandler setNext(SupportHandler handler) { this.next = handler; return handler; }
    abstract void handle(int level);
}
class L1Handler extends SupportHandler {
    void handle(int level) {
        if (level <= 1) System.out.println("L1 resolved it");
        else if (next != null) next.handle(level);
    }
}
class L2Handler extends SupportHandler {
    void handle(int level) { System.out.println("L2 resolved it"); }
}
L1Handler l1 = new L1Handler();
l1.setNext(new L2Handler());
l1.handle(2);
// Output: L2 resolved it
```

```csharp
abstract class SupportHandler {
    protected SupportHandler next;
    public SupportHandler SetNext(SupportHandler handler) { next = handler; return handler; }
    public abstract void Handle(int level);
}
class L1Handler : SupportHandler {
    public override void Handle(int level) {
        if (level <= 1) Console.WriteLine("L1 resolved it");
        else next?.Handle(level);
    }
}
class L2Handler : SupportHandler {
    public override void Handle(int level) => Console.WriteLine("L2 resolved it");
}
var l1 = new L1Handler();
l1.SetNext(new L2Handler());
l1.Handle(2);
// Output: L2 resolved it
```

### Mediator
In short: have a bunch of objects talk through one go-between instead of directly to each other.

Routes communication between many objects through one central coordinator, instead of wiring each object directly to every other one (which creates a tangled many-to-many mess). Analogy: air traffic control — planes don't coordinate directly with each other, they all go through the tower.

```ts
class ChatRoomMediator {
  private users: User[] = [];
  register(user: User): void { this.users.push(user); }
  send(message: string, from: User): void {
    for (const u of this.users) if (u !== from) u.receive(message, from.name);
  }
}
class User {
  constructor(public name: string, private mediator: ChatRoomMediator) { mediator.register(this); }
  send(message: string): void { this.mediator.send(message, this); }
  receive(message: string, from: string): void { console.log(`${this.name} got from ${from}: ${message}`); }
}
const room = new ChatRoomMediator();
const alice = new User("Alice", room);
const bob = new User("Bob", room);
alice.send("Hi Bob");
// Output: Bob got from Alice: Hi Bob
```

```java
class ChatRoomMediator {
    private List<User> users = new ArrayList<>();
    void register(User user) { users.add(user); }
    void send(String message, User from) {
        for (User u : users) if (u != from) u.receive(message, from.name);
    }
}
class User {
    String name; private ChatRoomMediator mediator;
    User(String name, ChatRoomMediator mediator) { this.name = name; this.mediator = mediator; mediator.register(this); }
    void send(String message) { mediator.send(message, this); }
    void receive(String message, String from) { System.out.println(name + " got from " + from + ": " + message); }
}
ChatRoomMediator room = new ChatRoomMediator();
User alice = new User("Alice", room);
User bob = new User("Bob", room);
alice.send("Hi Bob");
// Output: Bob got from Alice: Hi Bob
```

```csharp
class ChatRoomMediator {
    private List<User> users = new();
    public void Register(User user) => users.Add(user);
    public void Send(string message, User from) {
        foreach (var u in users) if (u != from) u.Receive(message, from.Name);
    }
}
class User {
    public string Name; private ChatRoomMediator mediator;
    public User(string name, ChatRoomMediator mediator) { Name = name; this.mediator = mediator; mediator.Register(this); }
    public void Send(string message) => mediator.Send(message, this);
    public void Receive(string message, string from) => Console.WriteLine($"{Name} got from {from}: {message}");
}
var room = new ChatRoomMediator();
var alice = new User("Alice", room);
var bob = new User("Bob", room);
alice.Send("Hi Bob");
// Output: Bob got from Alice: Hi Bob
```

Gotcha: a mediator that accumulates too much coordination logic becomes a "God object" — the exact many-to-many tangle it was meant to prevent, just centralized in one place instead.

### Memento
In short: save a snapshot of something's state now, so you can restore it later.

Captures and later restores an object's internal state (undo/rollback) without exposing that state's private details to whoever's doing the saving. Analogy: a video game save file — the game can restore your exact progress without you ever seeing the raw save-file bytes.

```ts
class EditorMemento {
  constructor(private readonly content: string) {}
  getContent(): string { return this.content; }
}
class TextEditor {
  private content = "";
  type(text: string): void { this.content += text; }
  save(): EditorMemento { return new EditorMemento(this.content); }
  restore(memento: EditorMemento): void { this.content = memento.getContent(); }
  getText(): string { return this.content; }
}
const editor = new TextEditor();
editor.type("Hello");
const snapshot = editor.save();
editor.type(" World");
editor.restore(snapshot);
console.log(editor.getText());
// Output: Hello
```

```java
class EditorMemento {
    private final String content;
    EditorMemento(String content) { this.content = content; }
    String getContent() { return content; }
}
class TextEditor {
    private String content = "";
    void type(String text) { content += text; }
    EditorMemento save() { return new EditorMemento(content); }
    void restore(EditorMemento memento) { content = memento.getContent(); }
    String getText() { return content; }
}
TextEditor editor = new TextEditor();
editor.type("Hello");
EditorMemento snapshot = editor.save();
editor.type(" World");
editor.restore(snapshot);
System.out.println(editor.getText());
// Output: Hello
```

```csharp
class EditorMemento {
    public string Content { get; }
    public EditorMemento(string content) { Content = content; }
}
class TextEditor {
    private string content = "";
    public void Type(string text) => content += text;
    public EditorMemento Save() => new EditorMemento(content);
    public void Restore(EditorMemento memento) => content = memento.Content;
    public string GetText() => content;
}
var editor = new TextEditor();
editor.Type("Hello");
var snapshot = editor.Save();
editor.Type(" World");
editor.Restore(snapshot);
Console.WriteLine(editor.GetText());
// Output: Hello
```

## Architectural (often lumped in)

These operate at a coarser grain than the patterns above — they organize a whole app's layers rather than a single class relationship — so the treatment here is lighter (concept + when-to-use, code only where it clarifies rather than requiring a full framework scaffold).

### MVC
In short: keep your data, your display, and your input-handling in three separate boxes.

Splits an app into **Model** (data/logic), **View** (display), **Controller** (input handling), so each can change independently — swap the View (web to mobile) without touching business logic, or change data logic without touching how it's displayed. Analogy: a restaurant — kitchen (Model) prepares food, the table/menu (View) presents it, the waiter (Controller) takes your order and relays it between the two. Flow: input reaches the Controller, which updates the Model, which the View then reflects.

### MVVM
In short: like MVC, but the display updates itself automatically whenever the underlying data changes.

Like MVC, but the **View** binds directly to a **ViewModel**'s observable properties (data binding) — no Controller manually pushing updates into the View; the binding framework keeps them in sync automatically. Common in WPF, Angular, Vue, SwiftUI. Analogy: a live spreadsheet formula — change the underlying cell (ViewModel), the display (View) updates itself, nobody manually refreshes it. Cross-ref: this relies on **Observer** under the hood — the View "observes" ViewModel property changes.

### Repository
In short: hide exactly where/how data is stored behind one simple "get me this" method.

An abstraction layer between business logic and data access, so the rest of the app depends on a contract like `UserRepository.findById()`, not on whether that's backed by SQL, a REST API, or an in-memory list. Analogy: a librarian — you ask for a book by title, you don't care which shelf, floor, or building it's actually stored in.

```ts
interface UserRepository { findById(id: number): { id: number; name: string } | null; }
class InMemoryUserRepository implements UserRepository {
  private users = [{ id: 1, name: "Alice" }];
  findById(id: number) { return this.users.find(u => u.id === id) ?? null; }
}
const repo: UserRepository = new InMemoryUserRepository();
console.log(repo.findById(1));
// Output: { id: 1, name: 'Alice' }
```

Swapping `InMemoryUserRepository` for a `SqlUserRepository` later touches nothing that depends on the `UserRepository` interface.

### Dependency Injection
In short: hand a class what it needs from outside, instead of letting it create those things itself.

Supplies a class's dependencies from outside (constructor/setter/parameter) rather than the class `new`-ing them itself — a concrete form of Inversion of Control, and the practical mechanism behind SOLID's Dependency Inversion (see 1-oop.md's SOLID section). Analogy: a lamp doesn't manufacture its own power plant — power is "injected" from the wall socket, so the same lamp works regardless of where the electricity came from.

```ts
interface Database { save(data: string): void; }
class MySQLDatabase implements Database { save(data: string): void { console.log(`Saved "${data}" to MySQL`); } }
class UserService {
  constructor(private db: Database) {} // injected, not `new`'d internally
  register(name: string): void { this.db.save(name); }
}
new UserService(new MySQLDatabase()).register("Alice");
// Output: Saved "Alice" to MySQL
```

`UserService` never mentions `MySQLDatabase` by name in its own body — swap in a `PostgresDatabase` or a test mock without touching `UserService` at all.
