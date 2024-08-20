+++
title = "TS - сопоставимые типы"
date = "2024-08-20"
tags = ["typescript", "utility types"]
categories = ["Development"]
+++

## Exclude

`Exclude<UnionType, ExcludedMembers>` - исключает определенные типы из объединенного типа:

```typescript
type A = 'a' | 'b' | 'c'
type B = Exclude<A, 'a' | 'b'>
// -> B = 'c'
```

```typescript
type Shape =
  { kind: "circle"; radius: number }
| { kind: "square"; x: number }
| { kind: "triangle"; x: number; y: number };

type T3 = Exclude<Shape, { kind: "circle" }>

type T3 =
  { kind: "square"; x: number }
| { kind: "triangle"; x: number; y: number }
```

## Extract

`Extract<Type, Union>` - извлекает из типа только те, которые присутствуют в обоих объединенных типах:

```typescript
type A = 'a' | 'b' | 'c';
type B = 'a' | 'b';

type C = Extract<A, B>;
// -> C = 'a' | 'b'
```

```typescript
type Shape =
| { kind: "circle"; radius: number }
| { kind: "square"; x: number }
| { kind: "triangle"; x: number; y: number };

type T2 = Extract<Shape, { kind: "circle" }>

type T2 = {
    kind: "circle";
    radius: number;
}
```

## NonNullable

`NonNullable<Type>` - извлекает все типы, исключая `null` и `undefined`:

```typescript
let value: string | null | undefined;
let nonNullableValue: NonNullable<typeof value>;
// теперь nonNullableValue это string
```