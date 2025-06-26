# Behavioural Design Patterns
Behavioural design patterns are concerned with how objects interact and communicate with each other. They help in defining clear communication patterns between objects, making the system more flexible and easier to maintain.

## 1. Chain of Responsibility
### What is it?
The Chain of Responsibility pattern allows an object to pass a request along a chain of handlers. Each handler can either process the request or pass it to the next handler in the chain.

### When to use it?
When you have multiple objects that can handle a request, but you don't know which one will handle it. It decouples the sender of the request from the receiver.

### Example
```java
abstract class Handler {
    protected Handler next;

    public void setNext(Handler next) {
        this.next = next;
    }

    public abstract void handleRequest(String request);
}

class ConcreteHandlerA extends Handler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("A")) {
            System.out.println("Handler A processed the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

class ConcreteHandlerB extends Handler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("B")) {
            System.out.println("Handler B processed the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

class Client {
    public static void main(String[] args) {
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();
        
        handlerA.setNext(handlerB);
        
        handlerA.handleRequest("A"); // Output: Handler A processed the request.
        handlerA.handleRequest("B"); // Output: Handler B processed the request.
        handlerA.handleRequest("C"); // No output, as no handler processes "C"
    }
}
```