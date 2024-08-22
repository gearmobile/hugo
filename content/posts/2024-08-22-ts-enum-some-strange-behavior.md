+++
title = "TS - особенности Enum"
date = "2024-08-22"
tags = ["typescript", "enum"]
categories = ["Development"]
+++

Ниже вольный пересказ [оригинала статьи][1], которая показывает некоторые особенности Enum в Typescript. Итак, у нас есть enum, значениями которого являются числа `number`:

```typescript
enum PackStatusNumber {
  Draft = 0,
  Approved = 1,
  Shipped = 2,
}
```

... и есть enum, значениями которого являются строки `string`:


```typescript
enum PackStatusString {
  Draft = "Draft",
  Approved = "Approved",
  Shipped = "Shipped",
}
```

Ниже рассмотрим несколько особенностей поведения enum в Typescript.

## Сколько ключей в объекте

Давайте подумаем, сколько ключей имеют enum `PackStatusNumber` и `PackStatusString`? Если вывести ключи enum `PackStatusString`:

```typescript
const logStatusString = (status: PackStatusString) => {
  console.log(status);
};

console.log(Object.keys(PackStatusString));
```

... то получим ожидаемый результат - **три** ключа:

```typescript
(3) ["Draft", "Approved", "Shipped"]
0:"Draft"
1:"Approved"
2:"Shipped"
```

... но если сделаем тоже самое для enum `PackStatusNumber`:

```typescript
const logStatusNumber = (status: PackStatusNumber) => {
  console.log(status);
};

console.log(Object.keys(PackStatusNumber));
```

... то результат будет - **шесть**(!) ключей:

```typescript
(6) ["0", "1", "2", "Draft", "Approved",...]
0:"0"
1:"1"
2:"2"
3:"Draft"
4:"Approved"
5:"Shipped"
```

... это происходит потому, что Typescript в данном случае создает для каждого ключа объекта **две пары** - `value-to-key` и `key-to-value`!

## Проверка типа

Посмотрим, как работает проверка типа в Typescript применительно к enum.

Такой вариант передачи параметра в функцию - выполнится без ошибок:

```typescript
logStatusString(PackStatusString.Approved);
```

... но если попробовать передать как аргумент - строку (даже если это - значение enum'а):

```typescript
logStatusString('Approved');
```

... получим ошибку типизации - `BugFinder: Argument of type '"Approved"' is not assignable to parameter of type 'PackStatusString'`; но вот если тоже самое - сделать для числового enum `PackStatusNumber`:

```typescript
logStatusNumber(1);
```

... то ошибки не получим! Странное поведение.

## Один enum вместо другого

Typescript не позволяет использовать один enum вместо другого, даже если ключи у них - одинаковые. Допустим, создадим два таких enum - один `PackStatusString` уже был у нас, а второй `PackStatusStringSingle` будет таким:

```typescript
enum PackStatusString {
  Draft = "Draft",
  Approved = "Approved",
  Shipped = "Shipped",
}

enum PackStatusStringSingle {
  Draft = "Draft"
}
```

... и попробуем подставить `PackStatusStringSingle` вместо `PackStatusString`:

```typescript
logStatusString(PackStatusStringSingle.Draft);
```

... Typescript выдаст ошибку типизации - `BugFinder: Argument of type 'PackStatusStringSingle' is not assignable to parameter of type 'PackStatusString'`.

И даже если - сделать второй enum `PackStatusStringLink` - как ссылку на первый enum `PackStatusString`:

```typescript
enum PackStatusString {
  Draft = "Draft",
  Approved = "Approved",
  Shipped = "Shipped",
}

enum PackStatusStringLink {
  Draft = PackStatusString.Draft
}
```

... то все равно:

```typescript
logStatusString(PackStatusStringLink.Draft);
```

... получим ошибку типизации - `BugFinder: Argument of type 'PackStatusStringLink' is not assignable to parameter of type 'PackStatusString'`. Логичное поведение.

## Итог

Как подведение итога всему вышеописанному, автор статьи делает следующий вывод:

> Если я увидел перечисление в кодовой базе, я вряд ли от него избавлюсь. Но я бы не стал добавлять перечисление в новую кодовую базу. Если вы отчаянно нуждаетесь в перечислениях, я настоятельно рекомендую использовать только строковые перечисления. Они явны, ведут себя как перечисления и больше похожи на транспилированный код.

***

[1]: https://www.totaltypescript.com/why-i-dont-like-typescript-enums "Why I Don't Like Enums Matt Pocock"