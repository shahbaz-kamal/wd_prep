# 2. Design Patterns

**Creational**
- **Singleton** — one instance, global access. (Logger, DB connection pool.)
- **Factory Method** — subclass decides which class to instantiate.
- **Abstract Factory** — factory of related factories; families of objects.
- **Builder** — step-by-step construction of a complex object.
- **Prototype** — create by cloning an existing instance.

**Structural**
- **Adapter** — converts one interface into another. (Wrapping a legacy API.)
- **Decorator** — adds behaviour at runtime by wrapping. (Express middleware.)
- **Facade** — simplified front over a complex subsystem.
- **Proxy** — placeholder controlling access (lazy loading, access control, caching).
- **Composite** — treat individual objects and groups uniformly (tree structures).
- **Bridge** — decouple abstraction from implementation so both can vary.
- **Flyweight** — share common state across many objects to save memory.

**Behavioural**
- **Observer** — one-to-many notification on state change. (Event emitters, pub/sub.)
- **Strategy** — interchangeable algorithms selected at runtime.
- **Command** — encapsulate a request as an object (undo/redo, queues).
- **Iterator** — sequential access without exposing the underlying structure.
- **Template Method** — skeleton in base class, steps overridden by subclass.
- **State** — object changes behaviour when internal state changes.
- **Chain of Responsibility** — pass request along a chain of handlers.
- **Mediator** — central object coordinating communication between components.
- **Memento** — capture and restore an object's state.

**Architectural (often lumped in)**
- **MVC** — Model (data/logic), View (UI), Controller (input handling).
- **MVVM** — View binds to ViewModel via data binding.
- **Repository** — abstraction layer over data access.
- **Dependency Injection** — supplying dependencies from outside (a form of IoC).
