+++
title = "TS - Record"
date = "2024-08-09"
tags = ["typescript", "utility types"]
categories = ["Development"]
draft = true
+++

Record (запись) помогает определять словари, также называемые парами ключ/значение, с фиксированным типом для ключей и фиксированным типом для значений.
Другими словами, тип `Record` позволяет определить тип словаря, то есть имена и типы его ключей.

Сила типа `Record` в том, что с его помощью можно моделировать словари с фиксированным числом ключей.

В TypeScript Record имеют фиксированное количество членов (т.е. фиксированное количество полей), и эти члены обычно идентифицируются по имени. Это основное отличие записей от кортежей.

Record<Keys, Type> - строит **объектный** тип, ключами свойств которого являются `Keys`, а значениями свойств — `Type`.

## Record для создания модели университетских курсов

```typescript
type Course = "Computer Science" | "Mathematics" | "Literature";

interface CourseInfo {
    professor: string;
    cfu: number;
}

const courses: Record<Course, CourseInfo> = {
    "Computer Science": {
        professor: "Mary Jane",
        cfu: 12
    },
    "Mathematics": {
        professor: "John Doe",
        cfu: 12
    },
    "Literature": {
        professor: "Frank Purple",
        cfu: 12
    }
}
```

В данном примере мы определили тип `Course` - для ключей `Keys` словаря `courses`. И интерфейс `CourseInfo` - который будет определять тип значений свойств `Type` для словаря `courses`.

## Идентификация недостающих свойств

Typescript проверяет в создаваемом словаре - наличие всех заранее определенных ключей `Keys`:

```typescript
type Course = "Computer Science" | "Mathematics" | "Literature";
```

... и если при создании словаря будет *пропущен* один из обязательных ключей - например `Literature`:

```typescript
const courses: Record<Course, CourseInfo> = {
    "Computer Science": {
        professor: "Mary Jane",
        cfu: 12
    },
    "Mathematics": {
        professor: "John Doe",
        cfu: 12
    }
}
```

... то появится ошибка с предупреждением об отсутствии обязательного ключа `Literature` в словаре:

```typescript
Property 'Literature' is missing in type '{ "Computer Science": { professor: string; cfu: number; }; Mathematics: { professor: string; cfu: number; }; }' but required in type 'Record<Course, CourseInfo>'.(2741)
```
Также Typescript проверяет, чтобы в создаваемом словаре не было ключей `Keys`, которые не определены заранее. То есть, если сделать так:

```typescript
const courses: Record<Course, CourseInfo> = {
    "Computer Science": {
        professor: "Mary Jane",
        cfu: 12
    },
    "Mathematics": {
        professor: "Mary Lou",
        cfu: 12
    },
    "Literature": {
        professor: "Frank Purple",
        cfu: 12
    },
    "Geography": {
        professor: "Ivan Drago",
        cfu: 13
    }
}
```

... то получим ошибку с предупреждением, что создаваемый словарь может содержать только известные свойства и ключ `Geography` не существует в типе `Course`:

```typescript
Object literal may only specify known properties, and '"Geography"' does not exist in type 'Record<Course, CourseInfo>'.(2353)
(property) "Geography": {
    professor: string;
    cfu: number;
}
```

## Доступ к данным Record

Получить значение из словаря можно по его ключу. Например, если обратиться к ключу `Literature`:

```typescript
courses['Mathematics']
```

... получим значение, храняящееся в нем:

```typescript
{
    professor: "Mary Lou",
    cfu: 12
}
```

## Allowed Types for Keys

The types for `Keys` can only be `number`, `string` and `symbol`. Types other than these throw `2344` error at definition:

```typescript
type numberedUser = Record<number, TUser>;
type stringUser = Record<string, TUser>;
type symbolUser = Record<symbol, TUser>;

type booleanUser = Record<boolean, TUser>; // Type 'boolean' does not satisfy the constraint 'string | number | symbol'
type booleanUser = Record<object, TUser>; // Type 'object' does not satisfy the constraint 'string | number | symbol'
```

## Allowed Types for Values

Types for values can be of any type. objects and function types are common. This means, they can also be React components. Below, we see a React example.

For example, the record parameter is a bit easier to grasp than the index signature parameter:

```typescript
function logSalary1(salary: Record<string, number>) {
  console.log(salary)
}

function logSalary2(salary: { [key: string]: number }) {
  console.log(salary)
}
```

[](https://dmitripavlutin.com/typescript-record/)