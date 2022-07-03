# Exercise 13

```
Check date-wizard module implementation at:
node_modules/date-wizard/index.js
node_modules/date-wizard/index.d.ts

Extend type declaration of that module in:
module-augmentations/date-wizard/index.ts
```

```ts
// solution
// module-augmentations/date-wizard/index.d.ts

import 'date-wizard';

declare module 'date-wizard' {
  // Add your module extensions here.

  export function pad(s: number): string;
  interface DateDetails {
    hours: number;
    minutes: number;
    seconds: number;
  }
}

/*
// node_modules/date-wizard/index.d.ts에서 부족한 부분 채움 

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
*/
```
