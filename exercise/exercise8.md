# Exercise 8

```
Define type PowerUser which should have all fields
from both User and Admin (except for type),
and also have type 'powerUser' without duplicating
all the fields in the code.

- 'PowerUser'의 타입을 지정해라 (얘는 type을 제외하고 User와 Admin의 모든 fields를 가지고 있다)
- type 'powerUser'을 지정할 때 코드의 중복 없이 해라
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

type PowerUser = unknown;

export type Person = User | Admin | PowerUser;

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
  {
    type: 'powerUser',
    name: 'Nikki Stone',
    age: 45,
    role: 'Moderator',
    occupation: 'Cat groomer',
  },
];

function isAdmin(person: Person): person is Admin {
  return person.type === 'admin';
}

function isUser(person: Person): person is User {
  return person.type === 'user';
}

function isPowerUser(person: Person): person is PowerUser {
  return person.type === 'powerUser';
}

export function logPerson(person: Person) {
  let additionalInformation: string = '';
  if (isAdmin(person)) {
    additionalInformation = person.role;
  }
  if (isUser(person)) {
    additionalInformation = person.occupation;
  }
  if (isPowerUser(person)) {
    additionalInformation = `${person.role}, ${person.occupation}`;
  }
  console.log(`${person.name}, ${person.age}, ${additionalInformation}`);
}

console.log('Admins:');
persons.filter(isAdmin).forEach(logPerson);

console.log();

console.log('Users:');
persons.filter(isUser).forEach(logPerson);

console.log();

console.log('Power users:');
persons.filter(isPowerUser).forEach(logPerson);
```

```ts
// solution
// PowerUser는 User와 Admin의 모든 필드를 type을 제외하고 갖고 있으므로
// type을 Omit을 이용해 제거해주고 extends나 &를 통해 확장해주면 된다.

// 나는 interface 씀,,
interface PowerUser extends Omit<User, 'type'>, Omit<Admin, 'type'> {
  type: 'powerUser';
}

// 답지는 type 씀,,
type PowerUser = Omit<User, 'type'> &
  Omit<Admin, 'type'> & {
    type: 'powerUser';
  };
```

## keyword

- [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) - `Omit`
