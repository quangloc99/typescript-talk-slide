# Why typesystem?

- A type system is a *restriction*. A *good restriction*
  - Think of Rust
- Type system application
  - Compile time validation
  - IDE intellisense
  - Static analysis
  - Reflection
  - Compile time optimization (less relevant for Typescript)
    - There is Google closure compiler (for js with js docs)

# Typescript features

## Strict null check

- tsconfig.json flag
- null and undefined are separate types
- operator ?? and ?=

⇒ never call methods of null again!

## Union type

- literals are also types
- restrict the acceptable values

### Narrowing union types

- Typeof and instanceof
- Control flow analysis
- Discriminated union
- Type guard and assert function

## Type manipulation

(Demo only as it is very long)

- TODO demo

# Typescript library

Showcase only

## Type helper

### Definitelytyped

Microsoft-maintained repo with types for js libraries that do not have typing

### ts-essentials

Type utilities

### ts toolbelt

ts-essentials but bigger

## Runtime validator

### Zod

### arktype

## Metaprogramming

### Meta-types

Arithmetic and logic calculation with literal types

### HOTscript

Functional programming but with types

# Typescript tool

## typescript-eslint

- More linting rule for typescript

## Typerunner

tsc alternative, written in C++