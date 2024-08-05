+++
title = "TS - Utility Types"
date = "2024-08-05"
tags = ["typescript", "utility types"]
categories = ["Development"]
meta = true
+++

### Утилита Pick

```typescript
interface Bulochka {
    price: number;
    weight: number;
    lenght: number;
    brand: string;
}

type PickedBulochka = Pick<Bulochka, 'price' | 'brand'>;

const bulochka: PickedBulochka = {
    price: 12,
    brand: 'white'
}
```

Утилита Pick включает из интерфейса `Bulochka` пары ключ-значение по указанным ключам, в нашем случае - `price` и `brand`. В итоге тип `PickedBulochka` будет содержать только такой набор:

```typescript
type PickedBulochka = {
    price: number;
    brand: string;
}
```

### Утилита Omit

```typescript
interface Bulochka {
    price: number;
    weight: number;
    lenght: number;
    brand: string;
}

type PickedBulochka = Omit<Bulochka, 'price' | 'brand'>;

const bulochka: OmitedBulochka = {
    weight: 12,
    lenght: 20
}
```

Omit - прямая противоположность утилите Pick. Она исключает из интерфейса указанные ключи. В нашем случае в типе `OmitedBulochka` будут все ключи из интерфейса `Bulochka`, за исключением ключей `price` и `brand`:

```typescript
type OmitedBulochka {
    weight: number;
    lenght: number;
}
```

### Ссылки

* [Искусство типизации: TypeScript Utility Types](https://habr.com/ru/articles/711686/)
* [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)