# Exercise 5

```
- 데이터 필터링
- 기준에 일치하는 항목만 반환할 것

- 타입 구조를 복제하지 말고, filterUsers함수 정의만 수정해볼 것
- [심화] Exclude "type" from filter criterias
```

```ts
interface User {
  type: 'user';
  name: string;
  age: number;
  occupation: string;
}

interface Admin {
  type: 'admin';
  name: string;
  age: number;
  role: string;
}

export type Person = User | Admin;

export const persons: Person[] = [
  {
    type: 'user',
    name: 'Max Mustermann',
    age: 25,
    occupation: 'Chimney sweep',
  },
  {
    type: 'admin',
    name: 'Jane Doe',
    age: 32,
    role: 'Administrator',
  },
  {
    type: 'user',
    name: 'Kate Müller',
    age: 23,
    occupation: 'Astronaut',
  },
  {
    type: 'admin',
    name: 'Bruce Willis',
    age: 64,
    role: 'World saver',
  },
  {
    type: 'user',
    name: 'Wilson',
    age: 23,
    occupation: 'Ball',
  },
  {
    type: 'admin',
    name: 'Agent Smith',
    age: 23,
    role: 'Administrator',
  },
];

export const isAdmin = (person: Person): person is Admin =>
  person.type === 'admin';
export const isUser = (person: Person): person is User =>
  person.type === 'user';

export function logPerson(person: Person) {
  let additionalInformation = '';

  if (isAdmin(person)) {
    additionalInformation = person.role;
  }
  if (isUser(person)) {
    additionalInformation = person.occupation;
  }
  console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}

export function filterUsers(persons: Person[], criteria: User): User[] {
  return persons.filter(isUser).filter(user => {
    const criteriaKeys = Object.keys(criteria) as (keyof User)[];

    return criteriaKeys.every(fieldName => {
      return user[fieldName] === criteria[fieldName];
    });
  });
}

console.log('Users of age 23:');

filterUsers(persons, {
  age: 23,
  /* Error : Argument of type '{ age: number; }' is not assignable to parameter of type 'User'.
  Type '{ age: number; }' is missing the following properties from type 'User': type, name, occupation */
}).forEach(logPerson);
```

```ts
// persons 배열에서 User type만 filter해서,
// criteria 변수로 들어온 객체로 다시 filter한다.

// 1️⃣ 문제? criteria로 User type인 변수를 넣어줄건데, 어떤 속성이 있을지 모른다.
// 만약 age 속성만 가진 객체를 넣어준다면, type, name, occupation 속성이 없다는 type error가 뜬다.
export function filterUsers(
  persons: Person[],
  criteria: Partial<User>
  // Partial을 써주면 모든 type의 속성이 optional하게 해준다.
  // type Partial<T> = { [P in keyof T]?: T[P] | undefined; }
): User[] {
  return persons.filter(isUser).filter(user => {
    const criteriaKeys = Object.keys(criteria) as (keyof User)[];

    return criteriaKeys.every(fieldName => {
      return user[fieldName] === criteria[fieldName];
    });
  });
}

// 2️⃣ 문제? [심화] Exclude "type" from filter criterias
// type 속성을 filter 기준에서 제외해라

export function filterUsers(
  persons: Person[],
  criteria: Partial<Omit<User, 'type'>>
  // 'type' 속성을 제거해서 타입 지정
): User[] {
  return persons.filter(isUser).filter(user => {
    const criteriaKeys = Object.keys(criteria) as (keyof Omit<User, 'type'>)[];

    return criteriaKeys.every(fieldName => {
      return user[fieldName] === criteria[fieldName];
    });
  });
}
```

## keyword

- [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) - `Partial`
- `Omit` : 특정 속성만 제거한 타입을 정의합니다. - [참고](https://kyounghwan01.github.io/blog/TS/fundamentals/utility-types/#omit)
