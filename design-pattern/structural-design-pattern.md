# Structural Design Patterns
Structural design patterns are concerned with how classes and objects are composed to form larger structures. They help ensure that if one part of a system changes, the entire system doesn't need to do the same. Here are some common structural design patterns:

## 1. Adapter Pattern
### what is it?
The Adapter Pattern allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces.

### when to use it?
When you want to use a class that has an incompatible interface with the one you need, or when you want to create a reusable class that can work with different interfaces.

### example
```Java
// Target interface
public interface Target {
    void request();
}

// Adaptee class
public class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request");
    }
}

// Adapter class
public class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}

// Client code
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request(); // Output: Specific request
    }
}
```
## 2. Bridge Pattern
### what is it?
The Bridge Pattern separates an interface from its implementation, allowing the two to vary independently. It is used to decouple an abstraction from its implementation so that both can evolve independently.

### when to use it?
When you want to avoid a permanent binding between an abstraction and its implementation, or when you want to allow the abstraction and implementation to evolve independently.

### example
```Java
// Abstraction
public abstract class Abstraction {
    protected Implementor implementor;

    public Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    public abstract void operation();
}

// Implementor interface
public interface Implementor {
    void operationImpl();
}

// Concrete Implementor
public class ConcreteImplementorA implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementorA operation");
    }
}

// Concrete Implementor
public class ConcreteImplementorB implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementorB operation");
    }
}

// Refined Abstraction
public class RefinedAbstraction extends Abstraction {
    public RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    @Override
    public void operation() {
        implementor.operationImpl();
    }
}

// Client code
public class Client {
    public static void main(String[] args) {
        Implementor implementorA = new ConcreteImplementorA();
        Abstraction abstractionA = new RefinedAbstraction(implementorA);
        abstractionA.operation(); // Output: ConcreteImplementorA operation

        Implementor implementorB = new ConcreteImplementorB();
        Abstraction abstractionB = new RefinedAbstraction(implementorB);
        abstractionB.operation(); // Output: ConcreteImplementorB operation
    }
}
```

