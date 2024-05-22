# How is it done?

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

> TypeScript becomes JavaScript via the delete key. (ref https://www.typescriptlang.org/)

</v-click>

---

# Types as a functional programming language

## JavaScript + Type System = TypeScript

<v-click>

## <span class="opacity-20">~~JavaScript~~</span> + Type System = Type-level TypeScript

</v-click>

<div v-click class="flex gap-4">
<div>

**TypeLang** - the language of types.

**TypeLang** is an immutable, pure, lazy, functional programming language.

- functional - functions all we have, recursion is our friend (no higher order functions though)
- pure - no input/output
- immutable - we can not re-assign types once they already declared
- lazy - types are evaluated as needed. Opposed to JS eager evaluation.

</div>

<div class="relative min-w-[50%]">
  <img src="/can-not-hide.jpg" class="w-96" />
  <span class="absolute bottom-0 bg-black bg-opacity-80">Microsoft shipping full-blown programming language in type system, hoping no one notice.</span>
</div>

</div>

---

## **TypeLang** Values

### Things the language can make use of

<img src="/values-vs-types.png" />

---

## **TypeLang** Operations

### A set of available operations defines what we can do with values[]

<img src="/operations-vs-type-constructors.png"/>

On value-level there are values and operations which operate on those values - take values as parameters and return values.  
On type-level there are types and type constructors which operate on those types - take types as parameters and return types.  
Operations on types are different from the ones we have in JS. We only have type operations which can make sense on types.

---

## **TypeLang** Control structures

<img src="/control-structures.png"/>

---

## Value-level (**JavaScript**) _vs_ Type-level (**TypeLang**)

<img src="/value-level-type-level.png" class="h-52" />
<img src="/spiderman-value-type.jpg" class="h-52" />
