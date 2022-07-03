## `in`

- act as type guard

```ts
interface A {
  x: number;
}

interface B {
  y: string;
}

let q: A | B = ...;


if ('x' in q) {
  // q: A
} else {
  // q: B
}
```

## 참고

- https://stackoverflow.com/questions/50214731/what-does-the-in-keyword-do-in-typescript
