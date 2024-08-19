+++
title = "TS - Record"
date = "2024-08-09"
tags = ["typescript", "utility types"]
categories = ["Development"]
+++

Record (запись) помогает создавать [словари][1], также называемые парами ключ/значение, с **фиксированным типом** для ключей и **фиксированным типом** для значений.
Другими словами, тип `Record` позволяет определить тип словаря - то есть имена и типы его ключей.

Сила типа `Record` в том, что с его помощью можно моделировать словари с фиксированным числом ключей.

Анатомия Record - `Record<Keys, Type>` - строит **объектный** тип, ключами свойств которого являются `Keys`, а значениями свойств — `Type`.

## Record для создания модели университетских курсов

Еще раз - `Record` позволяет создавать объекты с динамической структурой, когда мы добавляем в них поля **во время исполнения программы**; с аннотацией `type` можно строить динамические `key/value` (*вообще можно любую структуру*): у интерфейсов основа - это всегда hard-coded объект.

Ниже приведе пример создания словаря при помощи Record:

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
Также Typescript проверяет, чтобы в создаваемом словаре не было ключей `Keys`, которые **не определены** заранее. То есть, если сделать так:

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

## Допустимые типы для ключей

Типами для ключей `Keys` в Record могут быть только - `number`, `string` и `symbol`. В приведенном ниже примере использование для ключа `Key` типа `boolean` или `object` приведет к ошибке:

```typescript
type numberedUser = Record<number, TUser>;
type stringUser = Record<string, TUser>;
type symbolUser = Record<symbol, TUser>;

type booleanUser = Record<boolean, TUser>; // Type 'boolean' does not satisfy the constraint 'string | number | symbol'
type booleanUser = Record<object, TUser>; // Type 'object' does not satisfy the constraint 'string | number | symbol'
```

## Допустимые типы для значений

Для значений `Type` допустим любой тип данных - наиболее частый случай, это объекты или функции. Это означает, что значениями могут быть компоненты.

## Примеры из практики

Пример ниже - `Record` имеет в качестве ключа `string`, а в качестве значения ключа - целый набор типов:

```typescript
private get params(): Record<string, string | number | boolean | ReadonlyArray<string | number | boolean>> {
    return {
        account: this.account,
        department: this.department,
        documentNumber: this.document.number,
        start: this.period.startDate,
        end: this.period.endDate,
        search: this.search,
        status: this.status
    };
}
```

... обратим внимание на запись `ReadonlyArray<string | number | boolean>` - в данном случае расширенный тип `ReadonlyArray<T>` предназначен для создания [неизменяемого массива][2]; более того, у такого массива отсутствуют методы для добавления, удаления или изменения элементов такого массива, в отличие от обычного массива `Array<T>`.

Еще один пример динамического создания объекта:

```typescript
public readonly refs: {
    meterStates: typeof CspMesMeterState;
    meterStateNames: Record<CspMesMeterState, string>;
} = {
    meterStates: CspMesMeterState,
    meterStateNames: {
        [CspMesMeterState.OK]: 'Работает, нет несоответствия',
        [CspMesMeterState.DEFECTIVE_METER]: 'Неисправный ЭС',
        [CspMesMeterState.ABSENT_METER]: 'Отсутствует ЭС',
        [CspMesMeterState.THEFT_METER]: 'Хищение ЭС',
        [CspMesMeterState.DEFECTIVE_SYSTEM]: 'Система неисправна',
        [CspMesMeterState.VIOLATION_ACT]: 'Нарушение учета с Актом',
        [CspMesMeterState.EXPIRED_INTERVAL]: 'Истек срок МПИ',
        [CspMesMeterState.DEFECTIVE_METER_WITH_EXPIRED_INTERVAL]: 'Нерабочий ПУ с истекшим сроком МПИ'
    }
};
```

... здесь `CspMesMeterState` является перечисляемым типом enum. Думаю, данный кейс является ярким примером создания словаря - четко видно структуру.

Достаточно интересный пример, когда используются сразу два типа - `Partial` и `Record`:

```typescript
public readonly contactTypes: {
    [key in AccountUnit]: Partial<Record<ContactType, string>>;
} = {
    [AccountUnit.supervisor]: {
        [ContactType.ADDRESS]: 'Адрес отделения',
        [ContactType.EMAIL]: 'Электронная почта отделения',
        [ContactType.PHONE]: 'Телефон отделения'
    },
    [AccountUnit.curator]: {
        [ContactType.ADDRESS]: 'Адрес куратора',
        [ContactType.EMAIL]: 'Электронная почта куратора',
        [ContactType.PHONE]: 'Телефон куратора'
    }
};
```

... здесь `Partial<Record<ContactType, string>>` - здесь динамически создается объект `Record<ContactType, string>`, у которого все поля делаются необязательными при помощи типа `Partial`.

В заключение еще пример - ничего особенного:

```typescript
private static readonly refs: Record<BankingProducts, ProductCard> = {
    [BankingProducts.virtualCard]: {
        title: 'Виртуальная карта',
        button: {
            label: 'Оформить',
            path: ['/credit-calc']
        }
    },
    [BankingProducts.mortgage]: {
        title: 'Заявка на ипотеку',
        button: {
            label: 'Заполнить анкету',
            path: ['/', APPEALS_ROUTE.root, APPEALS_ROUTE.predefined.mortgage]
        }
    },
    [BankingProducts.creditCalc]: {
        title: 'Расчёт кредита',
        button: {
            label: 'Рассчитать',
            path: [`${PARTNERS_ROUTE_PARAMS.products}/${BANKING_PRODUCTS_ROUTE_PARAMS.creditCalc}`]
        }
    },
    [BankingProducts.pacl]: {
        title: 'Предодобренный кредит',
        button: {
            label: 'Подробнее',
            path: ['/', VTB_ROUTE.root, VTB_ROUTE.pacls]
        }
    }
};
```

... `BankingProducts` - это enum; `ProductCard` - это интерфейс.

***

[1]: https://education.yandex.ru/handbook/algorithms/article/slovar "Словарь"
[2]: https://scriptdev.ru/guide/046/#readonlyarray "ReadonlyArray (неизменяемый массив)"