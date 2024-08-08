+++
title = "TS - Pick и Omit"
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

#### Реальный пример из практики:

```typescript
private readonly _withoutAccount: Pick<Account, 'displayedName'> & { id: string } = {
    id: 'without',
    displayedName: 'Здесь нет моего договора'
};
```

... здесь Pick обращается к свойству `displayedName` класса `Account`; и в качестве свойства класса выступает геттер `displayedName`:

```typescript
export class Account implements IAccountExtended {
    fullName: string;
    id: number;
    alias: string;
    addressFull: accounts.IAccountAddress = { identifier: false, value: '' };
    ...

    public get displayedName(): string {
        return `${this.alias || this.fullName || this.service.provider.name} (${this.tenancy.register})`;
    }

    constructor(source: accounts.IAccount, provider: Provider) {
        Object.assign(this, source);
        this.service.provider = provider;
    }

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

#### Реальные примеры из практики

Здесь видим интерфейс `IUserToAccount`, который расширяется другим интерфейсом `IUser`, при этом у интерфейса `IUser` выборочно забираются все поля, *кроме* поля `fields`. При этом обратим внимание на поле `role` - в нем также используется утилита Omit, при помощи которой поле `role` получает все поля из интерфейса `IRole`, *кроме* поля `operations`:

```typescript
interface IUserToAccount extends Omit<users.IUser, 'fields'> {
    role: Omit<IRole, 'operations'>;
}
```

Еще один наглядный пример:

```typescript
type IApiModal = Omit<IModal, 'pictures' | 'attachments'> & {
    pictures?: IApiPictureCollectionItem[];
    attachments: ApiAttachment[];
};
```

... здесь при помощи утилиты Omit из интерфейса `IModal` забираются все свойства, кроме `pictures` и `attachments`.

Далее видим еще один интересный инструмент TS - пересечение [Intersection][1], обозначаемое символом `&`. При помощи [Interseption][2] в тип `IApiModal` добавляются еще два поля - `pictures` и `attachments`, взятые из литерала объекта.

Если сказать точнее - поля `pictures` и `attachments` - попадают в тип `IApiModal` с новыми типами, нежели какие у них были в интерфейсе `IModal`.

Можно сделать красивее:

```typescript
type Modify<T, R> = Omit<T, keyof R> & R;

type ApiModal = Modify<IModal, {
    pictures?: IApiPictureCollectionItem[];
    attachments: ApiAttachment[];
};
```

### Ссылки

* [Искусство типизации: TypeScript Utility Types](https://habr.com/ru/articles/711686/)
* [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

***

[1]: https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#intersection-types "Intersection Types"
[2]: https://scriptdev.ru/guide/016/#intersection-type "Тип Пересечение (Intersection Type)"