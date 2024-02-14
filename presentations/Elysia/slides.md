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
	console.log("after handler");
});
```
