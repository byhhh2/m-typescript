# Exercise 7

```
Implement swap which receives 2 persons and returns them in
the reverse order. The function itself is already
there, actually. We just need to provide it with proper types.
Also this function shouldn't necessarily be limited to just
Person types, lets type it so that it works with any two types
specified.

- 2명의 persons를 받아서 그들의 순서를 바꿔서 반환해라
- 함수는 이미 있지만 적절한 타입을 제공해야한다.
- 또한, 이 함수는 person types에 한정되지 않는다. 두가지 type에 대해서만 작동하도록 해라.
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

function logUser(user: User) {
  const pos = users.indexOf(user) + 1;
  console.log(` - #${pos} User: ${user.name}, ${user.age}, ${user.occupation}`);
}

function logAdmin(admin: Admin) {
  const pos = admins.indexOf(admin) + 1;
  console.log(` - #${pos} Admin: ${admin.name}, ${admin.age}, ${admin.role}`);
}

const admins: Admin[] = [
  {
    type: 'admin',
    name: 'Will Bruces',
    age: 30,
    role: 'Overseer',
  },
  {
    type: 'admin',
    name: 'Steve',
    age: 40,
    role: 'Steve',
  },
];

const users: User[] = [
  {
    type: 'user',
    name: 'Moses',
    age: 70,
    occupation: 'Desert guide',
  },
  {
    type: 'user',
    name: 'Superman',
    age: 28,
    occupation: 'Ordinary person',
  },
];

export function swap(v1, v2) {
  return [v2, v1];
}

function test1() {
  console.log('test1:');
  const [secondUser, firstAdmin] = swap(admins[0], users[1]);
  logUser(secondUser);
  logAdmin(firstAdmin);
}

function test2() {
  console.log('test2:');
  const [secondAdmin, firstUser] = swap(users[0], admins[1]);
  logAdmin(secondAdmin);
  logUser(firstUser);
}

function test3() {
  console.log('test3:');
  const [secondUser, firstUser] = swap(users[0], users[1]);
  logUser(secondUser);
  logUser(firstUser);
}

function test4() {
  console.log('test4:');
  const [firstAdmin, secondAdmin] = swap(admins[1], admins[0]);
  logAdmin(firstAdmin);
  logAdmin(secondAdmin);
}

function test5() {
  console.log('test5:');
  const [stringValue, numericValue] = swap(123, 'Hello World');
  console.log(` - String: ${stringValue}`);
  console.log(` - Numeric: ${numericValue}`);
}

[test1, test2, test3, test4, test5].forEach(test => test());
```

```ts
// solution

// 1️⃣ 2명의 persons를 받아서 그들의 순서를 바꿔서 반환해라
// 함수는 이미 있지만 적절한 타입을 제공해야한다.
export function swap<T, F>(v1: T, v2: F): [F, T] {
  return [v2, v1];
}

// 2️⃣ 또한, 이 함수는 person types에 한정되지 않는다. 두가지 type에 대해서만 작동하도록 해라.
// (이건 답안에 없는 듯 맞는지 모름..)
export function swap<T extends User | Admin, F extends User | Admin>(
  v1: T,
  v2: F
): [F, T] {
  return [v2, v1];
}
```

## keyword

- [generics](https://www.typescriptlang.org/ko/docs/handbook/2/generics.html)
