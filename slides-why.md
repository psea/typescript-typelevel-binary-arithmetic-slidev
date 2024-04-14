- **Why** - Why TypeScript has it?

---

## What is Javascript

### Scheme + Self + C + Java = JavaScript

<div v-click>
When JavaScript joined programming language community
<img src="/lonely-cloun.jpg" class="h-92 rounded shadow" />
</div>

---

```yaml
layout: two-cols
layoutClass: gap-8
```

## TypeScript

Scheme + Self <span class="opacity-20">+ C + Java</span> = JavaScript

Two very dynamic languages. Both are are very flexible and permissive. So as the offspring, JS. Good luck with putting type over it.

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

Static vs Optional type
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
Often when trying to expand in one direction you will contract in others.
