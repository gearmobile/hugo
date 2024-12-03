+++
title = "Вложенные формы Angular"
date = "2024-12-03"
tags = ["angular", "form"]
categories = ["Development"]
+++

В Angular есть несколько способов достать контрол из вложенных форм. Допустим, есть пример вложенной (nested) формы:

```typescript
  public contactForm = this.fb.nonNullable.group({
    id: '',
    isPersonal: false,
    firstName: ['', [Validators.required, Validators.minLength(3)]],
    lastName: '',
    dateOfBirth: <Date | null>null,
    favoritesRanking: <number | null>null,
    phone: this.fb.nonNullable.group({
      phoneNumber: '',
      phoneType: '',
    }),
    address: this.fb.nonNullable.group({
      streetAddress: ['', Validators.required],
      city: ['', Validators.required],
      state: ['', Validators.required],
      postalCode: ['', Validators.required],
      addressType: ['', Validators.required],
    }),
    notes: ['', restrictedWordsValidator(['bad', 'ugly', 'stupid'])],
  });
```

Получить контрол (например, `firstName`) по имени при помощи метода `get` можно так:

```typescript
this.contactForm.get('firstName');
```

Здесь все просто. Но если нужен контрол из вложенных форм (например, `phoneNumber`), то нужно использовать метод `get` с несколько другим синтаксисом.

## Первый способ

Наверное, такой способ можно назвать - точечная нотация:

```typescript
this.contactForm.get('phone.phoneNumber')?.value
```

## Второй способ

Очень похожий способ, но используется массив с ключами-именами полей формы:

```typescript
this.contactForm.get(['phone', 'phoneNumber'])?.value
```

## Третий способ

Используется два метода `get`:

```typescript
this.contactForm.get('phone')?.get('phoneNumber')?.value
```

## Итог

Все три способа дают одинаковый результат. Но второй и третьй способ - выглядят более удобными и интуитивно понятными.