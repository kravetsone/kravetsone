---
theme: elysia
highlighter: shiki
layout: cover
---

<CoverContent/>

---
layout: default
---
<SlideLogo framework="ExpressJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

- Проверен временем
- Большое количество материала
- Считается стандартом (к сожалению)
- Популярен (множество готовых решений)

<p class="text-red">Минусы</p>

- Медленный
- Middleware
- Плохая типизация
- Из коробки почти ничего не имеет
- Написан в эру мамонтов
- Костыльный
- Разработчики забили

---
layout: default
---
<SlideLogo framework="ExpressJS" title="Медленный"/>

// TODO: Сделать свой чарт

<img class="mt-7" src="/benchmark.png"/>

---
layout: default
---
<SlideLogo framework="ExpressJS" title="Медленный"/>

// TODO: Описать медленный регексовый поиск экспресса

---
layout: default
---
<SlideLogo framework="ExpressJS" title="Middleware"/>

<div class="mt-7"/>

```ts twoslash
declare module "express-serve-static-core" {
    interface Request {
        user: { name: "MoscowJS" }
    }
}
// ---cut---
import Express from "express";

const app = Express();

app.use((req, res, next) => {
	if (!req.headers.authorization) return res.send("No access");
    
	req.user = { name: "MoscowJS" };
	console.log("before handler");

	next();
});

app.get("/", (req, res) => {
	return res.send(`Hello, ${req.user.name}!`);
});

app.use((req, res) => {
	console.log("after handler never executes...");
});
```

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Написан в эру мамонтов + костыльный"/>

<div class="mt-7"/>

```ts twoslash
/** Какая-то асинхронная операция по поиску юзера */
function findUser() {}
// ---cut---
import Express from "express";
import "express-async-errors";

const app = Express();

app.use(async (req, res) => {
	if (!req.headers.authorization) throw new Error("No access");

    await findUser();
});
// Да-да кривые типы экспресса с примером из доки - https://expressjs.com/en/guide/error-handling.html#error-handling
app.use((err, req, res, next) => {
	return res.status(400).json(err.message);
});
```

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Разработчики забили"/>

<div class="mt-7"/>

- 4.18.2 (latest) - год назад
- 4.18.1 - 2 года назад
- 4.18.0 - 2 года назад
- 4.17.3 - 2 года назад
- 5.0.0-beta.1 - 2 года назад
- 4.17.2 - 2 года назад
- 5.0.0-alpha.8 - 4 года назад
- 4.1.1 - 5 лет назад

// TODO: Придумать как обыграть покрасивее

---
layout: default
---

<SlideLogo framework="KoaJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

- Написан командой ExpressJS
- Значительно быстрее ExpressJS

<p class="text-red">Минусы</p>

- Middleware
- Плохая типизация
- Плохая работа с валидацией

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

- Современный
- Life-cycle hooks
- Производительный
- fast-json-stringify
- Построен на JSON Schema и AJV
- Отличный DX и swagger одной строчкой
- express-compatibility plugin

<p class="text-red">Минусы</p>

- Не идеал типизации

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Life-cycle hooks"/>

Request => Routing => Logger => onRequest Hook => preParsing Hook => Parsing => preValidation Hook => Validation => preHandler Hook => User Handler => Reply => preSerialization Hook => onSend Hook => Response => onResponse Hook