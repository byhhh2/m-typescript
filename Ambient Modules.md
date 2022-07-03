# Ambient Modules

- 써드파티 라이브러리의 타입선언
- 만약 써드파티 라이브러리에서 타입선언을 지원하지 않는 경우 직접 `index.d.ts`를 작성해 해결할 수 있다.
- `index.d.ts`의 기본 선언 방식을 앰비어트 선언(ambient declarations)라고 한다.
- 모듈의 타입을 설명하지만 구현은 포함하지 않는 파일
- `npm install --save-dev @types/node`

<br>

- `declare` : 모듈 선언
- `namespace` : 일종의 패키지, 클래스 • 인터페이스 • 함수 같은 내용을 한 파일에서 그룹화하여 관리

```ts
// index.d.ts
declare function dateWizard(date: Date, format: string): string;

declare namespace dateWizard {
  // namespace
  interface DateDetails {
    year: number;
    month: number;
    date: number;
  }

  function dateDetails(date: Date): DateDetails;
  function utcDateDetails(date: Date): DateDetails;
}

export = dateWizard;

// index.js
function dateWizard(date, format) {
  var details = dateWizard.dateDetails(date);
  return format.replace(/{([^}]+)}/g, function (s, match) {
    return dateWizard.pad(details[match]);
  });
}

dateWizard.pad = function (s) {
  s = String(s);
  while (s.length < 2) {
    s = '0' + s;
  }
  return s;
};
```

## `import`

```ts
// node_modules/date-wizard/index.d.ts

declare function dateWizard(date: Date, format: string): string;

declare namespace dateWizard {
  interface DateDetails {
    year: number;
    month: number;
    date: number;
  }

  function dateDetails(date: Date): DateDetails;
  function utcDateDetails(date: Date): DateDetails;
}

export = dateWizard;

// module-augmentations/date-wizard/index.d.ts

import 'date-wizard';

declare module 'date-wizard' {
  export function pad(s: number): string;
  interface DateDetails {
    hours: number;
    minutes: number;
    seconds: number;
  }
}
```

## 참고

- https://itchallenger.tistory.com/488
- https://soft91.tistory.com/74
- https://lts0606.tistory.com/25
