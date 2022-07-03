# Exercise 11

```
Check str-utils module implementation at:
node_modules/str-utils/index.js
node_modules/str-utils/README.md

Provide type declaration for that module in:
declarations/str-utils/index.d.ts

Try to avoid duplicates of type declarations,
use type aliases.

- 모듈의 타입선언을 제공해라 (declarations/str-utils/index.d.ts)
- 타입선언에 중복을 피하고, type alias를 사용해라
```

```ts
// declarations/str-utils/index.d.ts
// node_modules/str-utils/index.js에서 타입힌트 받아서 써주면 됨

declare module 'str-utils' {
  // export const ...
  // export function ...

  // strReverse
  export function strReverse(value: string): string;
  // strToLower
  export function strToLower(value: string): string;
  // strToUpper
  export function strToUpper(value: string): string;
  // strRandomize
  export function strRandomize(value: string): string;
  // strInvertCase
  export function strInvertCase(value: string): string;
}

// 답지
// 타입선언에 중복을 피하고, type alias를 사용해라
type StrUtil = (input: string) => string;

export const strReverse: StrUtil;
export const strToLower: StrUtil;
export const strToUpper: StrUtil;
export const strRandomize: StrUtil;
export const strInvertCase: StrUtil;
```

## keyword

- [ambient-modules](https://www.typescriptlang.org/docs/handbook/modules.html#ambient-modules)
