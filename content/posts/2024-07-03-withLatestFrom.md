+++
title: "Оператор withLatestFrom"
date = "2024-08-01"
categories = ["Development"]
tags: ["rxjs", "withLatestFrom"]
meta = true
+++

На вход оператора передается список потоков, он на них всех подписывается получает по последнему значению; потом берет значение из родительского потока и соединяет это все в один поток.

<!--more-->

### Практическое применение

Оператор, чаще всего нужен, чтобы при эмите значения в потоке, получить какие-то данные из другого потока.

Пример схематичный:

```typescript
hello$.pipe(
  withLatestFrom(this.toggleService.isEnabled('some-feature'))
)
```

... то есть - в данном случае, если значение пришло из родительского потока _hello$_ - оператор _withLatestFrom_ заберет значение из метода сервиса _isEnabled_ и вернет - поток из этих двух значений.

Пример реальный, из эффекта NgRx:

```typescript
export const saveCounter = createEffect(
  () =>
    inject(Actions).pipe(
      ofType(CounterActions.incrementCounter, CounterActions.decrementCounter),
      withLatestFrom(inject(Store).select(countSelect)),
      tap(([payload, count]) => {
        localStorage.setItem(ECounterStorage.CounterKey, count.toString());
      })
    ),
  { dispatch: false, functional: true }
);
```

... здесь запись `ofType(CounterActions.incrementCounter, CounterActions.decrementCounter)`, трактуется как ИЛИ; то есть, оператор `ofType` будет фильтровать поток `actions$` на экшен `incrementCounter` или экшен `decrementCounter`.

Оператор _withLatestFrom_ здесь также - подписан и слушает родительский поток _actions$_. Если фильтр _ofType_ сработает - из потока _actions$_ прийдет евент в оператор _withLatestFrom_, поэтому в свою очередь оператор _withLatestFrom_ заберет значение из потока _countSelect_, объединит оба значения и вернет новый поток - в евентом, содержащим оба эти значения - _([payload, count])_.

### Полезные ссылки

- [withLatestFrom](https://thinkrx.io/rxjs/withLatestFrom/)
- [withLatestFrom](https://www.learnrxjs.io/learn-rxjs/operators/combination/withlatestfrom)
- [Описание оператора с примером на Stackblitz](https://www.angulartraining.com/daily-newsletter/rxjs-withlatestfrom-operator/)