# `is`

- `parameterName is Type` 형식
- 어떤 변수와 호출될 때마다 TypeScript는 원래 type과 호환되는 경우 해당 변수를 특정 type으로 좁힌다.

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
```

## 참고

- https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates
