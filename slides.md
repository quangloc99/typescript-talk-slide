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
layout: cover
background: /inugami_korone_effective_typescript.png
---

<div absolute right-10 top-50 text-right font-extrabold>

# Typescript talk
Tran Quang Loc
</div>

<div absolute bottom-0 left-2 text-sm text-gray-2>

Image source: https://github.com/cat-milk/Anime-Girls-Holding-Programming-Books
</div>

---
clicks: 0
transition: pendle-section
---

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
transition: pendle-section
---

## Good restrictions examples

- Rust borrow checker.
- `null` check.
- Linter rules.
- Pureness check.
- **Type system**.

---
layout: PendleSection
transition: pendle-section
---

# Type system benefits

---

## Compile time validation

Mom, can we have type safety?

No, we have type safety at ~~home~~ runtime.

Type safety at runtime:

<div v-click>

```js
function isEven(a) {
  assert(typeof a == 'number');
  return a % 2 == 0;
}

function isOdd(a) {
  assert(typeof a == "number");
  return isEven(a);
}
```
</div>

---
transition: slide-up
---

## IDE intellisense

<center>
<img src="ide-intellisense/document-createElement.png" />
</center>

---
hideInToc: tru
transition: slide-up

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
transition: slide-up
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
transition: slide-up
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
transition: slide-up
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
transition: slide-up
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
transition: pendle-section
---

## Compile time optimization

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
layout: PendleSection
transition: pendle-section
---
# Typesafe with Typescript

---

## Typescript is structural typing

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
transition: slide-up
---

## Strict null check

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

---
hideInToc: true
transition: slide-up
---

## Strict null check
Always check your null first!

```ts
const users = [
  { name: 'foo', age: 20 },
  { name: 'bar', age: 30 },
];

const user = users.find((u) => u.name === 'baz');
//    ^? const user: { name: string, age: number } | undefined
```

<div grid grid-cols-2 grid-gap-1 v-click>
  <div>

<twemoji-cross-mark /> Error
  
```ts

console.log(user.age);
// 'user' is possibly 'undefined'.
```

  </div>

  <div>

<twemoji-check-mark-button /> Correct

```ts
if (user != null) {
  console.log(user.age);
}
```

  </div>
</div>

---
hideInToc: true
transition: slide-up
---

## Strict null check
B..but I don't wanna <twemoji-pleading-face />

<div grid grid-cols-2 grid-gap-1>

  <div v-click>

### Operator `??=`
[Nullish coalescing assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_assignment)

  </div>

  <div v-after>

```ts
let user = users.find((u) => u.name === 'baz');
user ??= { name: 'baz', age: 50 };
console.log(user.age);  // 50
```
  </div>

  <div v-click>

### Operator `??`
[Nullish coalescing operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)
  </div>

  <div v-after>

```ts
const user = users.find((u) => u.name === 'baz');
const defaultUser = user ?? { name: 'baz', age: 50 };
console.log(defaultUser);  // 50
```
  </div>

  <div v-click>

### Operator `?.`
[Optional chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

  </div>

  <div v-after>

```ts
const user = users.find((u) => u.name === 'baz');
console.log(user?.age);         // undefined
console.log(user?.age ?? 50);   // 50

console.log(
  user?.age?.toString(16) ?? 'cant convert to hex'
);
```
  </div>

</div>

---
hideInToc: true
---

## Strict null check

More Optional chaining!

```ts
const arr: number[] | null = null;
const obj: Record<string, string> | null = null;
console.log(arr?.[0]);          // undefined
console.log(obj?.['a' + 'b']);  // undefined
```

```ts
const func: ((a: number, b: number) => number) | null = null;
console.log(func?.(1, 2));  // undefined
```

Don't use `||`!
```ts
const a: number | null = null
const b: number | null = 0;

console.log(a || 10, a ?? 10);  // 10 10
console.log(b || 10, b ?? 10);  // 10 0
```

---
transition: slide-up
---

## Union type
Combine structurally-unrelated types into one

```ts
const a: number | string = 1;       // ✅ ok
const b: number | string = '1';     // ✅ ok
const c: number | string = false;   // ❌ nope
```

```ts
interface FullName { fullName: string };
interface PartialName { firstName: string; lastName: string };
type Name = FullName | PartialName;

const foo: Name = { fullName: 'John Doe' }
const bar: Name = { firstName: 'Jane', lastName: 'Doe' };
```

---
hideInToc: true
---
## Union type
Literals are types too!

<div grid grid-cols-2 grid-gap-1>
  <div>

```ts
type BinaryDigits = 0 | 1;


type HumanReadableBoolean = boolean | 'on' | 'off';


type DoYouLoveMeQuestionMark = 
  'Yes!' | 'Absolutely!' | 'Definitely!';

type LogLevel = 
  | 'off' | 'debug' | 'aggressive'
  | 0 | 1 | 2;


type Shape =
  | { type: 'circle';
      x: number; y: number; r: number; }
  | { type: 'rectangle'; 
      x: number; y: number; w: number; h: number; };

type NullableString = string | null | undefined;
```

  </div>

  <div>

```ts
let zero: BinaryDigit = 0;    // ✅
let two: BinaryDigit = 2;     // ❌

let onValue: HumanReadableBoolean = 'on';   // ✅
let wrongOnValue: HumanReadableBoolean = 1; // ❌

let nope: DoYouLoveMeQuestionMark = 'NO';   // ❌


let logLv: LogLevel = 'debug';        // ✅
let logLvNum: LogLevel = 2;           // ✅
let invalidLogLv: LogLevel = 'info';  // ❌
let invalidLogLvNum: LogLevel = 3;    // ❌

let circle: Shape = { type: 'circle', x: 0, y: 0 }; // ✅
let rec: Shape = {                                  // ✅
  type: 'rectangle', x: 0, y: 0, w: 10, h: 20
};
let line: Shape = {                                 // ❌
  type: 'line', x1: 0, y1: 0, x2: 10, y2: 10
};
```
  </div>
</div>

---
transition: slide-up
hideInToc: true
---

## Narrowing union types
Using `typeof`

```ts {all|1-3|5|6-8|all}
type LogLevelNum = 0 | 1 | 2;
type LogLevelText = 'off' | 'debug' | 'aggressive';
type LogLevel = LogLevelNum | LogLevelText;

function resolveLogLevel(logLevel: LogLevel): LogLevelNum {
  if (typeof logLevel === 'number') {
    return logLevel;
  }
  if (logLevel == 'off') return 0;
  if (logLevel == 'debug') return 1;
  if (logLevel == 'aggressive') return 2;

  assert(false);
}
```
---
hideInToc: true
transition: slide-up
---

## Narrowing union types
Control flow analysis

```ts{all|5,6|9|10|all}
type LogLevelNum = 0 | 1 | 2;
type LogLevelText = 'off' | 'debug' | 'aggressive';
type LogLevel = LogLevelNum | LogLevelText;

const LOG_LEVELS = ['off', 'debug', 'aggressive'] as const;
//    ^? const LOG_LEVELS: readonly ['off', 'debug', 'aggressive']

function logLevelToString(logLevel: LogLevel): LogLevelText {
  if (typeof logLevel === 'string') return logLevel;
  return LOG_LEVELS[logLevel];
}
```

---
hideInToc: true
transition: slide-up
---

## Narrowing union types
Control flow analysis

<div grid grid-cols-3 grid-gap-1>
  <div>

With branching
```ts
function withIfElse(
  x: number | string
) {
  // x is number | string
  if (typeof x === 'number') {
    // x is number
  } else {
    // x is string
  }
  // x is number | string
}
```

  </div>

  <div>

With `return`
```ts
function withReturn(
  x: number | string
) {
  // x is number | string
  if (typeof x === 'number') {
    // x is number
    return ;
  }
  // x is string
}
```

  </div>

  <div>

With `throw`
```ts
function withReturn(
  x: number | string
) {
  // x is number | string
  if (typeof x === 'number') {
    // x is number
    throw new Error('x should not be number');
  }
  // x is string
}
```
  </div>

</div>


---
transition: slide-up
hideInToc: true
---
## Narrowing union types
Equality narrowing

```ts
type LogLevelNum = 0 | 1 | 2;
type LogLevelText = 'off' | 'debug' | 'aggressive';
type LogLevel = LogLevelNum | LogLevelText;
```
```ts{all|2-5|3-5|4-5|5|all}
function resolveLogLevel(logLevel: LogLevel): LogLevelNum {
  if (logLevel == 'off') return 0;
  if (logLevel == 'debug') return 1;
  if (logLevel == 'aggressive') return 2;
  return logLevel;
}
```

<div v-click>

```ts
type DoYouLoveMeQuestionMark = 'Yes!' | 'Absolutely!' | 'Definitely!';
function shoutAnswer(answer?: DoYouLoveMeQuestionMark): string {
  if (answer == null) return 'Silent :(';
  return answer.toUpperCase();
}
```
</div>


---
transition: slide-up
hideInToc: true
---

## Narrowing union types
Using `instanceof`

<div grid grid-cols-2 grid-gap-1>
  <div>

```ts
class Duck {
  quack() {
    console.log('Quack!');
  }
}

class Horse {
  neigh() {
    console.log('Neigh!');
  }
}

type Animal = Duck | Horse;
```
  </div>

  <div>

```ts {all|2-4|5}
function makeNoise(animal: Animal) {
  if (animal instanceof Duck) {
    animal.quack();
  }
  animal.neigh();
}
```

```ts
makeNoise(new Duck());    // Quack!
makeNoise(new Horse());   // Neigh!
```
  </div>
</div>

---
hideInToc: true
transition: slide-up
---
## Narrowing union types
Discriminated union

```ts
type Shape =
  | { type: 'circle';    x: number; y: number; r: number; }
  | { type: 'rectangle'; x: number; y: number; w: number; h: number; };

function calcArea(shape: Shape) {
  if (shape.type === 'circle') {
    return Math.PI * shape.r ** 2;
  }
  return shape.w * shape.h;
}
```

---
hideInToc: true
transition: slide-up
---

## Narrowing union types
Type predicate functions

<div grid grid-cols-2 grid-gap-1>

  <div>

```ts
type Circle = {type: 'circle'; r: number;};
type Rectangle = {type: 'rectangle'; w: number; h: number;};
type Shape = Circle | Rectangle;

function isCircle(shape: Shape): shape is Circle {
  return shape.type === 'circle';
}

function isRectangle(shape: Shape): shape is Rectangle {
  return shape.type === 'rectangle';
}
```
  </div>

  <div>

```ts
function calcArea(shape: Shape) {
  if (isCircle(shape)) return Math.PI * shape.r ** 2;
  return shape.w * shape.h;
}
```

```ts
const a: Shape[] = [];

const b = a.filter((shape) => shape.type === 'circle');
//    ^? const b: Shape[];
const c = a.filter(isCircle);
//    ^? const c: Circle[];
const d = a.filter((shape): shape is Circle =>
  shape.type === 'circle'
);
```
  </div>
</div>

---
hideInToc: true
transition: slide-up
---

## Narrowing union types
Type assertion functions

```ts{all|5}
type LogLevelNum = 0 | 1 | 2;
type LogLevelText = 'off' | 'debug' | 'aggressive';
type LogLevel = LogLevelNum | LogLevelText;

function assertLogLevelIsSilent(logLevel: LogLevel): asserts logLevel is 0 | 'off' {
  if (logLevel !== 0 && logLevel !== 'off') {
    throw new Error(`log level should be silent, but found ${logLevel}`);
  }
}
```

<div v-click>

```ts
function silentProcess(logLevel: LogLevel) {
  assertLogLevelIsSilent(logLevel);

  // logLevel is now 0 or 'off'
}
```
</div>

---
hideInToc: true
transition: pendle-section
---

## Narrowing union types

There are still more!
- Truthiness narrowing
- The `in` operator narrowing
- Assignment
- `never` type
- Exhaustiveness checking

More info can be found in https://www.typescriptlang.org/docs/handbook/2/narrowing.html

---
layout: PendleSection
transition: pendle-section
---

# Typescript libraries and toolings

---

## Definitelytyped

https://definitelytyped.github.io/

The repository for high quality TypeScript type definitions

Examples for JQuery:

```shell
npm install --save-dev @types/jquery
```

---

## ts-essentials
https://github.com/ts-essentials/ts-essentials

All basic TypeScript types in one place 🤙

```shell
npm install --save-dev ts-essentials
```

Remember to turn on strict mode!

Examples

```ts
type A = AsyncOrSync<number>; 
//   ^? number | PromiseLike<number>
type B = MarkOptional<{ a: number; b: string; c: boolean }, 'a' | 'c'>;
//   ^? { a?: number; b: string; c?: boolean }

type Company = {
  name: string;
  employees: { name: string }[];
}
type C = DeepPartial<Company>;
//   ^? { name?: string | undefined; employees?: ({ name?: string | undefined } | undefined)[] | undefined }
```

---

## ts-toolbelt
https://github.com/millsp/ts-toolbelt

TypeScript's largest utility library, featuring **+200 utilities**!

```shell
npm install ts-toolbelt --save
```

Remember to turn on strict mode!

Examples:

```ts
import {Object} from "ts-toolbelt"
// Check the docs below for more

// Merge two `object` together
type merge = Object.Merge<{name: string}, {age?: number}>
// {name: string, age?: number}

// Make a field of an `object` optional
type optional = Object.Optional<{id: number, name: string}, "name">
// {id: number, name?: string}
```

---

## Zod
TypeScript-first schema validation with static type inference 

```shell
npm install zod
```

Usage:
<div grid grid-cols-2 grid-gap-1>
  <div>

Creating a simple string schema
```ts
import { z } from "zod";

// creating a schema for strings
const mySchema = z.string();

// parsing
mySchema.parse("tuna"); // => "tuna"
mySchema.parse(12); // => throws ZodError

// "safe" parsing (doesn't throw error if validation fails)
mySchema.safeParse("tuna"); // => { success: true; data: "tuna" }
mySchema.safeParse(12); // => { success: false; error: ZodError }
```
  </div>

  <div>

Creating an object schema
```ts
import { z } from "zod";

const User = z.object({
  username: z.string(),
});

User.parse({ username: "Ludwig" });

// extract the inferred type
type User = z.infer<typeof User>;
// { username: string }
```
  </div>
</div>

---
transition: pendle-section
---

## ArkType
https://arktype.io/

ArkType is a runtime validation library that can infer **TypeScript definitions 1:1** and reuse them as **highly-optimized validators** for your data.

```shell
npm install arktype
```

Example:

<div grid grid-cols-2 grid-gap-1>
  <div>

```ts {all|7-8}
import { type } from "arktype"

// Definitions are statically parsed and inferred as TS.
export const user = type({
    name: "string",
    device: {
        platform: "'android'|'ios'",
        "version?": "number"
    }
})
```
  </div>

```ts

// Validators return typed data or clear,
// customizable errors.
export const { data, problems } = user({
    name: "Alan Turing",
    device: {
        // problems.summary: "device/platform
        // must be 'android' or 'ios' (was 'enigma')"
        platform: "enigma"
    }
})
```

  <div>
  </div>
</div>

---
layout: PendleSection
transition: pendle-section
---

# Bonus: Type manipulation
Typescript true power!

--- 

## meta-types

https://github.com/grantila/meta-types

TypeScript meta functions for (especially variadic) meta programming 

```
npm install meta-types
```

Examples
```ts
import type { Add, Sub, Mul, If, GreaterThan } from 'meta-types'
type T1 = Add< 13, 11 >; // T1 is 24
type T2 = Sub< 13, 11 >; // T2 is 2
type T3 = Mul< 13, 11 >; // T3 is 143

type T4 = If< true, "yes", "no" >;  // T4 is "yes"
type T5 = If< false, "yes", "no" >; // T5 is "no"

type T6 = GreaterThan< 42, 40 >;       // T6 is true; 42 > 40
type T7 = GreaterThan< 40, 42 >;       // T7 is false; 40 < 42
```

---

## Higher-Order TypeScript (HOTScript)

https://github.com/gvergnaud/hotscript

A library of composable functions for the type level!

```shell
npm install -D hotscript
```

Examples:

```ts
import { Pipe, Tuples, Strings, Numbers } from "hotscript";

type res1 = Pipe<
  //  ^? 62
  [1, 2, 3, 4],
  [
    Tuples.Map<Numbers.Add<3>>,       // [4, 5, 6, 7]
    Tuples.Join<".">,                 // "4.5.6.7"
    Strings.Split<".">,               // ["4", "5", "6", "7"]
    Tuples.Map<Strings.Prepend<"1">>, // ["14", "15", "16", "17"]
    Tuples.Map<Strings.ToNumber>,     // [14, 15, 16, 17]
    Tuples.Sum                        // 62
  ]
>;
```

---

## How do you like your Typescript, ser?

<center>
  <img src="winnie-the-pooh-3-validation-levels.png" width="400" />
</center>

---

## Example: Let's make `resolveLogLevel` even better!

```ts
const a = resolveLogLevel('off');
const b = resolveLogLevel(1);
const c = resolveLogLevel('aggressive');
```

<div grid grid-cols-2 grid-gap-2>
  <div v-click>

Current typing:
```ts
const a: 0 | 1 | 2;
const b: 0 | 1 | 2;
const c: 0 | 1 | 2;
```
  </div>

  <div v-click>

Better typing:
```ts
const a: 0;
const b: 1;
const c: 2;
```
  </div>
</div>

---

## First attempt
Function overloading
```ts
function resolveLogLevel(logLevel: 'off'): 0;
function resolveLogLevel(logLevel: 'debug'): 1;
function resolveLogLevel(logLevel: 'aggressive'): 2;
function resolveLogLevel(logLevel: 0): 0;
function resolveLogLevel(logLevel: 1): 1;
function resolveLogLevel(logLevel: 2): 2;
```

<div grid grid-cols-2 grid-gap-1 text-center v-click>
  <div justify-self-center>

Scalable for machine?

<img src="perhaps-meme.webp" width="200" />
  </div>

  <div justify-self-center>

Scalable for human?

<img src="bug-bunny-no-memes.jpg" width="200" />
  </div>
</div>

---

## Conditional typing

Syntax
```ts
type X = A extends B ? TrueBranchType : FalseBranchType;
```

Examples
```ts
type A = 'off';
type B = 0;
type C = 'off';

type UnderlyingTypeOfA = A extends string ? string : number;        // string
type UnderlyingTypeOfB = B extends string ? string : number;        // number

type IsAOff = A extends 'off' ? true : false;                       // true
type IsAEqB = A extends B ? (B extends A ? true : false) : false;   // false
type IsAEqC = A extends C ? (C extends A ? true : false) : false;   // true
```

---

## Basic logic for resolved log level

<iframe 
  width="900"
  height="450"
  src="https://www.typescriptlang.org/play?#code/C4TwDgpgBAglC8UAMBuAUAeg1UlYKgHIB7AM1MPSx3GjkUIBMIAjAVwHNLNtc6DCAQw4cAThADOEgJYA3CNzR8oAJUnEANvMYAZYhx0R5GgGLFR9NFGv4IAD2AQAdowlEyFKAH5kVqAC5bB2dXImZ2Lm8oAEY-QLh7Rxc3IRFxKTkFKIAmONgUIA"
/>

---

## Generic type

Syntax
```ts
type GenericType<TypeParam1 [extends Contraints1], TypeParam2 [extends Constraints2], ...> = TypeDefinition;
```

Examples:

```ts
type Identity<T> = T;
type Nullable<T> = T | null | undefined;
type Matrix<T> = T[][];
type Vector<T extends (number | bigint)> = { x: T; y: T };
type ConcatTyple<A extends unknown[], B extends unknown[]> = [...A, ...B];
```

```ts
type UnderlyingType<A> = A extends string ? string : number;
type IsOff<A extends string> = A extends 'off' ? true : false;
type Eq<A, B> = A extends B ? (B extends A ? true : false) : false;
```

---

## The `ResolvedLogLevel` type

<iframe 
  width="900"
  height="450"
  src="https://www.typescriptlang.org/play?#code/C4TwDgpgBAMg9gcxhAbhANlAvFA5HAMwNygB88ATCAIwFcETzcBDBBAJwgGcuBLNRlAAMZKAEZRAJgDcAKFmhIUAErc46NBXhJUGADwBBKBAAewCADsKXWImRp0APmyyobqEdPmrN-ERIA-MKuUABcHsZmltaUNPSB4iHhnlE+eKwc3HwCUEGSSR5yCuDQRjiqXOqa2vb6fsSOcgD0Te4AegHFSgBC2CpqGhBadrroekKNsi3tnYrQAMJ9FVVDNaN6LGycPPwQuJPTbh1d0AAiSwPVIw7jopuZOwIHrUedQA"
/>

---

## The `resolveLogLevel` function
<iframe 
  width="900"
  height="450"
  src="https://www.typescriptlang.org/play?#code/GYVwdgxgLglg9mABAJwKYGc4BsBuqAycA5vqnlgDz46KoAeUqYAJuooSWalgHwAUWYqXIAudjgCUYgEoZseZh2HcqOHgG4AUKEiwEKObgJCuWASdHsL3KVc7kAciAC2iAN6bEiGMETn73IgAvCGIAORwwMBhEgZQIMhIAAxaXj5+ggFYwaFhzKgARiBEMXEJSACMqd6+-srZIUHhAIZERGjo6DB4pWjxiYgATNV95YiZ9VoAvpoQCOhQiM3BBphGSqZ8eYXFMVoA9PteXgB6APyz84sFKx3yxll8SRIHR8fnl2ALiBC3hngbchbVrtDBdHovTSHY6ID6aTRQACeAAdUHZ6isIlEwogAD7hfJFEp4lptDrg1A4-FJEkVEnDBEotGA7hOVxNGn4un4hmM1GIWRrBQsygAQVoDCYrHRph4wU8x3F9EYLDYWOiiDOiCSCrESslqoJO2JWoqusQ+pV0rCIPJ3UpmqG5tFWk0Hi8Omg8CQd3W1iwAHUYFAABYAUWQyDgyFUEqtbBF-AmpjE1Fsgvuin9qjl7uO6TqphyTXVvVQ-WS1TStWT5GLRqJZYriCqCurGX99ZtZLB9qbYwZMNGA1r3GqMymQA"
/>

---

## Indexed access type

Syntax
```ts
type Value = Obj[Index];
```

Where `Obj` should be an object type, and `Index` should be **key** of `Obj` (should extends `keyof Obj`).

Examples:
<div grid grid-cols-2 grid-gap-1>
  <div>

```ts
type Obj = {
  a: number;
  b: string;
  c: boolean;
};

type Tuple = [number, string, boolean];
```
  </div>

  <div>

```ts
type A = Obj['a'];  // number
type B = Obj['b'];  // string
type C = Obj['c'];  // boolean

type T0 = Typle[0]; // number
type T1 = Tuple[1]; // string
type T2 = Tuple[2]; // boolean

// number | string | boolean
type ValueOfObj = Obj[keyof Obj];
type ValueOfTuple = Tuple[keyof Tuple];
// number | boolean
type AorC = Obj['a' | 'c'];  
type T0or2 = Tuple[0 | 2];
```

  </div>
</div>

---

## Another way to implement `ResolvedLogLevel`

```ts
type ResolvedLogLevelMap = {
  0: 0;
  1: 1;
  2: 2;
  'off': 0;
  'debug': 1;
  'aggressive': 2;
};

type LogLevel = keyof ResolvedLogLevelMap;
type ResolvedLogLevel<Lv extends LogLevel> = ResolvedLogLevelMap[Lv];
```

---

## Example: Simple contract method ABI to function

<div grid grid-cols-2 grid-gap-1>
  <div v-click>

```json
{ "name": "decimals",
  "inputs": [],
  "outputs": [ { "type": "uint8" } ],
}
```
  </div>

  <div v-after>

```ts
type Fn = () => [number];
```
  </div>

  <div v-click>

```json
{ "name": "transferFrom",
  "inputs": [
    { "name": "from", "type": "address" },
    { "name": "to", "type": "address" },
    { "name": "amount", "type": "uint256" }
  ],
  "outputs": [ { "type": "bool" } ],
}
```
  </div>

  <div v-after>

```ts
type Fn = (
  from: string, to: string, amount: bigint
) => [bool];
```
  </div>

  <div v-click>

```json
{ "name": "observations",
  "inputs": [ { "type": "uint256" } ],
  "outputs": [
    { "name": "blockTimestamp", "type": "uint32" },
    { "name": "lnImpliedRateCumulative",
      "type": "uint216" },
    { "name": "initialized", "type": "bool" }
  ],
}
```
  </div>

  <div v-after>

```ts
type Fn = (_param: bigint): [bigint, bigint, boolean];
```
  </div>

</div>

---
hideInToc: true
---

## Step 1. Primitive type mapping

```ts {all|5}
type MapPrimitiveType<T> =
    T extends 'bool' ? boolean
  : T extends 'address' ? string
  : T extends 'uint8' ? number
  : T extends `uint${number}` ? bigint
  : never;
```

<div v-after>

* * *

### Template literal type

Syntax
```ts
type StringType = `x${T1}y${T2}z`;
```

Examples

```ts
type AddMrTitle<Name extends string> = `Mr. ${Name}`;
type SimpleEmail = `${string}@${string}.${string}`;

type HexString = `0x${string}`;
type ContractIdentity = `${HexString /* chain id */}-${HexString /* contract address */}`;

type TestUint256 = 'uint256' extends `uint${number}` ? true : false;  // true
```

</div>

---
hideInToc: true
---

## Step 2. Array of primitive type mapping

```ts{all|3}
type MapPrimitiveArrayType<Arr> =
    Arr extends readonly [] ? []
    : Arr extends readonly [{type: infer HeadType} , ...infer Rest]
      ? [MapPrimitiveType<HeadType>, ...MapPrimitiveArrayType<Rest>]
      : never;
```

<div v-after>

* * *

### Inferring Within Conditional Types

Syntax
```ts
type T = A extends SomeType<infer T> ? /* do something with T */ : FalseBranchType;
```

Examples:
```ts
type ArrayElementType<Arr> = Arr extends (infer T)[] ? T : never;
type FlattenArrayType<Arr> = Arr extends (infer T)[] ? FlattenArrayType<T> : Arr;
type GetFirstElementType<Arr> = Arr extends [infer First, ...infer _Rest] ? First : never;
type GetType<Entry> = Entry extends { type: infer T } ? T : never;
```

</div>

---
hideInToc: true
---

## Step 3. Create the function

```ts
type AbiToFunc<Abi> =
  Abi extends { inputs: infer Input; outputs: infer Output }
  ? (...params: MapPrimitiveArrayType<Input>) => MapPrimitiveArrayType<Output>
  : never;
```

```ts
declare function generateFunction<Abi>(abi: Abi): AbiToFunc<Abi>;
```

---
hideInToc: true
---

## Result

<iframe 
  width="900"
  height="450"
  src="https://www.typescriptlang.org/play?#code/GYVwdgxgLglg9mABAWwIYzACgJSIN4BQiiECAzlIgCYCmEMaANgGLgSIC8iA5jWDQCdUUGq0iwEmACIBRAMIBJALIBBADIBlAPoqAQguwBuIogD0p4sQB6AfhOkwFRFCGPgg5gLjIx7Lr34hEV8JLAAVACUVADkNZhkIrWYIgHklHX0jE3NLRFt7cko4ACMyQQA3YXhHT3BOHj5BYVE2UMwU3Q0EgDUVMIUU2IyDY2Icy3yAXwICBydZRVVNYfrCYgAiMFRkGnWALkR12nomMnWAGhN1jAAHECgzg4BtAF1Ljbh7u4f9xCe8Q5QACeN12B3WIAwUAAHOtEJM3gRJohUGQSIVjLNCohIjE4gkkql0noFKsrlsdr91i5UG4PF5kBcrrd7o8-iZiADNtswYdgAyLoCQbz1qgqFQBDQyGd4e9LFyKSKoHBBdThVSxRKpTLJnLOYdFRrkJ8wFBVcDQVTIaaAEwAVgAbHDpsRER8vqzfv8hZbwcU4HBGM7EcjUejHFBMXNKB0uhFev1BtoSWSNobwSUygJKqEznLrmBvmzvWrfYdrVB7U74W7Dp8oEWvRz8AaeVTiow4BAANZhBhSqDbG7m9XgisAZhtzr1+Gbabb4MYYAUyBujBgNCoEWachAyBAjCq5V2M42FpFFZtAEYnc3dc2FQvDhgYLBUOuAF6bkdl9b+wPOiYIYomi0aYgQ56IEoqA3AACgIDCvjAx5hMKAA8YQAHycCYYSIDQAAeIhgFQaIAOT-owZGIDYiCUTQtImAceGEcRpGIGRmqStK1G0RQCFgNwTE4vhRF8OxZEVtCvGIGAe7FIIwksWJJFogABhWAAkeBycgCkCJMak0XRMDcFCwn8MeAiYpB0FwQhyBIceKgCEIQKoaCaEuQI2EcCY3miWxaKSmKCCMECfwvCYtGvMJAWseJwUMVQYURd654HBg7gCIgAASyUeTQsqIAAdGVWWCIgEQDlFxAxXZ8GIbAKHoflYqFZh5ylWVDUOU5NDeag7nodVFCYbViAHJZgijIgEHCogKjFDAYRwL4XnLb5-nLYFiUtiyDyZWA2WIAohb3IYiD1kWR0nSkHqUC6xmYGVJU3KgQjIGQBy9U1yEDa5Q2FWhZ3fJhuAcNhv2Oc1ANucD90NvcmEWTQVmYsch6SogoDiNUDSBM0ITVBtMCYZgqDLQcS0wNg1PLat6005hmJAA"
/>

---

## Example: Route URL parsing with typed parameters

<div grid grid-cols-2 grid-gap-1>
  <div>

```ts
`/v1/{chainId:number}/assets`
```
  </div>

  <div>

```ts
type Params = {
  chainId: number;
};
```
  </div>

  <div>

```ts
`/v1/{chainId:number}/assets/{address:string}`
```
  </div>

  <div>

```ts
type Params = {
  chainId: number;
  addrses: string;
};
```
  </div>

  <div>

```ts
`/v1/ve-pendle/statistics`
```
  </div>

  <div>

```ts
type Params = {};
```
  </div>
</div>

---
hideInToc
---

# Step 1. Primitive type mapping (again)
```ts
type MapPrimitiveType<T> =
    T extends 'string' ? string
  : T extends 'number' ? number
  : T extends 'boolean' ? boolean
  : never;
```

---
hideInToc
---

## Step 2. Map part to object

```ts
type MapPartToObject<Part extends string> =
    Part extends `{${infer Prop}:${infer Type}}`
  ? { [key in Prop]: MapPrimitiveType<Type> }  // The same as `Record<Prop, MapPrimitiveType<Type>>`
  : {};
```

Examples:

```ts
type ChainId = MapPartToObject<'{chainId:number}'>;  // { chainId: number }
type Address = MapPartToObject<'{address:string}'>;  // { address: string}

type NoParamPart = MapPartToObject<'tokens'>;  // {}
```

---
hideInToc: true
---

## Step 3. Divide and conquer

```ts
type ParseURLParams<URL extends string> =
    URL extends `${infer Part}/${infer Rest}`
  ? MapPartToObject<Part> & ParseURLParams<Rest>
  : MapPartToObject<URL>;
```

---
hideInToc: true
---

## Result

<iframe 
  width="900"
  height="450"
  src="https://www.typescriptlang.org/play?#code/C4TwDgpgBACghgJzgWwM4EYoF5aNRAVQCUAZeJNAHgHIB6AN3VoG8BjACzgEsA7ASQAmALh4BXZACMICAL604qfMFTUAfAG4AULVpQ9APQD8m0JFwVUAJmzn8xMohSoaDJm069BI8VNnzFEMoscAICCBCKQqjACLwA5jJqWjp6UEYm4NAAQoHA0uRO1jjkdqQFaDAA9tEwCJWsEc50jCwc3PzCYpLScgpKqMGh4ZHRsTwJSdq6Bsam0OWoAMw2JYRljlTNTPQQALSQPAIANhC00XDAXNFcrCoaU6npms9zUACycGC1XMhclzsAFUylABqmwmlSUABUAgAA88odUFBqKN4tQoIYoKjxhCoEIoTD4RBEciur50ZiydJcfjoXCEQIkdQJJVKic4DwKVAWWyIByaVAeBAdggtBkzB8vohgADKgB5CQAKwgrGAlHIwEJDKR2LiYKwuL0Gq1xMZUAABswACTMXgAM2ksDqYBkQht9sdQMgMhk5txmOYUAA2gBrCAgKC8J2VMAAXXxku+v3+EC9EBBmTBMgFzBkYteq3sC0o9hNJN1+sNUFL9NNSPN7p4DoQ5mAckbzagRAibb9ekxielsoVytV6ulYIAZLY1g4LJRu9FVALBwgZfKlSq1fZ7uL5nhZwsqjU6g1FCXSGWzRWbABhI6VIXjhClOdOC8kVS7173x-pgAaV5IpUm6qvqUCBkGADSkY8FAYYgJUdpQP+8YodBsbqFAMhAA"
/>

---

## Capabilities of Typescript

Typescript type system is **Turing complete**!

<div grid grid-cols-2 grid-gap-5>
<div v-click>

| **Normal programming language** | **Typescript Type system** |
| :-------------------------- | :--------------------- |
| Branching                   | Conditional typing              |
| Assignment                  | Conditional typing with `infer` |
| Function                    | Generic type                    |
| Loop                        | Recursive generic type          |
</div>

<div v-click>

### Proof

Brainfuck intepreter in Typescript type system:
- https://github.com/susisu/typefuck

```ts
import type { Brainfuck } from "@susisu/typefuck";

type Program = ">,[>,]<[.<]";
type Input = "Hello, world!";

// = "!dlrow ,olleH"
type Output = Brainfuck<Program, Input>; 
```

- https://github.com/QuiiBz/tsfuck
</div>

</div>



---
layout: quote
transition: pendle-section
---

## There are still more!
Check out https://www.typescriptlang.org/docs/handbook/2/types-from-types.html


---
layout: PendleSection
transition: pendle-section
---

# Thank you for your attention! <div fw100 transiton-400 animate-bounce-alt animate-count-infinite animate-duration-1s inline-block>💖</div>