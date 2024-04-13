# Types as a functional programming language

### TypeScript = JavaScript + Types

It _is_ JavaScript, just with added Types.
And Types is a language also. Two for the price of one!

### Type Level Type Script = ~~JavaScript~~ + Types

**TLTS** is a immutable, pure, lazy, functional programming language.

- functional - functions all we have, recursion is our friend (no higher order functions though)
- pure - no input/output
- immutable - we can not re-assign types once they already declared
- lazy - types are evaluated as needed. Opposed to JS eager evaluation.

---

## Values

<img src="values-vs-types.png"/>

---

## Operations

<img src="operations-vs-type-constructors.png"/>

On value-level there are values and operations which operate on those values - take values as parameters and return values.

On type-level there are types and type constructors which operate on those types - take types as parameters and return types.

Operations on types are different from the ones we have in JS. We only have type operations which can make sense on types.

---

## Control structures

<img src="control-structures.png"/>

---

## Value vs type

<img src="value-level-type-level.png" class="h-52" />
<img src="spiderman-value-type.jpg" class="h-52" />

---

### Primitive

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

```yaml
layout: two-cols
layoutClass: gap-16
```

### Type constructors

Makes types out of other types.
If in type world types are values, then type constuctions are operations on types.

Value level:

- values: `42`, `true`, `hello world`
- operations: `+`, `&&`, `===`
  Type level:
- values: `number`, `boolean`, `string`, `true`, `42`, `hello world`
- operations: create tuple, array, record, object, union, intersection

::right::

- Type level
  - objects `{key1: string; key2: boolean}`
  - records `{[key: string]: number}`
  - arrays `number[]`
  - tuples `[string, boolean]`
- union `|`
- intersection `&`
- function type `=>`
  Could think about type operations as data structures for types. We can put other types in and extract them out.

---

### Union

Constructor:

```ts
type T = boolean | string;
```

Access: `???` What does that even mean? Iterate over elements ?
Think as a set of types

### Intersection

Constructor:

```ts
type T = ... & ...
```

Access: `???` What does it even mean? Take original parts ?

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
Concat:

```ts
type E = [...T1, ...T2]; // merge tuples

//Tuples and arrays can be combined in the same constructor.
type T = [number, ...number[]]; // non-empty array of numbers
```

---

### Array

Constructor: `type T = boolean[]`
Access: `type E = T[number]` extract type of an array values
Access: `type E = T[0]` same

---

### Objects

Constructor: `type T = { key1: string; key2: boolean }`
Access: `type E = T['key1' | 'key1'] // string | boolean` // get property types
Access: `type E = keyof T // 'key1' | 'key2'` get property keys type
Access: `type E = T[keyof T]` shortcut to get all possible property keys type

---

### Records

Constructor: `type T = { [key: string]: boolean }`, `Record<Keys, Type>`, `Record<K, V> = { [Key in K]: V }`
Access: `type E = T[string]` // get property value type. Not possible Object constructor
Access: `type E = keyof T` // get property keys type

---

### Function type

Constructor:

```ts
type T = (input: number) => boolean;
```

Access:

```ts
type P = Parameters<T>`, `type R = ReturnType<T>
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

Absolute unique. Togeter with conditional recursive types we can iterate over strings.

```ts twoslash
type MakeMeKebab<S> = S extends `${infer First} ${infer Rest}`
  ? `${First}-${MakeMeKebab<Rest>}`
  : S;

type E = MakeMeKebab<"tomato cheese anchovy cheese">;
```

be amaized: https://github.com/codemix/ts-sql

## Control structures

- Functions

- Conditionals (if-then expressions)

- Pattern matching

- Recursion (loops)

---

### Functions

Generic types are equivalent of function calls.

TLTS functions are generic types. They take types as inputs and values as outputs.

```ts
// value level
const myFunc = (a, b) => a + b;
```

```ts
type Fn<A, B> = A | B;

// functions might have defaults
type Fn<A, B = null> = [A, B];
```

Type constraints. It's possible to specify a type of an input. A type of a type?!

```ts
type Fn<A extends string, B = null> = { [Key in A]: B };
```

Here `A extends string` defines what `A` could be - `A` should be assignable to `string`.  
(Plays similar role to classes constraints in Haskell.)

---

### Conditionals (if-then expressions)

Conditional type: `type X = A extends B ? T : F`
example:`type T = A extends B ? true : false`

Since type-level typescript is a functional programming language, there are no conditional statements,
only conditional type expressions - each type constructor returns a type.

Equivalent JS conditional expression: `let x = A <= B ? true : false`

The `extends` keyword defins A and B subtype relationship - A is a subtype of B.
We can imagine it as
`type T = A is a subtype of B ? true : false`
or
`type T = A is a subset of B ? true : false`
or
`type T = A is assignable to B ? true : false`
or anyother synomym to it
A is a subtype of B (B is a supertype of A) <- A formal definition
A is a subset of B (a set of values of A is a subset of values of B. B includes all values of A)
A is "smaller" then B
A implements B
A extends B
A can be assigned to B
A can be used where B is expected
A can be used in place of B <- a formal meaning of a formal definition
A <: B, A ⊑ B, A ≤: B, A ⊆ B, A -> B

A caveat: In `type T = never extends number ? true : false`, conditional type will be evaluated to `never`.
Even though `never` is a subtype of `number` it is treated specially,
and conditional with `never` on the left of `extends` will always be evaluated to `never`.

Example:
`type If<A extends boolean, T, F> = A extends true ? T : F`
`type E = If<true, 42, string> // 42`

Here `A extends boolean` in a parameter of generic type defines what `A` could be - `A` should be assignable to `boolean`.
It is type constraint (plays similar role to classes constraints in Haskell).
Are they the types for the types?

Constraints are helpful to force TS to infer subtypes of a type parameter - forcing type narrowing.

```ts
const asObj = <S>(value: S) => ({ value });
const obj = asObj("hello"); // { value: string }
```

```ts
const asObj = <S extends string>(value: S) => ({ value });
const obj = asObj("hello"); // { value: 'hello' }
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

### Pattern matching

I told you, TLTS is a functional programming language.

TLTS has pattern matching. Ironically JS itself still doesn't.

Pattern matching: Checking if type conforms to some pattern (another type) and deconstructing the type according to the pattern.
Allows to extract the values from type constructors.

Pattern match `T` against pattern with shape `{ id: string, status: _something_ }`

```ts
type GetStatus<T> = T extends { id: string; status: infer S } ? S : never;
type E = GetStatus<{ id: "abc"; status: "ok" }>; // E = 'ok'
```

Pattern match `T` against a tuple.

```ts
type Head<T> = T extends [infer H, ...unknown[]] ? H : never;
type Last<T> = T extends [...unknown[], infer Last] ? Last : never;

type E1 = Head<[1, 2]>; // 1
type E2 = Last<[1, 0, 2]>; // 2
```

Functions are type constructors also, so pattern match works

```ts
type FromFunc<T> = T extends (arg: infer A) => infer R ? { input: A, output R} : never
type E = FromFunc<(i: string) => number> // { input: string; output: string }
```

Works with user-defined type constructors aka Generic Types

```ts
type Thing<Id, Name> = { id: Id; name: Name }

// Pattern matching against Thing generic type
type GetId<T> = T extends Thing<infer Id, infer Name> ? Id : never;

type E = GetId<Thing<string, number> // string
```

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

Trick to constrain an inferred type.

```ts
type FirstIfString<T> = T extends [infer S extends string, ...unknown[]]
  ? S
  : never;
```

`infer S extends string` matches against S, it also ensures that S has to be a string. If S isn’t a string, it takes the false path, which in these cases is never.

---

### Recursion (loops)

Recursive generic types + conditional types (aka Recursive Conditional Types)

```ts
type FnRec<Input> = // recursive function
  Input extends Some // condition
    ? FnRec<DoSomething<Input>> // recursive case
    : BaseCase; // base case
```

Side note: Following functional language tradition TypeScript performs Tail-Call Optimization on Recursive Conditional Types 🤯️

JavaScript itself still don't

Looping over lists

```ts
type Fn<List> = List extends [infer First, ...infer Rest] ? Fn<Rest> : BaseCase;
```

Example

```ts
type Contains<List, X> = List extends [infer First, ...infer Rest]
  ? First extends X
    ? true
    : Contains<Rest, X>
  : false;
```

---

## Lazy evaluated

```ts
halt = (c) => (c ? "42" : halt(c));
halt(true); // evaluates to ‘42’
halt(false); // goes infinite

ifFunc = (c, t, f) => (c ? t : f);
ifFunc(true, "yes", "no"); // return yes
ifFunc(true, "yes", halt(false)); // the answer is 'yes' but we never get it

type Halt<C> = C extends true ? "42" : Halt<C>;

type If<C, T, F> = C extends true ? T : F;

type Eval = If<true, "yes", Halt<false>>; // this evaluates to 'yes', cause TS types are lazy evaluated
```

<img src="/party.png" class="h-40 rounded shadow" />
