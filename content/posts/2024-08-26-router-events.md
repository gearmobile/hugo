+++
title = "События маршрутизации Angular"
date = "2024-08-26"
tags = ["angular", "router"]
categories = ["Development"]
+++

Ниже перечисленны и приведены примеры из реальной практики для некоторых событий маршрутизации в Angular.

## NavigationStart - событие начала навигации:

```typescript
this.more.isSelected$ = router.events.pipe(
    filter(e => e instanceof NavigationStart),
    debounceTime(300),
    startWith(''),
    tap(() => this._promoted = false),
    map(() => this._items$.getValue().slice(4).some(item => router.url.startsWith(`/${item.route.path}`)))
);
```

## NavigationEnd - событие окончания навигации:

```typescript
this.router.events
    .pipe(
        filter(event => event instanceof NavigationEnd),
        debounceTime(500),
        takeUntil(this.destroy$)
    )
    .subscribe((event: NavigationEnd) => {
        this.store.dispatch(new NotificationsActions.RefreshUrls(event.url));
        this.store.dispatch(NotificationsActions.GetUnreadCount);
    });
```

## NavigationCancel - событие при отклонении навигации:

```typescript
public readonly isRouteChanging$ = this._router.events.pipe(
    filter(e => e instanceof NavigationStart || e instanceof NavigationEnd || e instanceof NavigationCancel ),
    map(e => e instanceof NavigationStart),
    shareReplay({ bufferSize: 1, refCount: true }),
    takeUntil(this._destroy$)
);
```

... возникает, когда Guard возвращает `false`.

## NavigationError - событие ошибки навигации

Возникновение непредвиденной ошибки в процессе осуществления навигации.

## Общее

Перечисленные выше события могут быть обработаны в любом компоненте или сервисе приложения. Чтобы определить для них обработчики, необходимо подписаться на свойство `events` сервиса Router:

```typescript
this.router.events.pipe()
```

... в котором находится поток с событиями маршрутизации.

Все события описываются отдельными классами, которые хранятся в пакете `@angular/router`. Например, для события `NavigationStart` есть класс [NavigationStart](https://angular.dev/api/router/NavigationStart), в котором описаны доступные свойства и методы; например, свойство `url` хранит активный url-маршрута.

## Ссылки

- [Angular Router Events Lifecycle](https://dev.to/elviskim18/angular-router-events-lifecycle-4l7g)