+++
title = "TS - Readonly, Required, Partial"
date = "2024-08-06"
tags = ["typescript", "utility types", "readonly", "required", "partial"]
categories = ["Development"]
meta = true
+++

### Утилита Readonly

Просто делает все поля объекта как `readonly`:

```typescript
interface Bulochka {
    price: number;
    weight: number;
    lenght: number;
    brand: string;
}

const bulochka: Readonly<Bulochka> = {
    price: 12,
    weight: 200,
    lenght: 100,
    brand: 'white'
}
```

... в данном случае все поля объекта `bulochka` будут доступны только для чтения:

```typescript
const bulochka = {
    readonly price: number;
    readonly weight: number;
    readonly lenght: number;
    readonly brand: string;
}
```

#### Примеры из реальной практики

```typescript
private filterGroups(groups: Readonly<IFaqGroup[]>, filter: Readonly<IFaqFilter>): IFaqGroup[] {
    let result: IFaqGroup[] = [...groups];

    if (filter.group) {
        result = result.filter(group => group.id === filter.group);
    }

    if (filter.search) {
        result = result.filter(group => group.name.includes(filter.search)
            || new FaqQuestionsFilterPipe().transform(group.questions, filter).length > 0);
    }

    return result;
}
```

### Утилита Required

... просто делает все поля объекта **обязательными**; такой пример:

```typescript
interface Bulochka {
    price?: number;
    weight?: number;
    lenght?: number;
    brand?: string;
}

const readonlyBulochka: Required<Bulochka> = {
    price: 12,
    weight: 200,
    lenght: 100,
};
```

... будет с ошибкой, так как не задано обязательное поле `brand`:

```typescript
Property 'brand' is missing in type '{ price: number; weight: number; lenght: number; }' but required in type 'Required<Bulochka>'.
```

###  Утилита Partial

Противоположность утилите Required - делает поля объекта **не обязательными**. Такой пример ошибкой не будет:

```typescript
interface Bulochka {
    price: number;
    weight: number;
    lenght: number;
    brand: string;
}

const readonlyBulochka: Partial<Bulochka> = {
    price: 12,
    weight: 200,
    lenght: 100,
};
```

#### Примеры из реальной практики

```typescript
@Pipe({
    name: 'addAllSelectionViewAsAccount'
})
export class AddAllSelectionViewAsAccountPipe implements PipeTransform {
    transform(accounts: Account[]): Partial<Account>[] {
        return [{ id: ALL_SELECTOR.id, displayedName: ALL_SELECTOR.name }, ...accounts];
    }
}
```

```typescript
private checkBaseConfigParameters(_cfg: Partial<IConfig>): void {
    if (!_cfg.basePath) {
        throw new BaseConfigParameterError('Адрес сервера');
    }

    if (!_cfg.customer) {
        throw new BaseConfigParameterError('Контрагент');
    }
}
```

```typescript
private static readonly _templateFileName: Partial<Record<IndicationType, string>> = {
    [IndicationType.interval]: 'Интервальные показания',
    [IndicationType.integral]: 'Интегральные показания',
    [IndicationType.meteringPointConsumption]: 'Расход ТУ'
};
```

***