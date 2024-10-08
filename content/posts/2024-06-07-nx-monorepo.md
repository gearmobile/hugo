+++
title = "Nx - monorepo NestJS + Angular"
date = "2024-06-07"
description = "Научимся, как устанавливать и подключать в Nest-проекте такой ORM-инструмент, как Prisma"
tags = ["nx", "nestjs", "angular"]
categories = ["Development"]
+++

На официальном сайте Nx не смог найти готовых примеров конфигурации Nx для развертывания fullstack-бойлерплейта для фронтенд и бэкэнд - Angular + NestJS. После некоторых поисков нашел такой пример на StackOverflow - [NX Monorepo (NestJS/Angular)](https://stackoverflow.com/questions/77347982/nx-monorepo-nestjs-angular).

Ниже привел *более подробный* вариант описания этого способа.

## Пошаговая инструкция

Устанавливаем Nx глобально в системе - для более удобной последующей работы [Installing Nx Globally](https://nx.dev/getting-started/installation#installing-nx-globally):

```bash
npm add --global nx@latest
```

Переходим к процессу развертывания - инициализируем новый workspace командой [Installation](https://nx.dev/getting-started/installation):

```bash
npx create-nx-workspace

Need to install the following packages:
create-nx-workspace@19.2.3
Ok to proceed? (y)
```

... задаем имя для нового проекта:

```bash
Where would you like to create your workspace? ‣ awesome-project 
```

... на вопрос о выборе предпочтительного стека отвечаем - **None** - сами наполним workspace нужными стеками чуть позднее:

```bash
? Which stack do you want to use? …

None:          Configures a TypeScript/JavaScript project with minimal structure.
React:         Configures a React application with your framework of choice.
Vue:           Configures a Vue application with your framework of choice.
Angular:       Configures a Angular application with modern tooling.
Node:          Configures a Node API application with your framework of choice.
```

... на вопрос о выборе типа репозитория - выбираем - **Integrated Monorepo**:

```bash
? Package-based monorepo, integrated monorepo, or standalone project? …

Package-based Monorepo:     Nx makes it fast, but lets you run things your way.
Integrated Monorepo:        Nx creates a monorepo that contains multiple projects.
Standalone:                 Nx creates a single project and makes it fast.
```

... на предложение воспользоваться Nx Cloud отвечаем, что не хотим - **Skip for now**:

```bash
? Do you want Nx Cloud to make your CI fast? …

(it's free and can be disabled any time)
Yes, enable Nx Cloud
Yes, configure Nx Cloud for GitHub Actions
Yes, configure Nx Cloud for Circle CI
Skip for now
```

... процесс инициализации занимает некоторое время, после чего получаем готовый пустой workspace:

```bash
 NX   Creating your v19.2.3 workspace.

✔ Installing dependencies with npm
✔ Successfully created the workspace: awesome-project.
```

... переходим во вновь созданный проект:

```bash
cd awesome-project
```

... добавляем необходимые Nx-зависимости:

```bash
npm i -D @nx/angular @nx/nest @nx/js
```

... затем генерируем в workspace фронтенд на angular - [@nx/angular](https://nx.dev/nx-api/angular):

```bash
nx g @nx/angular:app frontend-part
```

... затем генерируем бекенд на nestjs - [@nx/nest](https://nx.dev/nx-api/nest):


```bash
nx g @nx/nest:app backend-part
```

... генерируем shared library [@nx/js:library](https://nx.dev/nx-api/js/generators/library):

```bash
nx g @nx/js:lib shared-lib
```

С этого момента у нас есть базовая настройка. Мы можете импортировать общую библиотеку следующим образом::

```typescript
import { sharedLib } from "@my-project/shared-lib";
```

Если в дальнейшем в проекте планируется использовать Prisma и PostgreSQL, то по дальнейшим шагам можно ознакомиться здесь - [Prisma - установка в Nest-проекте](https://musocoder.netlify.app/posts/2022-02-23-prisma-connect/).

## Дополнительные источники

В процессе общения по текущей теме в Discord (Nx-форум) c пользователем *@rujorgensen* последний любезно предложил свой вариант готового решения - [nx-workspace](https://github.com/rujorgensen/nx-workspace). Можно сделать fork репозитория - и попробовать использовать его, как вариант.

Еще один вариант создания fullstack-бойлерплейта для фронтенд и бэкэнд - [Создание пустого проекта с помощью NestJS-mod](https://habr.com/ru/articles/834888/). Идея проекта - не только пустые приложения сделать, но и наполнить бойлерплейтом, стандартным который обычно используется.

## Проверка первоначального решения

Как проверка работоспособности первоначального решения - привожу ссылку на учебный проект [Task Manager](https://github.com/My-Angular-Projects/task-manager).

На момент написания статьи - проект находится в *стадии разработки*.

## UPD

Проект Nx находится в активной разработке и половина описанных шагов (на момент написания данного поста) - уже не актуальны на число 15.08.2024.