+++
title = "RxJs - отладка кода"
date = "2024-08-13"
tags = ["rxjs", "debug"]
categories = ["Development"]
+++

Наиболее простой пример debug'а RxJs-кода:

```typescript
service.getSomeObservable().subscribe(data => {
    console.log(data);
});
```

У такого способа есть существенные ограничения и главное - такой способ может работать только внутри `subscribe`. Кстати, такой пример использования `console.log` можно упростить:

```typescript
service.getSomeObservable().subscribe(console.log);
```

... это также будет работать!

Однако, если есть такой пример кода:

```typescript
this.continent$ = this.continentSelect.valueChanges.pipe(
    withLatestFrom(country$),
    map(([continent, countries]) => [
        continent,
        countries.filter((c) => c.continent == continent),
    ]),
    tap(([continent, filteredCountries]) => {
        this.countries = filteredCountries;
        this.countrySelect.setValue(filteredCountries[0].country);
    }),
    map(([continent]) => continent.substring(0, 3).toUpperCase())
);
```

... и стоит задача проверить его работу при помощи `console.log` - то здесь на помощь может прийти оператор `tap` - можно добавить его там, где необходимо проверить результат работы кода:

```typescript
this.continent$ = this.continentSelect.valueChanges.pipe(
    tap(console.log), // <- tap
    withLatestFrom(country$),
    tap(console.log), // <- tap
    map(([continent, countries]) => [
        continent,
        countries.filter((c) => c.continent == continent),
    ]),
    tap(console.log), // <- tap
    tap(([continent, filteredCountries]) => {
        this.countries = filteredCountries;
        this.countrySelect.setValue(filteredCountries[0].country);
    }),
    tap(console.log), // <- tap
    map(([continent]) => continent.substring(0, 3).toUpperCase())
);
```

Можно сделать универсальный вариант функции-дебаггера, который будет работать только в режиме разработки:

```typescript
export function debug() {
    return environment.production ? tap() : tap(console.log);
}
```

... и тогда можно использовать для отладки функцию `debug`:

```typescript
this.continent$ = this.continentSelect.valueChanges.pipe(
    debug(), // <- debug
    withLatestFrom(country$),
    debug(), // <- debug
    map(([continent, countries]) => [
        continent,
        countries.filter((c) => c.continent == continent),
    ]),
    debug(), // <- debug
    tap(([continent, filteredCountries]) => {
        this.countries = filteredCountries;
        this.countrySelect.setValue(filteredCountries[0].country);
    }),
    debug(), // <- debug
    map(([continent]) => continent.substring(0, 3).toUpperCase())
);
```

## Итог

Дебажить RxJs можно двумя способами - `console.log` и `tap`; выбор инструмента зависит от контекста задачи.

## Ссылка

* [How to debug RxJs code with Angular](https://blog.angulartraining.com/how-to-debug-rxjs-code-with-angular-0151fb2283dd)