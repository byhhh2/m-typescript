# Type alias vs interface

## ✨ 차이점

- `type`은 새 프로퍼티를 추가하도록 개방될 수 없지만, `interface`는 확장 가능
- `type`은 교집합을 통해 확장가능, 생성된 뒤 바꾸는 건 불가능
- `interface`는 선언 병합(Declaration Merging)이 가능하지만 `type`은 불가능 (선언적 확장)

### `interface` 확장

```ts
//interface 확장

interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;
```

<br>

### `type alias` 확장

```ts
//type 확장

type Animal = {
  name: string;
};

type Bear = Animal & {
  honey: Boolean;
};

const bear = getBear();
bear.name;
bear.honey;
```

<br>

### `type alias`도 `interface`에 `extends`, `class`에 `implements` 키워드를 사용할 수 있다.

```ts
type Human = {
  name: string;
  age: number;
};

interface Crew extends Human {
  nickname: string;
}

const crew1 = {
  name: '변영화',
  age: 25,
  nickname: '무비',
};

function isCrew(input: any): input is Crew {
  return (
    typeof input.name === 'string' &&
    typeof input.age === 'number' &&
    typeof input.nickname === 'string'
  );
}

console.log(isCrew(crew1));
```

<br>

### 선언 병합(Declaration Merging) - `interface`만 가능

```ts
// 🌴 interface

interface Crew {
  name: string;
  age: number;
}

interface Crew {
  nickname: string;
}

function isCrew(input: any): input is Crew {
  return (
    typeof input.name === 'string' &&
    typeof input.age === 'number' &&
    typeof input.nickname === 'string'
  );
}

const crew1 = {
  name: '변영화',
  age: 25,
  nickname: '무비',
};
```

```ts
// 🌴 type alias

type Crew = {
  name: string;
  age: number;
};

type Crew = {
  nickname: string;
};

// Error : Duplicate identifier 'Crew'.
```

### computed value - `type`만 가능

```ts
type names = 'firstName' | 'lastName';

type NameTypes = {
  [key in names]: string;
};

const yc: NameTypes = { firstName: 'hi', lastName: 'yc' };

interface NameInterface {
  // error
  [key in names]: string;
}
```

## 참고

- https://yceffort.kr/2021/03/typescript-interface-vs-type
- https://medium.com/humanscape-tech/type-vs-interface-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%BB%EA%B2%8C-f36499b0de50
