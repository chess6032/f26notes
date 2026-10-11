# Polymorphism with Generics

A ***generic type parameter*** ("generic" for short) is a placeholder type that is later defined upon usage. e.g., `template <class T>` in C++.

## Naming conventions

* `T`, then `U`, `V`, `W`, etc.
  * Some weirdos use `A`, `B`, `C`, etc. for their likeness to Greek letters $\alpha$, $\beta$, $\zeta$ that you'd see in math proofs.
* If using lots of generics or using them in a complicated way, consider naming them.

## Bounded polymorphism

Sometimes, "this thing of some type `T`" isn't enough, and you need "this thing is of some type `U` that is *at least `U`*." For example, suppose you want a generic, but you want it to be an instance of a class or its subclasses so that you can safely call one of its methods.

Many languages support a way to enforce that constraint (e.g., in TypeScript, a `<U extends T>` generic requires that whatever you use as `U` is an instance/implementation of `T`). That constraint is called an *upper bound*. (Not sure if "lower bound" refers to anything.)
