# Creational Design Patterns

Creational design patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. They can be divided into class-creation patterns and object-creational patterns.
These patterns can be further categorized into:

## 1. Singleton Pattern
### What is it? 
The Singleton Pattern ensures that a class has only one instance and provides a global point of access to it. It is often used for logging, driver objects, caching, thread pools, and database connections.

### When to use it?
- When exactly one object is needed to coordinate actions across the system.
### Types of Singleton Pattern  
1. **Eager Initialization**: The instance is created at the time of class loading.
2. **Lazy Initialization**: The instance is created when it is requested for the first time.
3. **Double-Checked Locking**: A more efficient way to implement a thread-safe singleton by reducing the overhead of acquiring a lock.
4. **Synchronized Method**: A simple way to ensure that only one thread can access the method that creates the instance.

#### Eager Initialization Example
```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {
        // private constructor to prevent instantiation
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

#### Lazy Initialization Example
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // private constructor to prevent instantiation
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
#### Double-Checked Locking Example
```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {
        // private constructor to prevent instantiation
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
#### Synchronized Method Example
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // private constructor to prevent instantiation
    }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## 2. Factory Method Pattern
### What is it?
The Factory Method Pattern defines an interface for creating an object, but lets subclasses alter the type of objects that will be created. It allows a class to defer instantiation to subclasses.

### When to use it?
- When a class cannot anticipate the class of objects it must create.

### Example
```java
public abstract class Product {
    public abstract void use();
}

public class ConcreteProductA extends Product {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductA");
    }
}

public class ConcreteProductB extends Product {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductB");
    }
}

public abstract class Creator {
    public abstract Product factoryMethod();

    public void someOperation() {
        Product product = factoryMethod();
        product.use();
    }
}

public class ConcreteCreatorA extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

public class ConcreteCreatorB extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}

public class Client {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        creatorA.someOperation(); // Using ConcreteProductA

        Creator creatorB = new ConcreteCreatorB();
        creatorB.someOperation(); // Using ConcreteProductB
    }
}
```

## 3. Abstract Factory Pattern
### What is it?
The Abstract Factory Pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. It allows for the creation of objects that belong to a specific family.

### When to use it?
- When you need to create families of related or dependent objects without specifying their concrete classes.

### Example
```java

public interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

public class ConcreteFactory1 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}   

public class ConcreteFactory2 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}

public interface ProductA {
    void use();
}

public interface ProductB {
    void use();
}

public class ConcreteProductA1 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductA1");
    }
}

public class ConcreteProductA2 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductA2");
    }
}

public class ConcreteProductB1 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductB1");
    }
}

public class ConcreteProductB2 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductB2");
    }
}

public class Client {
    public static void main(String[] args) {
        AbstractFactory factory1 = new ConcreteFactory1();
        ProductA productA1 = factory1.createProductA();
        ProductB productB1 = factory1.createProductB();
        productA1.use(); // Using ConcreteProductA1
        productB1.use(); // Using ConcreteProductB1

        AbstractFactory factory2 = new ConcreteFactory2();
        ProductA productA2 = factory2.createProductA();
        ProductB productB2 = factory2.createProductB();
        productA2.use(); // Using ConcreteProductA2
        productB2.use(); // Using ConcreteProductB2
    }
}
```

## 4. Builder Pattern
### What is it?
The Builder Pattern is a creational design pattern that allows for the step-by-step construction of complex objects. It separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

### When to use it?
- When the construction process of an object is complex and should be independent of the parts that make up the object and how they are assembled.

### Example
```java
public class Product {
    private String partA;
    private String partB;
    private String partC;

    public void setPartA(String partA) {
        this.partA = partA;
    }

    public void setPartB(String partB) {
        this.partB = partB;
    }

    public void setPartC(String partC) {
        this.partC = partC;
    }

    @Override
    public String toString() {
        return "Product{" +
                "partA='" + partA + '\'' +
                ", partB='" + partB + '\'' +
                ", partC='" + partC + '\'' +
                '}';
    }
}

public interface Builder {
    void buildPartA();
    void buildPartB();
    void buildPartC();
    Product getResult();
}

public class ConcreteBuilder implements Builder {
    private Product product;

    public ConcreteBuilder() {
        this.product = new Product();
    }

    @Override
    public void buildPartA() {
        product.setPartA("Part A");
    }

    @Override
    public void buildPartB() {
        product.setPartB("Part B");
    }

    @Override
    public void buildPartC() {
        product.setPartC("Part C");
    }

    @Override
    public Product getResult() {
        return product;
    }
}       

public class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    public void construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();
    }
}

public class Client {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.getResult();
        System.out.println(product); // Product{partA='Part A', partB='Part B', partC='Part C'}
    }
}
```

## 5. Prototype Pattern
### What is it?
The Prototype Pattern is a creational design pattern that allows for the creation of new objects by copying an existing object, known as the prototype. It is useful when the cost of creating a new object is more expensive than copying an existing one.

### When to use it?
- When the classes to instantiate are specified at runtime, for example, through dynamic loading.

### Example
```java
public abstract class Prototype {
    public abstract Prototype clone();
}

public class ConcretePrototype extends Prototype {
    private String field;

    public ConcretePrototype(String field) {
        this.field = field;
    }

    @Override
    public Prototype clone() {
        return new ConcretePrototype(field);
    }

    @Override
    public String toString() {
        return "ConcretePrototype{" +
                "field='" + field + '\'' +
                '}';
    }
}
public class Client {
    public static void main(String[] args) {
        ConcretePrototype prototype = new ConcretePrototype("Original");
        ConcretePrototype clone = (ConcretePrototype) prototype.clone();
        System.out.println(prototype); // ConcretePrototype{field='Original'}
        System.out.println(clone); // ConcretePrototype{field='Original'}
    }
}
```

## Summary
Creational design patterns provide various mechanisms to create objects in a flexible and reusable manner. They help in managing object creation complexity, ensuring that the system remains scalable and maintainable. Understanding these patterns is crucial for software developers to design robust applications that can adapt to changing requirements.

## Conclusion
Creational design patterns are essential for managing object creation in software development. They provide a structured approach to instantiate objects, ensuring that the system is flexible, maintainable, and scalable. By understanding and applying these patterns, developers can create robust applications that can easily adapt to future changes and requirements.

## Further Reading
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612) by Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides
- [Head First Design Patterns](https://www.amazon.com/Head-First-Design-Patterns-Brain-Friendly/dp/0596007124) by Eric Freeman, Bert Bates, Kathy Sierra, Elisabeth Robson
- [Refactoring Guru - Design Patterns](https://refactoring.guru/design-patterns) - A comprehensive resource for understanding design patterns with examples in multiple programming languages.
- [SourceMaking - Design Patterns](https://sourcemaking.com/design_patterns) - A detailed guide to design patterns with explanations and examples.
- [GeeksforGeeks - Design Patterns](https://www.geeksforgeeks.org/design-patterns-set-1-introduction/) - An introduction to design patterns with examples in Java.


## Related Topics
- [Structural Design Patterns](design-patter/structural-design-pattern.md)
- [Behavioral Design Patterns](design-pattern/behavioral-design-pattern.md)