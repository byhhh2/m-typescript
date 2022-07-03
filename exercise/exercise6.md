# Exercise 6

```
Fix typing for the filterPersons so that it can filter users and return User[] when personType='user' and return Admin[] when personType='admin'. Also filterPersons should accept partial User/Admin type according to the personType. `criteria` argument should behave according to the`personType` argument value. `type` field is not allowed in the `criteria` field.

- filterPersons에서 personType이 'user'일 때는 User[]를 반환하도록, personType이 'admin'일 때는 Admin[]을 반환하도록 수정해준다.
- filterPersons은 personType에 따라 partial User/Admin type을 accept해야한다.
- 'criteria'인자는 'personType'인자 값에 따라 처리되어야 한다.
- 'type' field는 'criteria' field에 허용되지 않는다.

[심화]

Implement a function `getObjectKeys()` which returns more
convenient result for any argument given, so that you don't
need to cast it.

let criteriaKeys = Object.keys(criteria) as (keyof User)[];
-->
let criteriaKeys = getObjectKeys(criteria);

- `getObjectKeys()`를 구현해서 criteria에 맞는 criteriaKeys를 편리하게 반환할 수 있도록 해보자.
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
  { type: 'admin', name: 'Jane Doe', age: 32, role: 'Administrator' },
  { type: 'user', name: 'Kate Müller', age: 23, occupation: 'Astronaut' },
  { type: 'admin', name: 'Bruce Willis', age: 64, role: 'World saver' },
  { type: 'user', name: 'Wilson', age: 23, occupation: 'Ball' },
  { type: 'admin', name: 'Agent Smith', age: 23, role: 'Anti-virus engineer' },
];

export function logPerson(person: Person) {
  console.log(
    ` - ${person.name}, ${person.age}, ${
      person.type === 'admin' ? person.role : person.occupation
    }`
  );
}

export function filterPersons(
  persons: Person[],
  personType: string,
  criteria: unknown
): unknown[] {
  return persons
    .filter(person => person.type === personType)
    .filter(person => {
      /*
        index.ts(65,44): error TS2769: No overload matches this call.
  Overload 1 of 2, '(o: {}): string[]', gave the following error.
    Argument of type 'unknown' is not assignable to parameter of type '{}'.
  Overload 2 of 2, '(o: object): string[]', gave the following error.
    Argument of type 'unknown' is not assignable to parameter of type 'object'.
      */
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every(fieldName => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}

export const usersOfAge23 = filterPersons(persons, 'user', { age: 23 });
export const adminsOfAge23 = filterPersons(persons, 'admin', { age: 23 });

console.log('Users of age 23:');
usersOfAge23.forEach(logPerson);

console.log();

console.log('Admins of age 23:');
adminsOfAge23.forEach(logPerson);
```

```ts
// solution

// overloads
// function signatures
// 1️⃣ filterPersons에서 personType이 'user'일 때는 User[]를 반환하도록, personType이 'admin'일 때는 Admin[]을 반환하도록 수정해준다.
// filterPersons은 personType에 따라 partial User/Admin type을 accept해야한다.
// 'criteria'인자는 'personType'인자 값에 따라 처리되어야 한다.
export function filterPersons(
  persons: Person[],
  personType: 'admin',
  criteria: Partial<Admin>
): Admin[];
export function filterPersons(
  persons: Person[],
  personType: 'user',
  criteria: Partial<User>
): User[];
export function filterPersons(
  persons: Person[],
  personType: string,
  criteria: Partial<Person>
): Person[] {
  return persons
    .filter(person => person.type === personType)
    .filter(person => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every(fieldName => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}

// 2️⃣ 'type' field는 'criteria' field에 허용되지 않는다.
// 'Omit'을 통해 'type' field를 제거해준다.
export function filterPersons(
  persons: Person[],
  personType: 'admin',
  criteria: Partial<Omit<Admin, 'type'>>
): Admin[];
export function filterPersons(
  persons: Person[],
  personType: 'user',
  criteria: Partial<Omit<User, 'type'>>
): User[];
export function filterPersons(
  persons: Person[],
  personType: string,
  criteria: Partial<Person>
): Person[] {
  return persons
    .filter(person => person.type === personType)
    .filter(person => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every(fieldName => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}

// 3️⃣ [심화] `getObjectKeys()`를 구현해서 criteria에 맞는 criteriaKeys를 편리하게 반환할 수 있도록 해보자.

const getObjectKeys = <T>(obj: T) => Object.keys(obj) as (keyof T)[];

export function filterPersons(
  persons: Person[],
  personType: 'admin',
  criteria: Partial<Omit<Admin, 'type'>>
): Admin[];
export function filterPersons(
  persons: Person[],
  personType: 'user',
  criteria: Partial<Omit<User, 'type'>>
): User[];
export function filterPersons(
  persons: Person[],
  personType: string,
  criteria: Partial<Person>
): Person[] {
  return persons
    .filter(person => person.type === personType)
    .filter(person => {
      let criteriaKeys = getObjectKeys(criteria); // 3️⃣
      return criteriaKeys.every(fieldName => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

## keyword

- [function overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)
- [generics](https://www.typescriptlang.org/ko/docs/handbook/2/generics.html)
