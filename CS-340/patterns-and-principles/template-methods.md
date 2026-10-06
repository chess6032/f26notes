# Template Methods

BIG IDEA: Define the skeleton of an operation in a base class, deferring some of its steps to the subclass. This way, subclasses can redefine certain steps of the operation *without changing the operation's structure*.

Template methods use an *inverted* control structure: a **base class calls its *sub*-class's methods**, not the other way around. This is sometimes referred to as "The Hollywood Principle," that is, "Don't call us, we'll call you."

<!-- ```ts
abstract class Base {
    protected abstract subOp(): any;

    public templateMethod() {
        // ...
        this.subOp();
        // ...
    }
}

class Sub extends Base {
    protected override subOp() {
        // ...
    }
}

const thing = new Sub;
thing.templateMethod();
``` -->

![Template Method](./template-method.svg)

## Example

(TODO)

## Kinds of ops template methods call

* Concrete operations 
  * (either on the ConcreteClass or on client classes).
* Concrete AbstractClass operations 
  * (i.e., operations that are generally useful to subclasses).
* Primitive operations 
  * (i.e., abstract operations).
  * MUST be override in subclasses.
* Factory methods.
* Hook operations.
  * Provide a default behavior that subclasses can extend if necessary.
  * (Often does nothing by default.)
  * Not required to be overriden by subclasses.

> [!TIP] 
> It's important for template methods to **specify which operations are hooks** (*may* be overriden) and which are **abstract operations** (*must* be overriden).

