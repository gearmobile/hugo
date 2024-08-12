+++
title = "TS - Index Signature"
date = "2024-08-10"
tags = ["typescript", "index signature"]
categories = ["Development"]
+++

## Зачем Index Signature

Идея Index Signature заключается в типизировании объектов **неизвестной структуры** - когда известны только *тип ключа* и *тип значения*.

Пример использования Index Signature:

```typescript
function totalSalary(salaryObject: { [key: string]: number }) {
  let total = 0;
  for (const name in salaryObject) {
    total += salaryObject[name];
  }
  return total;
}
```

... `{ [key: string]: number }` — это Index Signature, которая сообщает TypeScript, что параметр `salaryObject` должен быть объектом с типом `string` в качестве ключа и типом `number` в качестве значения.

Функция `totalSalary` не примет объект, который имеет, например, *строки* в качестве значений:

```typescript
const salary3 = {
  baseSalary: '100 thousands'
};

// Type error:
// Argument of type '{ baseSalary: string; }' is not assignable to parameter of type '{ [key: string]: number; }'.
//   Property 'baseSalary' is incompatible with index signature.
//     Type 'string' is not assignable to type 'number'.
totalSalary(salary3);
```

## Синтаксис Index Signature

Синтаксис Index Signature очень простой и похож на синтаксис свойства, но с одним отличием - в квадратных скобках указывается тип ключа:

```typescript
{ [key: KeyType]: ValueType }
```

Типом ключа в Index Signature может быть только - `string`, `number` или `symbol`. Использование любого другого типа не допускается:

```typescript
interface Dictionary {
  // Type error: 
  // An index signature parameter type must be 'string', 'number', 
  // 'symbol', or a template literal type.  
  [key: boolean]: string;
}
```

## Несколько примеров

Ключ и значение - оба имеют тип `string`:

```typescript
interface StringByString {
  [key: string]: string;
}

const heroesInBooks: StringByString = {
  'Gunslinger': 'The Dark Tower',
  'Jack Torrance': 'The Shining'
};
```

Ключ имеет тип `string`, значение может иметь тип - `string`, `number` или `boolean`:

```typescript
interface Options {
  [key: string]: string | number | boolean;
  timeout: number;
}

const options: Options = {
  timeout: 1000,
  timeoutMessage: 'The request timed out!', // <- index signature case
  isFileUpload: false // <- index signature case
};
```

В этом примере стоит обратить внимание на строку `timeout: number` - видим, что Index Signature может комбинироваться с обычным объявлением свойства, как в случае с `timeout`.

## Ссылки

* [Index Signatures in TypeScript](https://dmitripavlutin.com/typescript-index-signatures/)