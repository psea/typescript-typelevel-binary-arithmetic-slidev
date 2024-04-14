# Why TypeScript has it?

---

```yaml
layout: two-cols
layoutClass: gap-8
```

## What is JavaScript ?

### Scheme + Self + C + Java = JavaScript

<div v-click class="relative">
<span class="absolute bottom-0 bg-opacity-50 bg-black">When JavaScript joined programming language community</span>
<img src="/lonely-cloun.jpg" class="h-92 rounded shadow" />
</div>

::right::

<v-click>

- Scheme - functions
- Self - prototype based
- C - syntax
- Java - name

<p>New. Fun. Different. Fascinated. Flexible. Weird. Refreshing. Cool. Awkward. Easy-going. </p>
<p>Well suited to put some scripts on a web page</p>
<p class="text-xl">And... very hard to write large scale applications</p>

</v-click>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

## TypeScript

Scheme + Self <span class="opacity-20">+ C + Java</span> = JavaScript

Two very dynamic languages. Both are very flexible and permissive. So as the offspring, JS. Good luck with putting type over it.

<div v-click>
JavaScript + Types
<img src="/brendan.png" class="rounded shadow" />
</div>

::right::

<div v-click>
We got you covered!
<img src="/anders.jpg" class="rounded shadow" />
</div>

---

## TypeScript = **JavaScript** + Types

TypeScript _is_ **JavaScript**.

> TypeScript is **JavaScript** with syntax for types. (ref https://www.typescriptlang.org/)

<v-click>

TypeScript is designed to

- type-check **JavaScript** programs
- provide better language tooling for **JavaScript** programs
- ensure correctness of **JavaScript** programs

</v-click>

<v-click>

TypeScript _is not (meant to be)_

- a new language
- a compiler
- a JavaScript replacer

</v-click>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

Any _other_ statically typed languages (Think Java, Go, C#, Rust, Haskell, etc)

<img src="/correct-typed-programs.png" class="rounded shadow" />

:: right::

- Correct programs - can run and produce expected result
- Typed programs - can be type-checked and compiled

We only can write programs within the boundaries of type system. Type system limits what we can do.

---

Static vs Optional type
<img src="/static-vs-optional-type-lang.png" class="h-full rounded shadow" />

---

Static vs Optional type developer experience
<img src="/static-vs-optional-type-lang-dev.png" class="h-full rounded shadow" />

---

```yaml
layout: two-cols
layoutClass: gap-8
```

<img src="/typescript-dev.png" class="rounded shadow" />

::right::

JS is a highly dynamic, flexible language and people took advantage of that. There is an inherent tension between JS flexibility
and type system rigidity.

**For some _correct_ JS programs we can _not_ provide types.**

In JS you can write all kind of programs, outside what is possible to type.  
TS type system might not be able to capture all possible correct programs.

TS tries to push the boundaries of typed programs.
New TS versions allow to describe correctness of a richer set of correct programs.

TS is a set of compromises.  
Sometimes when trying to expand in one direction you will contract in others.

---

# Is it all roses?

### It is great and unique

- Structural types
- Literal types
- Conditional types
- Recursive generics

<v-click>

### Also complicated!

Are TypeScript types having it’s C++ moment? It becomes unwieldy complicated and unmanageable?

Why they developed such a complex type system?

- The development stems from the JS usage patterns.
- It reflects the flexibility of JS.
- The problem TS is trying to solve - how to statically type a dynamically typed language with a great variety of usage patterns!

</v-click>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

A couple of type definitions from Redux library.

```ts
export type ValidateSliceCaseReducers<
  S,
  ACR extends SliceCaseReducers<S>,
> = ACR & {
  [T in keyof ACR]: ACR[T] extends {
    reducer(s: S, action?: infer A): any;
  }
    ? {
        prepare(...a: never[]): Omit<A, "type">;
      }
    : {};
};

type SliceDefinedCaseReducers<CaseReducers extends SliceCaseReducers<any>> = {
  [Type in keyof CaseReducers]: CaseReducers[Type] extends infer Definition
    ? Definition extends AsyncThunkSliceReducerDefinition<any, any, any, any>
      ? Id<
          Pick<
            Required<Definition>,
            "fulfilled" | "rejected" | "pending" | "settled"
          >
        >
      : Definition extends {
            reducer: infer Reducer;
          }
        ? Reducer
        : Definition
    : never;
};
```

::right::

<p>Libraries are using complex types (both external and internal)</p>
<p>and there are no signs it will become easier</p>

<img src="/nothing-stops-this-train.png" />

<p>We often have to deal with obscure type errors. And it is not fun!</p>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

# Opinion

Advice

<p>Don’t develop in type system. It might seem clever when written but it's the same trap every time</p>

<img src="/who-wrote-that-code.png" class="h-80"/>

::right::

<div v-click class="relative">
  <span class="absolute bottom-0 w-72 text-xl bg-black bg-opacity-50">Me trying to pushing complex type to production code</span>
  <img src="/goat.png" class="h-80"/>
</div>

<div v-after>
  <p class="text-2xl">It doesn’t look good</p>
  <p class="text-2xl">Don’t do it!</p>
</div>

---

# It can be fragile

### `any` keyword, type predicates, assertions, function overloads are **all lies**.

The carefully constructed type having my back while I code.

<img src="/fragile.png" class="h-80"/>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

# You can use types to help yourself

<img src="/help-yourself.png" class="h-80"/>

::right::

# Just don't overuse it. Use common sense

<img src="/help-yourself-2.png" class="h-80"/>

---

# Let the types be with you
