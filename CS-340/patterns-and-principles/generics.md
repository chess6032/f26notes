# Polymorphism with Generics

A ***generic type parameter*** ("generic" for short) is a placeholder type that is later defined upon usage. e.g., `template <class T>` in C++.

Generics are defined by adding `<T>` after a function's/class's/whatever's name (where `T` would then be the placeholder type). For arrow functions, you put the angle brackets right before the parenthesized list of parameters.

## Generics in TypeScript

```ts
// generics in type script

class GenericClass<T> { /* ... */ }

interface GenericInterface<T> { /* ... */ };

type GenericAlias<T> = { /* ... */ };

function genericFunction<T>(…) { /* ... */}

const genericArrowFunc = <T>(…) => { /* ... */ };
```

Separate multiple types with commas. e.g., `<T, U, V>`.

> [!WARNING]
> If you're defining a generic arrow function in a `.tsx` file, you have to ***include a comma** or else it will be interpreted as JSX*.
>
> ```ts
> const bad  = <T>(…) => … ;  // <T> is interpreted as a JSX tag. Throws error.
> const good = <T,>(…) => … ; // <T,> is correctly interpreted as a generic.
> ```

### Inferred bindings on generic function calls

TypeScript can *infer* the type argument for function calls.

```ts
// Plain function declaration
function first<T>(arr: T[]): T { return arr[0]; }
first([1, 2, 3]);        // T inferred as number

// Arrow function
const wrap = <T,>(x: T) => ({ value: x });
wrap('hi');              // T inferred as string

// Generic class constructor
class Box<T> { constructor(public item: T) {} }
new Box(42);             // Box<number>, T inferred from the argument

// Built-in library methods
[1, 2, 3].map(n => String(n));   // map<U> infers U as string
Promise.resolve(true);           // Promise<boolean>
new Map([['a', 1]]);             // Map<string, number>
```

This only works for *function calls*. If you reference a *type* that uses a generic, inference doesn't apply. (e.g., when declaring a variable of type `Box<T>`, you have to supply a type for `T`.)

## Naming conventions

* `T`, then `U`, `V`, `W`, etc.
  * Some weirdos use `A`, `B`, `C`, etc. for their likeness to Greek letters $\alpha$, $\beta$, $\zeta$ that you'd see in math proofs.
* If using lots of generics or using them in a complicated way, consider naming them.
