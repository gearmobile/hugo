+++ 
draft = false
date = 2022-03-31
title = "TypeORM - что такое Repository"
description = "Repository - менеджер для управления таблицами в базе данных"
slug = ""
authors = ["Moe Green"]
tags = ["typeorm", "repository"]
categories = ["Nest"]
externalLink = ""
series = []
+++

Repository в [TypeORM][2] - это отдельный специальный *класс*; другими словами можно сказать - это *менеджер для управления таблицами* в базе данных. Для каждой конктретной таблицы в БД (MySQL, Postgres и тп) - создается свой собственный repository (класс) - для управления записями в этой конкретной таблице.

Каждый репозиторий имеет *набор методов* для чтения, создания, обновления, удаления и тп - записей в таблице. В нем ещё могут быть дата-мапперы, которые маппят данные, пришедшие из БД в тот вид, который требует сервис на бекенде.

Здесь представлены все [свойства и методы][1] класса Repository.

***
[1]: http://typeorm.delightful.studio/classes/_repository_repository_.repository.html "Class Repository<Entity>"
[2]: https://typeorm.io/ "TypeORM"