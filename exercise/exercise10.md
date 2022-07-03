# Exercise 10

```
We don't want to reimplement all the data-requesting
functions. Let's decorate the old callback-based
functions with the new Promise-compatible result.
The final function should return a Promise which
would resolve with the final data directly
(i.e. users or admins) or would reject with an error
(or type Error).

The function should be named promisify.

- request function을 다시 모두 구현 할 필요는 없다.
- 새로운 promise 호환되는 callback 함수를 decorate하자.
- 마지막 함수는 promise를 반환한다. (resolve with users or admins / reject error)

[심화]

Create a function promisifyAll which accepts an object
with functions and returns a new object where each of
the function is promisified.

Rewrite api creation accordingly:

const api = promisifyAll(oldApi);

- 모르겠어서 못 품 ㅋㅋ
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

type Person = User | Admin;

const admins: Admin[] = [
  { type: 'admin', name: 'Jane Doe', age: 32, role: 'Administrator' },
  { type: 'admin', name: 'Bruce Willis', age: 64, role: 'World saver' },
];

const users: User[] = [
  {
    type: 'user',
    name: 'Max Mustermann',
    age: 25,
    occupation: 'Chimney sweep',
  },
  { type: 'user', name: 'Kate Müller', age: 23, occupation: 'Astronaut' },
];

export type ApiResponse<T> =
  | {
      status: 'success';
      data: T;
    }
  | {
      status: 'error';
      error: string;
    };

export function promisify(arg: unknown): unknown {
  return null;
}

const oldApi = {
  requestAdmins(callback: (response: ApiResponse<Admin[]>) => void) {
    callback({
      status: 'success',
      data: admins,
    });
  },
  requestUsers(callback: (response: ApiResponse<User[]>) => void) {
    callback({
      status: 'success',
      data: users,
    });
  },
  requestCurrentServerTime(callback: (response: ApiResponse<number>) => void) {
    callback({
      status: 'success',
      data: Date.now(),
    });
  },
  requestCoffeeMachineQueueLength(
    callback: (response: ApiResponse<number>) => void
  ) {
    callback({
      status: 'error',
      error: 'Numeric value has exceeded Number.MAX_SAFE_INTEGER.',
    });
  },
};

export const api = {
  requestAdmins: promisify(oldApi.requestAdmins),
  requestUsers: promisify(oldApi.requestUsers),
  requestCurrentServerTime: promisify(oldApi.requestCurrentServerTime),
  requestCoffeeMachineQueueLength: promisify(
    oldApi.requestCoffeeMachineQueueLength
  ),
};

function logPerson(person: Person) {
  console.log(
    ` - ${person.name}, ${person.age}, ${
      person.type === 'admin' ? person.role : person.occupation
    }`
  );
}

async function startTheApp() {
  console.log('Admins:');
  (await api.requestAdmins()).forEach(logPerson);
  console.log();

  console.log('Users:');
  (await api.requestUsers()).forEach(logPerson);
  console.log();

  console.log('Server time:');
  console.log(
    `   ${new Date(await api.requestCurrentServerTime()).toLocaleString()}`
  );
  console.log();

  console.log('Coffee machine queue length:');
  console.log(`   ${await api.requestCoffeeMachineQueueLength()}`);
}

startTheApp().then(
  () => {
    console.log('Success!');
  },
  (e: Error) => {
    console.log(
      `Error: "${e.message}", but it's fine, sometimes errors are inevitable.`
    );
  }
);
```

```ts
// solution

// promisify : promise를 반환하는 함수를 반환해야한다.
export function promisify<T>(
  arg: (callback: (response: ApiResponse<T>) => void) => void
): () => Promise<T> {
  return () =>
    new Promise((resolve, reject) => {
      // arg를 호출해서 response를 이용해 resolve, reject를 한다.
      arg(response => {
        if (response.status === 'success') {
          resolve(response.data);
        } else {
          reject(new Error(response.error));
        }
      });
    });
}
```

---

### Promise

```js
// Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
// Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
// Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

// ex1)
function getData() {
  return new Promise(function (resolve, reject) {
    let data = 100;

    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function (resolvedData) {
  console.log(resolvedData); // 100
});

// ex2)
function getData() {
  return new Promise(function (resolve, reject) {
    fetch(API_URL)
      .then(response => response.json())
      .then(data => resolve(data));
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function (resolvedData) {
  console.log(resolvedData); // API response
});

// ex3) reject
function getData() {
  return new Promise(function (resolve, reject) {
    fetch(API_URL)
      .then(response =>
        response ? response.json() : reject(new Error('request is failed'))
      )
      .then(data => resolve(data));
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData()
  .then(function (resolvedData) {
    console.log(resolvedData);
  })
  .catch(error => console.log(error));
```

---

## keyword

- generics

## 참고

- https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
