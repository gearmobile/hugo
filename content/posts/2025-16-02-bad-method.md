+++
title = "Давайте избавляться от "нехорошего" метода"
date = "2025-16-02"
tags = ["angular", "form"]
categories = ["Development"]
+++

До выхода Angular 14 мы частенько пользовались методом `get`, который возвращает `FormControl`:

```ts
this.editForm.get('firstName').value; // => так не безопасно!
```

... использование больше небезопасно. В данном примере TypeScript **не выдаст ошибку**, даже если поля `firstName` в модели формы не существует!

Поэтому правильнее заменить метод `get` на обращение через `controls`:

```ts
this.editForm.controls.firstName.value;  // => а так безопасно
```