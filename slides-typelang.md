# TypeLang

---

### _TypeLang_ values - types

- Primitive types

```ts
type B = boolean;
type S = string;
type X = Array<number>;
```

- _Literal types_. TypeScript unique ?

```ts
// literal values
const someNum = 37;
const s = "hello";

// literal types
type SomeNum = 37;
type S = "hello";
```

---

### _TypeLang_ Operations - Type constructors

- union `|`
- intersection `&`
- arrays `number[]`
- tuples `[string, boolean]`
- objects `{key1: string; key2: boolean}`
- records `{[key: string]: number}`
- function type `=>`

  Could think about type operations as data structures for types. We can put other types in and extract them out.

---

### Union

Constructor:

```ts
type T = boolean | string;
```

<hr class="m-4" />

### Intersection

Constructor:

```ts twoslash
type Signal = "red" | "yellow" | "green";
type Ok = "green" | "white";

type T = Signal & Ok;
```

---

### Tuples

Constructor:

```ts
type T = [boolean, string];

type T = [first: boolean, second: string]; //tuple elements could have names
type T = [boolean, string?]; //tuple elements could be optional
```

Access:

```ts
type E = T[0 | 1]; // extracting type of individual elements
type E = T[number]; // of all elements
type E = T["length"]; // get tuple length
```

Operations on tuples:
Concatenate:

```ts
type E = [...T1, ...T2]; // merge tuples

//Tuples and arrays can be combined in the same constructor.
type T = [number, ...number[]]; // non-empty array of numbers
```

---

### Array

Constructor:

```ts
type T = boolean[];
```

Access:

```ts
type E = T[number]; //extract type of an array values
type E = T[0]; // same
```

---

### Objects

Constructor:

```ts
type T = { key1: string; key2: boolean };
```

Access:

```ts twoslash
type T = { key1: string; key2: boolean };

type E1 = T["key1" | "key2"]; // get property types
type E2 = keyof T; // get property keys type
type E3 = T[keyof T]; // shortcut to get all possible property keys type
```

---

### Records

Constructor:

```ts
type T = { [key: string]: boolean }

type T = Record<Keys, Type>
type T = Record<K, V> = { [Key in K]: V }
```

Access:

```ts
type E = T[string]; // get property value type. Not possible Object constructor
type E = keyof T; // get property keys type
```

---

### Function type

Constructor:

```ts
type T = (input: number) => boolean;
```

Access:

```ts
type P = Parameters<T>;
type R = ReturnType<T>;
```

---

### Template type literals

Constructor:

```ts twoslash
type A = "hello";
type B = 37;
type T = `${A} foo ${B}`;
```

Access by pattern matching

```ts twoslash
type E = `a foo 37` extends `${infer A} foo ${infer B extends number}`
  ? [A, B]
  : never;
```

Absolute unique. Together with conditional recursive types we can iterate over strings.

```ts twoslash
type MakeMeKebab<S> = S extends `${infer First} ${infer Rest}`
  ? `${First}-${MakeMeKebab<Rest>}`
  : S;

type E = MakeMeKebab<"tomato cheese anchovy cheese">;
```

be amazed: https://github.com/codemix/ts-sql

---

## _TypeLang_ Control structures

- Functions

- Conditionals (if-then expressions)

- Pattern matching

- Recursion (loops)

---

### _TypeLang_ Functions

Generic types are equivalent of function calls.

_TypeLang_ functions are generic types. They take types as inputs and produce types as outputs.

```ts
// value level
const myFunc = (a, b) => a + b;

const x = myFunc(1, 2);
```

```ts twoslash
// type level
type MyFn<A, B> = A | B;

type X = MyFn<1, 2>;
```

---

### _TypeLang_ Functions

#### Defaults.

```ts twoslash
type Fn<A, B = null> = [A, B];
```

<hr class="m-4" />

#### Type constraints

```ts
type Fn<A extends string, B = null> = { [Key in A]: B };
```

It's possible to specify a type of an input. A type of a type?!

Here `A extends string` defines what `A` could be - `A` should be assignable to `string`.  
(Plays similar role to classes constraints in Haskell.)

Constraints are also helpful to force TS to infer subtypes of a type parameter - forcing type narrowing.

```ts twoslash
//without constraint
const asObj = <S>(value: S) => ({ value });
const obj = asObj("hello");
```

```ts twoslash
// with constraint
const asObj = <S extends string>(value: S) => ({ value });
const obj = asObj("hello");
```

---

### _TypeLang_ Conditionals (if-then expressions)

Conditional type:

```ts
type X = A extends B ? T : F;
```

example:

```ts twoslash
type A = "hello";
type B = string;

type X = A extends B ? true : false;
```

Equivalent JS conditional expression:

```ts
const x = A <= B ? true : false;
```

Since type-level typescript is a functional programming language, there are no conditional statements,
only conditional type expressions - each type constructor returns a type.

---

### _TypeLang_ Condition checks for a subtype relationship

```ts
type X = A extends B ? T : F;
```

The `A extends B` defines A and B subtype relationship - A is a subtype of B.

```ts
// NOT REAL SYNTAX. MEANING OF THE `extends` KEYWORD
type T = A is a subtype of B ? true : false
type T = A is a subset of B ? true : false
type T = A is assignable to B ? true : false
```

---

### What is a subtype

- `A` is a subtype of `B` (`B` is a supertype of `A`) _A formal definition_
- `A` can be used in place of `B` _a formal meaning of a formal definition_
- `A` is a subset of `B` (a set of values of `A` is a subset of values of `B`. `B` includes all values of `A`)
- `A` is "smaller" then `B`
- `A` implements `B`
- `A` extends `B`
- `A` can be assigned to `B`
- `A` can be used where B is expected
- `A <: B`, `A ⊑ B`, `A ≤: B`, `A ⊆ B`, `A -> B`

---

### Special cases

A caveat:

```ts
type T = never extends number ? true : false;
```

conditional type will be evaluated to `never`.
Even though `never` is a subtype of `number` it is treated specially,
and conditional with `never` on the left of `extends` will always be evaluated to `never`.

Example:

```ts twoslash
type If<A extends boolean, T, F> = A extends true ? T : F;
type E = If<true, 42, string>; // 42
```

---

### IsEqual

`A extends B` define subtype relationship. Not Equality.
TypeScript doesn't provide a way to check if two types are equal.
To check if two types are equal we need to check if

`A` is a subtype of `B`  
and  
`B` is a subtype for `A`

```ts
type Extends<A, B> = A extends B ? true : false;

type Equal<A, B> = [Extends<A, B>, Extends<B, A>] extends [true, true]
  ? true
  : false;
```

Approximation, still wrong on some cases.

---

### _TypeLang_ Pattern matching

TypeLang has pattern matching. Ironically JS itself still doesn't.

Pattern matching: Checking if type conforms to some pattern (another type) and deconstructing the type according to the pattern.
Allows to extract the values from type constructors.

#### Patter match object

```ts twoslash
// Pattern match `T` against pattern with shape `{ id: string, status: _something_ }`
type GetStatus<T> = T extends { id: string; status: infer S } ? S : never;

type E = GetStatus<{ id: "abc"; status: "ok" }>;
```

---

### _TypeLang_ Pattern matching

#### Patter match tuple

```ts twoslash
// Pattern match `T` against a tuple.
type Head<T> = T extends [infer H, ...unknown[]] ? H : never;
type Last<T> = T extends [...unknown[], infer Last] ? Last : never;

type E1 = Head<[1, 2]>;
type E2 = Last<[1, 0, 2]>;
```

---

### _TypeLang_ Pattern matching

#### Patter match function

Functions are type constructors also, so pattern match works

```ts twoslash
type FromFunc<T> = T extends (arg: infer A) => infer R
  ? { input: A; output: R }
  : never;

type E = FromFunc<(i: string) => number>;
```

---

### _TypeLang_ Pattern matching

#### Patter match user-defined type constructor

Works with user-defined type constructors aka Generic Types

```ts
type Thing<Id, Name> = { id: Id; name: Name }

// Pattern matching against Thing generic type
type GetId<T> = T extends Thing<infer Id, infer Name> ? Id : never;

type E = GetId<Thing<string, number> // string
```

---

### _TypeLang_ Pattern matching

A trick to get intermediate results from a computation

```ts
// instead of
type Fn<I> = DoSomething<Calc<I>> & // calling Calc twice here
  DoSomethingElse<Calc<I>>; // and here

// Optimizing type level performance 🤯
// we can use "assign" calculation to intermediate variable
type Fn<X> =
  Calc<X> extends infer Y // get Y from calculation
    ? DoSomething<Y> & DoSomethingElse<Y> // use Result
    : never;
```

Equivalent of Haskell

```haskell
f x =
   let y = ... x ...
   in  ... y ...
```

---

### _TypeLang_ Pattern matching

Trick to constrain an inferred type.

```ts
type FirstIfString<T> = T extends [infer S extends string, ...unknown[]]
  ? S
  : never;
```

`infer S extends string` matches against `S`,
it also ensures that `S` has to be a string. If `S` isn’t a string, it takes the false path, which in these cases is never.

---

### _TypeLang_ Recursion (loops)

Recursive generic types + conditional types (aka Recursive Conditional Types)

```ts
type FnRec<Input> = // recursive function
  Input extends Some // condition
    ? FnRec<DoSomething<Input>> // recursive case
    : BaseCase; // base case
```

Side note: Following functional language tradition TypeScript performs Tail-Call Optimization on Recursive Conditional Types 🤯️

JavaScript itself still don't

---

### _TypeLang_ Recursion (loops)

#### Looping over lists

```ts
type Fn<List> = List extends [infer First, ...infer Rest] ? Fn<Rest> : BaseCase;
```

Example

```ts twoslash
type Contains<List, X> = List extends [infer First, ...infer Rest]
  ? First extends X
    ? true
    : Contains<Rest, X>
  : false;

type E1 = Contains<[1, "two", false, 4], 4>;
type E2 = Contains<[1, "two", false, 4], boolean>;
type E3 = Contains<[1, "two", false, 4], 37>;
```

---

## _TypeLang_ is Lazy

```ts
halt = (c) => (c ? 42 : halt(c));
halt(true); // evaluates to ‘42’
halt(false); // goes infinite

ifFunc = (c, t, f) => (c ? t : f);
ifFunc(true, "yes", "no"); // return yes
ifFunc(true, "yes", halt(false)); // the answer is 'yes' but we never get it

type Halt<C> = C extends true ? 42 : Halt<C>;

type If<C, T, F> = C extends true ? T : F;

type Eval = If<true, "yes", Halt<false>>; // this evaluates to 'yes', cause TS types are lazy evaluated
```
