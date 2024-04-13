# Type level binary arithmetics

<img src="party.png" class="h-58" />

---

## Binary arithmetics with logic gates

<div class="flex gap-4">
  <div><img src="bits-add.png" class="h-32" /></div>
  <div v-click><img src="bin-add.png" class="h-32" /></div>
</div>

<div class="flex">
  <div v-click class="absolute bg-black"><img src="bin-add-step-0.png" class="h-40" /></div>
  <div v-click class="absolute bg-black"><img src="bin-add-step-1.png" class="h-40" /></div>
  <div v-click class="absolute bg-black"><img src="bin-add-step-2.png" class="h-40" /></div>
  <div v-click class="absolute bg-black"><img src="bin-add-step-3.png" class="h-40" /></div>
  <div v-click class="absolute bg-black"><img src="bin-add-step-4.png" class="h-40" /></div>
</div>

---

```yaml
layout: two-cols
```

## Half adder

<div class="flex flex-col gap-4">
  <div><img src="half-adder-table.png" class="h-32" /></div>
  <div><img src="half-adder-gates.png" class="h-32"/></div>
  <div><img src="half-adder.png" class="h-32"/></div>
</div>

::right::

<v-click>
```ts
// Gates
type Bit = 0 | 1;
type Not<A> = A extends 0 ? 1 : 0;
type Or<A, B> = [A, B] extends [0, 0] ? 0 : 1;
type And<A, B> = [A, B] extends [1, 1] ? 1 : 0;
type Xor<A, B> = [A, B] extends [1, 0] | [0, 1] ? 1 : 0;
type NAnd<A, B> = Not<And<A, B>>;
```
</v-click>

<v-click>
```ts
// half adder
type HalfAdder<A, B> = [S: Xor<A, B>, CO: And<A,B>]
```
</v-click>
---

```yaml
layout: two-cols
layoutClass: gap-8
```

## Full adder

<div class="flex flex-col gap-8">
  <div><img src="full-adder-gates.png" class="h-32"/></div>
  <div><img src="full-adder.png" class="h-32"/></div>
</div>

::right::

<v-click>
```ts
type FullAdder<A, B, CI> =
  HalfAdder<A, B> extends [infer HA1S, infer HA1CO]
    ? HalfAdder<CI, HA1S> extends [infer HA2S, infer HA2CO]
      ? [S: HA2S, CO: Or<HA1CO, HA2CO>]
      : never
    : never;
```
</v-click>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

## 4-bit adder

<div class="flex flex-col gap-8">
  <div><img src="4-bit-adder-gates.png" class="h-48"/></div>
  <div><img src="4-bit-adder.png" class="h-48"/></div>
</div>

::right::

```ts
type BinaryNumber = Array<Bit>;

type LastBit<T extends BinaryNumber> = T extends [...any, infer Last]
  ? Last
  : 0;
type Size<T extends BinaryNumber> = T["length"];

type Head<T extends any[]> = T extends [...infer Head, any] ? Head : [];

// Adder
type Adder<
  A extends BinaryNumber,
  B extends BinaryNumber,
  CI extends Bit = 0,
> = [Size<A>, Size<B>] extends [0, 0]
  ? []
  : FullAdder<LastBit<A>, LastBit<B>, CI> extends [
        infer S,
        infer CO extends Bit,
      ]
    ? [...Adder<Head<A>, Head<B>, CO>, S]
    : never;
```
