# What is type-level programming?

## Normal types

Add two numbers

```ts twoslash
declare function sumStr(a: string, b: string): number;

const x = sumStr("23", "32");
```

Sort array of number

```ts twoslash
declare function sortStr(xs: string[]): number[];

const xs = ["3", "1", "2", "0"];
const sortedXs = sortStr(xs);
```

---

# What is type-level programming?

## Make type-system calculate the result

But it's clear `x` could must be `55` and `sortedXs` must be `[1, 2, 3]`.  
Can we make type-system to be a little bit more precise?  
Calculate the result and tell us what `x` and `sortedXs` should be ?  
All _before_ running the program, purely relying on a type-checker? <span v-click>Yes, we can!</span>

<div class="grid gap-4 grid-cols-[auto_auto]">

```ts
const x = sumStr("23", "32");
//    ^? const x:55
//
//
```

<div v-after>
```ts
declare function sumStr<A extends string, B extends string>(
  a: A,
  b: B
): AddStr<A, B>;
```
</div>

```ts
const xs = ["3", "1", "2"] as const;
const sortedXs = sortStr(xs);
//    ^? sortedXs = [1, 2, 3];
```

<div v-after>
```ts
declare function sortStr<A extends readonly string[]>(xs: A): SortNumberStr<A>;
//
//
```
</div>

</div>

<span v-after>
Without ever running the program we got the results. This might just make TypeScript the fastest language on Earth, with a runtime of precisely zero. 😉️
</span>

---

# How `SortNumberStr<A>` type look like?

```ts
type SortNumberStr<Xs extends readonly string[]> = MapToNumbers<
  MergeSort<MapFromStrings<Mutable<Xs>>>
>;
```

```ts {*}{lines:true,maxHeight:'70%'}
type MergeSplit<
  T extends BinaryNumber[],
  Xs extends BinaryNumber[] = [],
  Ys extends BinaryNumber[] = [],
> = T extends [infer X]
  ? [[...Xs, X], Ys]
  : T extends [
        infer X extends BinaryNumber,
        infer Y extends BinaryNumber,
        ...infer Rest extends BinaryNumber[],
      ]
    ? MergeSplit<Rest, [...Xs, X], [...Ys, Y]>
    : [Xs, Ys];

type Merge<Xs extends BinaryNumber[], Ys extends BinaryNumber[]> = Xs extends []
  ? Ys
  : Ys extends []
    ? Xs
    : [Xs, Ys] extends [
          [infer X extends BinaryNumber, ...infer Xs extends BinaryNumber[]],
          [infer Y extends BinaryNumber, ...infer Ys extends BinaryNumber[]],
        ]
      ? Compare<X, Y> extends "LT"
        ? [X, ...Merge<Xs, [Y, ...Ys]>]
        : [Y, ...Merge<[X, ...Xs], Ys>]
      : never;
type MergeSort<Xs extends Array<BinaryNumber>> = Xs["length"] extends 1 | 0
  ? Xs
  : MergeSplit<Xs> extends [
        infer Ls extends Array<BinaryNumber>,
        infer Rs extends Array<BinaryNumber>,
      ]
    ? Merge<MergeSort<Ls>, MergeSort<Rs>>
    : never;

declare function sortStr<A extends readonly string[]>(xs: A): SortNumberStr<A>;
```
