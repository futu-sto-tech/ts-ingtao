# TS-INGTAO TypeScript Lab


## Type-driven workflow

A type-driven workflow means that you think of coding as a type transformations, moving from one type to another through your system. This approach is very common in strictly typed functional languages such as Haskell, OCaml and Rust. What it boils down to is that you first try to layout your problem using types before actually implementing anything. By doing this you will provide `tsc`, the TypeScript compiler, with information it can use to help guide you towards the correct implementation. In order for this to work there are a few rules you should adhere to:

- Do not use `any`.
- Always provide type annotations for function parameters AND return value. The return value is especially helpful since the compiler will be able to tell you if you are returning the wrong thing as you code.
- Use `interface`s, `type`s and `enum`s to model your business logic. `enum` are especially helpful for ensuring input data is correct.

### TypeScript configuration

Ensure strict type-checking rules by setting `strict: true` in the `compilerOptions` section of `tsconfig.json`. This will ensure that the following flags are enabled:

- `noImplicitAny`
- `alwaysStrict`
- `noImplicitThis`
- `strictBindCallApply`
- `strictFunctionTypes`
- `strictNullChecks`
- `strictPropertyInitialization`

Read more in the [compiler options documentation](https://www.typescriptlang.org/docs/handbook/compiler-options.html).

If you are migrating an existing JS project to TypeScript these settings might be too strict.

## Working with types

### Union types: X or Y

### Optional properties and arguments

### Type aliases

### `type` vs `interface`

An `interface` allows extension after it has been created, but `type` does not:

```typescript
interface Book {
  title: string
}

interface Book {
  author: string
}

const book: Book = {
  title: 'Jitterbug Perfume',
  author: 'Tom Robbins'
}
```

```typescript
type Book = {
  title: string
}

type Book = {
  author: string
}

// Error: Duplicate identifier 'Book'.
```

## Enums and other closed types

### Creating an object from an `enum`

```typescript
enum Country {
  SE = 'se',
  FI = 'fi',
}

type Config = {
  [k in Country]: { currency: string; apiKey: string }
}

const config: Config = {
  [Country.SE]: { currency: 'SEK', apiKey: 'v3rys3kr3t' },
  [Country.FI]: { currency: 'EUR' },
};
```

### Exhaustiveness checks

This only works if you explicitly specify the return type!

```typescript
enum Size {
  S,
  M,
  L,
}

// The below examples only work if you specify the return type!!

// Type-checks because it is exhaustive
function getPrice(size: Size): number {
  switch (size) {
    case Size.S:
      return 1;
    case Size.M:
      return 5;
    case Size.L:
      return 10;
  }
}

// Doesn't type-check because non-exhausitve
function toString(size: Size): string {
  switch (size) {
    case Size.S:
      return 'Small';
    case Size.M:
      return 'Medium';
  }
}
```


## Extending existing types

### Manipulate existing types with utility types

Useful for extending interfaces and types in your project.

<https://www.typescriptlang.org/docs/handbook/utility-types.html>

1.  Partial

2.  Pick

3.  Omit

4.  ReturnType

5.  Readonly

### Extend using `declare`

Useful when you need to extend global objects such as `Window`.

```typescript
declare global {
  interface Window {
    tvty?: Function;
  }
}
```


## Generic types

### Generic constraints with `extends`

```typescript
function getTitle<T extends { title: string }>(entry: T) {
  return entry.title;
}

interface Book {
  title: string
  author: string
}

const book: Book = {
  title: 'Sommarskuggan och kalasbuset',
  author: 'Tina Mackic',
}

// OK
const title = getTitle(book);

// Error
const title = getTitle({ foo: 'bar' });
```

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
```


## Conditional types
