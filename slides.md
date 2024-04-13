---
# try also 'default' to start simple
# theme: seriph
# theme: academic
# theme: eloc
theme: default
# background: https://cover.sli.dev
# background: assets/party.png
# some information about your slides, markdown enabled
title: TypeScript type-level programming
info: |
  ## Type-level programming
  Arithemetics

# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# The Unreasonable Effectiveness of TypeScript

## TypeScript type-level programming

Running example: Type-level arithmetics

---

## Setting expectations

<v-clicks>

- Practicality: 0%
- Time waste: 100%

</v-clicks>

<pre class="mt-8">
<span v-click>1. Learn something? "Yes"</span>
<span v-click>2. Is this something useful? "No"</span>
<span v-click>——————————————————————————————————
∴  Conclusion: Learn something useless.
</span>
</pre>

<p v-click>If we learn something useless does it count as learning?</p>

<p v-click>Is it fun?</p>

<p v-click>Why? Because we can!</p>

---

### Topics to cover

- **What** - What is Type-Level Programming?
- **How** - How is it done?
- **Why** - Why TypeScript has it?

---

```yaml
layout: two-cols
layoutClass: gap-16
```

# Table of contents

<v-click>
  <img src="/ts-cute.jpg" class="p-4 rounded shadow" />
  <p>To hide the lack of real content I'll try to cover it with random unrelated memes 🤦️</p> 
</v-click>

::right::

<Toc minDepth="1" maxDepth="2"></Toc>

---

## What is Javascript

- Scheme + Self + C + Java = JavaScript

<img v-click src="/lonely-cloun.png" class="p-4 rounded shadow" />

---

```yaml
layout: two-cols
layoutClass: gap-16
```

## TypeScript

Scheme + Self <span class="opacity-20">+ C + Java</span> = JavaScript

Two very dymanic languages. Both are are very flexible and permissive. So as the offspring JS. Good luck with putting type over it.

JavaScript + Types

<img v-click src="/brendan.png" class="rounded shadow" />

::right::

<img v-click src="/anders.jpg" class="rounded shadow" />

---

```yaml
src: ./slides-how.md
```

---

```yaml
src: ./slides-arithmetics.md
```

---

```yaml
src: ./slides-mergesort.md
```

---

```yaml
src: ./slides-why.md
```
