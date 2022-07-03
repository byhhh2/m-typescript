# Exercise 4

```
Figure out how to help TypeScript understand types in
this situation and apply necessary fixes.
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
];

/*
  함수 반환타입에 is 키워드 활용 
*/
/*
export function isAdmin(person: Person) {
  return person.type === 'admin';
}*/
export function isAdmin(person: Person): person is Admin {
  return person.type === 'admin';
}

/*
export function isUser(person: Person) {
  return person.type === 'user';
}
*/
export function isUser(person: Person): person is User {
  return person.type === 'user';
}

export function logPerson(person: Person) {
  let additionalInformation: string = '';

  if (isAdmin(person)) {
    additionalInformation = person.role;
  }
  if (isUser(person)) {
    additionalInformation = person.occupation;
    // Error : Property 'occupation' does not exist on type 'Person'. Property 'occupation' does not exist on type 'Admin'.
  }
  console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}

console.log('Admins:');
persons.filter(isAdmin).forEach(logPerson);

console.log();

console.log('Users:');
persons.filter(isUser).forEach(logPerson);
```

## keyword

- [type predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)
