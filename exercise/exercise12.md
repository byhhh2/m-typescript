# Exercise 12

```
Check stats module implementation at:
node_modules/stats/index.js
node_modules/stats/README.md

Provide type declaration for that module in:
declarations/stats/index.d.ts

Higher difficulty bonus exercise:

Avoid duplicates of type declarations.

- 모듈의 타입 선언을 제공하면됨.
```

```ts
// solution
// declarations/stats/index.d.ts

declare module 'stats' {
  type Comparator<T> = (a: T, b: T) => number;

  export function getMaxIndex<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): number;
  export function getMaxElement<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): T | null;
  export function getMinIndex<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): number;
  export function getMinElement<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): T | null;
  export function getMedianIndex<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): number;
  export function getMedianElement<T>(
    input: Array<T>,
    comparator: Comparator<T>
  ): T | null;
  export function getAverageValue<T>(
    input: Array<T>,
    getValue: (item: T) => number
  ): number | null;
}

// 답지
declare module 'stats' {
  type Comparator<T> = (a: T, b: T) => number;

  type GetElement = <T>(input: Array<T>, comparator: Comparator<T>) => T | null;
  type GetIndex = <T>(input: Array<T>, comparator: Comparator<T>) => number;

  export const getMaxIndex: GetIndex; // function -> const
  export const getMaxElement: GetElement;
  export const getMinIndex: GetIndex;
  export const getMinElement: GetElement;
  export const getMedianIndex: GetIndex;
  export const getMedianElement: GetElement;
  export function getAverageValue<T>(
    input: Array<T>,
    getValue: (item: T) => number
  ): number | null;
}
```
