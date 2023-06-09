---
theme: default
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Typescript talk
layout: image
image: /inugami_korone_effective_typescript.png
---

<div class="absolute top-a bottom-a right-10">
  <h1>Typescript talk</h1>
</div>

---
transition: pendle-section
---

<Toc />

---
transition: pendle-section
layout: PendleSection
---

# Type system. Good or bad?

---

## Type system is a _restriction_

Restriction is not always bad.

But too much freedom can lead to very **dangerous** things.

<v-clicks>

- `null` pointer.
- Unsafe cast.
  - Calling undefined method.
  - Un-sanitized user input.
- Zero-access control
  - Calling _unintentional_ methods.
  - Calling method the wrong way.
- Self-modifying code
  - Javascript's `eval`.

</v-clicks>

---

## _Restriction_ can be good

- _Restriction_ **prevents** programmers from doing **bad** things.
- _Restriction_ **guides** programmers to do the **correct** things.
- _Restriction_ does **not** need to be **painful**.

---

## Good restrictions examples

- Rust borrow checker.
- `null` check.
- Linter rules.
- Pureness check.
- **Type system**.

---
layout: section
---

# Type system benefit

---

## Compile time validation

> - Mom, can we have type safety?
> - No, we have type safety at ~~home~~ runtime.
>
> Type safety at runtime:
>
> ```js
> function isEven(a) {
>   assert(typeof a == 'number');
>   return a % 2 == 0;
> }
>
> function isOdd(a) {
>   assert(typeof a == "number");
>   return isEven(a);
> }
> ```

---

## IDE intellisense

<center>
<img src="ide-intellisense/document-createElement.png" />
</center>

---
hideInToc: true
---

## IDE intellisense

<center>
<img src="/ide-intellisense/get-value-of-input.png" />
</center>

---
hideInToc: true
---

## IDE intellisense

<center>
<img src="/ide-intellisense/get-value-of-div.png" />
</center>

---

## Static analysis

Tool with **more restrictions** to enforce correctness, and can even detect **defects** and **vulnerabilities**.

- Cpp:
  [cppcheck](https://github.com/danmar/cppcheck),
  [Clang Static Analyzer](https://clang-analyzer.llvm.org/),
  [clang-tidy](https://clang.llvm.org/extra/clang-tidy/).
- Solidity:
  [solhint](https://github.com/protofire/solhint),
  [slither](https://github.com/crytic/slither),
  [smtchecker](https://docs.soliditylang.org/en/v0.8.17/smtchecker.html).
- Rust:
  [rust-analyzer](https://rust-analyzer.github.io/),
  [clippy](https://github.com/rust-lang/rust-clippy).
- Javascript:
  [eslint](https://eslint.org/),
  [jshint](https://github.com/jshint/jshint).
- Typescript:
  [eslint](https://eslint.org/),
  [typescript-eslint](https://typescript-eslint.io/).

---
hideInToc: true
---

## Static analysis

Some good and interesting rules for both JS and TS.

<div class="grid grid-cols-2 grid-gap-5">
  <div>

<div v-click>

- [for-direction](https://eslint.org/docs/latest/rules/for-direction#rule-details)
```ts
/*eslint for-direction: "error"*/
for (let i = 0; i < 10; i--) {}
for (let i = 10; i >= 0; i++) {}
for (let i = 0; i > 10; i++) {}
```
</div>

<div v-click>

- [no-modify-loop-condition](https://eslint.org/docs/latest/rules/no-unmodified-loop-condition)
```ts
/*eslint no-unmodified-loop-condition: "error"*/
let node = something;
while (node) { doSomething(node); }

node = other;
for (let j = 0; j < items.length; ++i) {
    doSomething(items[j]);
}
```
</div>

  </div>

  <div>

<div v-click>

- [no-unreachable](https://eslint.org/docs/latest/rules/no-unreachable)

```ts
/*eslint no-unreachable: "error"*/
function foo() {
    return true;
    console.log("done");
}
function bar() {
    throw new Error("Oops!");
    console.log("done");
}
while(value) {
    break;
    console.log("done");
}
function baz() {
    if (Math.random() < 0.5) { return; }
    else { throw new Error(); }
    console.log("done");
}

```
</div>

  </div>

</div>


---
hideInToc: true
---

## Static analysis
JS eslint vs TS

<div class="grid grid-cols-2 grid-gap-10">
  <div>

<div v-click=1>

```ts
const myArray = ['a', 'b', 'c'];
// create { a: 1, b: 2, c: 3 };
const indexMap = myArray.reduce((memo, item, index) => {
  memo[item] = index;
}, {});
```
</div>

<div v-click=2>

```ts
const math = Math();
const newMath = new Math();
const json = JSON();
const newJSON = new JSON();
```
</div>

<div v-click=3>

```ts
class Foo {
  x = 1;
  constructor() {}
}
class Bar extends Foo {
  constructor() {
    this.y = this.x + 1;
    super();
  }
}
```
</div>

  </div>

  <div>

Javascript eslint:

- <span v-after=1>[array-callback-return](https://eslint.org/docs/latest/rules/array-callback-return)</span>
- <span v-after=2>[no-obj-calls](https://eslint.org/docs/latest/rules/no-obj-calls)</span>
- <span v-after=3>[no-this-before-super](https://eslint.org/docs/latest/rules/no-this-before-super)</span>

Typescript: <span v-click=4>No rules required!</span>

<center v-after>

<img src="/typescript-vs-js-eslint-meme.png" width="250"/>

</center>

  </div>
</div>

---
hideInToc: true
---
## Static analysis
Linting with `@typescript-eslint/recommended-requiring-type-checking`


<span>

- Working with promises

</span>


<div grid grid-cols-2 grid-gap-1 >

<div> <twemoji-cross-mark /> Incorrect </div>
<div> <twemoji-check-mark-button /> Correct </div>

<div v-click>

```ts
const promise = Promise.resolve('value');
if (promise) {}

```
</div>

<div v-after>

```ts
const promise = Promise.resolve('value');
if (await promise) {}
```
</div>

<div v-click>

```ts
async function invalidInTryCatch1() {
  try {
    return Promise.resolve('try');
  } catch (e) {}
}

```
</div>

<div v-after>

```ts
async function validInTryCatch1() {
  try {
    return await Promise.resolve('try');
  } catch (e) {}
}

```
</div>

<div v-click>

```ts
[1, 2, 3].forEach(async value => {
  await doSomething(value);
});
```
</div>

<div v-after>

```ts
Promise.all(
  [1, 2, 3].map(async value => {
    await doSomething(value);
  }),
).catch(handleError);
// or use for-of
```
</div>

</div>

---
hideInToc: true
---
## Static analysis
Linting with `@typescript-eslint/recommended-requiring-type-checking`

<span v-click>

- no `any`

</span>

<div grid grid-cols-2 grid-gap-1 v-after>

<div> <twemoji-cross-mark /> Incorrect </div>
<div> <twemoji-check-mark-button /> Correct </div>

<div>

```ts
// fail with no-explicit-any
const age: any = 'seventeen';

// fail with no-unsafe-assignment
const x = 1 as any;

// no-unsafe-argument
function a(x: number, y: number) {}
const args: any[] = [];
a(...args);
```
</div>

<div>

```ts

const age: number = 17;


const x = 1;


function a(x: number, y: number) {}
const args: [number, number] = [1, 2];
a(...args);
```
</div>

</div>

<span v-click>

- [no-base-to-string](https://typescript-eslint.io/rules/no-base-to-string)

</span> 

<div grid grid-cols-2 grid-gap-1 v-after>

<div> <twemoji-cross-mark /> Incorrect </div>
<div> <twemoji-check-mark-button /> Correct </div>

<div>

```ts
class Foo {};
console.log(`value=${new Foo()}`);
// value=[object Object]
```
</div>

<div>

```ts
class Foo { toString() { return 'Foo'; }}
console.log(`value=${value}`); 
// value=Foo
```
</div>

</div>
---

## Reflection

<div class="grid grid-cols-2 grid-gap-3">

<div>

Example: Dependency injection in Nest.js

```ts {all|8}
import { Controller, Get, Post, Body } from "@nestjs/common";
import { CreateCatDto } from "./dto/create-cat.dto";
import { CatsService } from "./cats.service";
import { Cat } from "./interfaces/cat.interface";

@Controller("cats")
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

</div>

<div>
<div v-click>

In `tsconfig.json`

```json {3}
{
  "compilerOptions": {
    "emitDecoratorMetadata": true
  }
}
```
</div>

<div v-click>
Behind the scenes
```ts {4,11}
var __decorate = /* */;
var __metadata = /* */;
export let CatsController = class CatsController {
    constructor(catsService) {
        this.catsService = catsService;
    }
    // ...
};
CatsController = __decorate([
    Controller('Cat'),
    __metadata("design:paramtypes", [CatsService])
], CatsController);
```

</div>

</div>

</div>

---

## Compile time optimization.

- [google-closure-compiler](https://github.com/google/closure-compiler/)
  - Compile and optimize Javascript code.
  - Use `jsdoc` for typing.
  - Example:

<div grid grid-cols-2 grid-gap-1>

<div>
Input
```js
function add(a, b) { return a + b; }
console.log(add(1, 2), add('x', 'y'));
```
</div>

<div>
Output

```js
console.log(3,"xy");
```
</div>

</div>

- [Tsickle](https://github.com/angular/tsickle)
  - Compile TS -> JS but with JSDoc. Then compile it with closure compiler.
  - Not ready for public use. Used **internally** in google.

---
layout: section
---
# Typesafe with Typescript

---

# Typescript is structural typing

https://www.typescriptlang.org/docs/handbook/type-compatibility.html

```ts
interface Pet {
  name: string;
}
class Dog {  // looks ma, no implements
  name: string;
}
let pet: Pet;
// OK, because of structural typing
pet = new Dog();
```

```ts
// dog's inferred type is { name: string; owner: string; }
let dog = { name: "Lassie", owner: "Rudd Weatherwax" };
pet = dog;
```

The following give errors <twemoji-cross-mark />, as that object literals may only specify **known properties**:

```ts
pet = { name: "Lassie", owner: "Rudd Weatherwax" };
```

---
# Strict null check

- tsconfig.json
```json
{
  "compilerOptions": {
    "strictNullCheck": true,
    // Or just use "strict": true 👍
  }
}
```

- `null` and `undefined` are now **separate** types!
  - `number` is not the same as `number | undefined` or `number | null`.