---
theme: elysia
mdc: true
highlighter: shiki
layout: cover
addons:
  - slidev-addon-qrcode
twoslash: build
export:
  timeout: 60000
  format: pdf
  dark: true
---

<div class="h-full flex flex-col justify-between">

<h1 class="w-2xl">Познаём Elysia и Bun — фреймворк, сделанный для людей</h1>

<div class="flex items-center justify-between">

<div class="flex flex-col gap-0 pt-25 text-xl">
    <span class="font-bold">Всеволод Деткин</span>
    <span>Бекенд разработчик, Элитриум</span>
</div>

<img class="-mr-25" width="500px" src="/fox.webp" />

</div>

</div>

<!--
Всем привет! Меня зовут Всеволод Деткин и я представляю компанию «Элитриум». В сегодняшнем докладе я бы хотел рассказать о мире фреймворков для создания серверных приложений и показать тот, который запал мне в душу. (Дополнить/переписать)
-->

---
layout: default
title: Обо мне
---
<div class="flex justify-between">
<div class="flex flex-col flex-items-center w-full p-5">
    <h1 class="text-xl flex-self-start">Обо мне</h1>
    <ul class="flex-self-start">
        <li>WIP</li>
        <!-- <li>Учусь мб</li>
        <li>Победитель хакатонов...</li>
        <li>Любитель новых технологий</li>
        <li>Любитель перекладывать жсоны</li>
        <li>Проекты</li>
        <li>Люблю ходить на митапы кста</li> -->
    </ul>
</div>

<img width="500" class="h-[450px]" src="/me.jpg" />
</div>

---
title: Express
---

<SlideLogo framework="ExpressJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

<v-clicks>

-   Проверен временем
-   Большое количество материала
-   Считается стандартом (к сожалению)
-   Популярен (множество готовых решений)

</v-clicks>

<p class="text-red">Минусы</p>

<v-clicks>

-   Медленный
-   Middleware
-   Плохая типизация
-   Из коробки почти ничего не имеет (микро-фреймворк)
-   Написан в эру мамонтов
-   Костыльный
-   Разработчики забили

</v-clicks>

<!--
Давайте перейдём к сути. Начнём с разбора уже существующих и устоявшихся решений таких как например ExpressJS. Начнём с его плюсов

[Click] Проверен временем. ExpressJS был разработан ещё в ноябре 2010 года и используется до сих пор что означает что проверку временем он однозначно прошёл
[Click] Большое количество материала. Думаю что каждый из вас кто занимался разработкой серверной части видел эту гору материала по экспрессу, но и замечал сколько материала уже морально устарело (например, решения на том же Stack Overflow). (Написать лучше)
[Click] Считается стандартом (к сожалению). Экспресс считается стандартом в мире Node.JS и от него сложно убежать. Каждый новичок начинает именно с него, огромное количество продакшн-решений до сих пор использует его. Он даже используется как основной адаптер для NestJS фреймворка, который многим полюбился.
[Click] Популярен (множество готовых решений). Я думаю, что для никого не секрет насколько же у Express'а большая экосистема, огромное количество туториалов и готовых разработок.

**А теперь перейдём к минусам:**

[Click] Медленный. ExpressJS действительно многое делает медленно. Например про работу его роутинга мы поговорим на следующих слайдах.
[Click] Middleware. Это паттерн, когда данные перетекают по цепочке из функции в функцию и мутируются. В современном мире на его место встали Хуки, о которых мы поговорим позднее.
[Click] Плохая типизация. ExpressJS поставляется с типами отдельно и они не идеально описаны.
[Click] Из коробки почти ничего не имеет (микро-фреймворк). ExpressJS по своей сути является микро-фреймворком, который предоставляет пользователю медленный роутинг и Middlewares. Какое-то время даже body-parser приходилось загружать отдельно.
[Click] Написан в эру мамонтов. Как бы это не звучало, но ExpressJS невероятно старый.
[Click] Костыльный. Последний мажор был выпущен до появления Promises так что теперь чтобы отловить ошибку брошенную в асинхронном Middleware нам приходится использовать хак
[Click] Разработчики забили. Разработчики более не занимаются активной разработкой/поддержкой Express'а.
-->

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Медленный"/>

<img class="mt-7" src="/benchmark.png"/>

<!--
Касательно его медлительности. Вот мы можем увидеть бенчмарк который показывает насколько express ныне не конкурентно-способен. **Забавный факт** - раздел в документации NestJS про использование Fastify адаптера вместо Express называется «**Performance (Fastify)**»
-->

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Медленный роутинг"/>

<div class="flex items-center justify-around h-full">

<div class="flex flex-col justify-center items-center">
/some/:value 

<formkit-arrowdown class="text-4xl" />
<div>
<logos-npm-icon /> path-to-regexp
</div>
<formkit-arrowdown class="text-4xl" />

/^\/some\/(?:([^\/]+?))\/?$/i
</div>

<QRCode type="svg" data="https://forbeslindesay.github.io/express-route-tester/"
            :dotsOptions="{ type: 'extra-rounded', color: 'purple' }" :width="200"
            :height="200" />

</div>

<!--
TODO: переделать слайд визуально.
В роутинге Express'а не применяется хитрых алгоритмов по типу RadixTree, которые можно встретить у конкурентов.
Express же использует библиотеку path-to-regexp которая превращает ваш текст в регулярное выражение
-->

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Middleware"/>



---

<SlideLogo framework="ExpressJS" title="Middleware"/>

<div class="flex">

```ts twoslash
declare module "express-serve-static-core" {
    interface Request {
        user: { name: "Yandex" };
    }
}
// ---cut---
import Express from "express";

const app = Express();

app.use((req, res, next) => {
    if (!req.header("authorization")) return res.send("No access");

    req.user = { name: "Yandex" };
    console.log("before handler");

    next();
});
app.use((req, res, next) => {
    // Непредсказуемо мутируем ещё что-то
    next();
});
app.get("/", (req, res) => {
    res.send(`Hello, ${req.user.name}!`);
});
```
<div class="flex flex-col items-center justify-center ml-35">
Middleware (req, res, next)
<formkit-arrowdown class="text-4xl" />
Middleware (req, res, next)
<formkit-arrowdown class="text-4xl" />
Controller (req, res)
</div>

</div>

<!--
TODO: flow-chart req => req => req => send

Вот так выглядит пример работы Middleware. Функции вида req, res, next. И с помощью next мы можем пойти в следующий Middleware или с помощью return остановить его здесь.
-->

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Написан в эру мамонтов + костыльный"/>

```ts {1,2|all} twoslash
/** Какая-то асинхронная операция по поиску юзера */
function findUser() {}
// ---cut---
import Express from "express";
import "express-async-errors";

const app = Express();

app.use(async (req, res) => {
    if (!req.header("authorization")) throw new Error("No access");

    await findUser();
});
// Да-да кривые типы экспресса с примером из доки - https://expressjs.com/en/guide/error-handling.html#error-handling
app.use((err, req, res, next) => {
    return res.status(400).json(err.message);
});
```

<!--
Касательно его костыльности. Мы не можем просто взять и выбросить ошибку в асинхронной функции без использования хака / своей обёрткой над middleware.

[Click] А ещё параметры error-handler Middleware становятся **any**. Хотя это есть в документации...
-->

---
layout: full
---

<img src="/swagger-autogen.png"/>


---
layout: default
---

<SlideLogo framework="ExpressJS" title="Разработчики забили"/>

-   4.18.3 (latest) - пару недель назад (спойлер - исправили 1 баг за год)
-   4.18.2 - год назад
-   4.18.1 - 2 года назад
-   4.18.0 - 2 года назад
-   4.17.3 - 2 года назад
-   5.0.0-beta.1 - 2 года назад
-   4.17.2 - 2 года назад
-   5.0.0-alpha.8 - 4 года назад
-   4.17.1 - 5 лет назад

...
-   4.0.0 - 04.09.2014
<!--
Express перестал активно обновляться. Как например первая бета 5.0.0 вышла 2 года назад. И с того момента не получила никаких коммитов в ветке на гитхабе. Хотя документация во всю кричит «Документация версии 5.0.0 уже доступна».
-->

---
layout: full
---

<img class="w-full" src="/hyper-express.png" width="500"/>

---
layout: default
title: Koa
---

<SlideLogo framework="KoaJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

<v-clicks>

-   Написан командой ExpressJS
-   Значительно быстрее ExpressJS
-   Закрывает многие проблемы ExpressJS

</v-clicks>

<p class="text-red">Минусы</p>

<v-clicks>

-   Middleware
-   Не идеал типизации
-   Плохая интеграция с OpenAPI
-   Не особо популярен

</v-clicks>

<!--
Теперь давайте перейдём к следующему фреймворку. Им будет **Koa**. 

**Начнём с плюсов:**

[Click] Написан командой ExpressJS. Koa был написан командой, которая занималась и ExpressJS. Так что этот фреймворк справедливо можно назвать перерождением Express'а
[Click] Значительно быстрее ExpressJS. В Koa нет таких значительных проблем как в Express'е.

**А теперь к минусам:**

[Click] Middleware. Так как это буквально перерождение Express паттерн Middleware никуда не исчез.
[Click] Плохая интеграция с OpenAPI. // TODO:
[Click] Не особо популярен. Пропагандировался как фреймворк нового поколения исправляя callback hell генераторами // TODO: дописать додумать
-->

---

<SlideLogo framework="KoaJS" title="Генераторы"/>

```ts
var koa = require("koa");
var app = koa();

app.use(function *(next) {
    var start = new Date;
    yield next;
    var ms = new Date - start;
    console.log("%s %s - %s", this.method, this.url, ms);
});

app.use(function *(){
    this.body = "Hello world!";
});

app.listen(3000);
```

<!-- 
KoaJS назывался «фреймворком нового поколения» и решал проблему callback hell ещё до появления сахара в виде async/await
но после появления их появления люди так и остались на express
-->

---
layout: default
title: Fastify
---

<SlideLogo framework="FastifyJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

<v-clicks>

-   Современный
-   Life-cycle hooks
-   Производительный
-   fast-json-stringify
-   Построен на JSON Schema и AJV
-   Отличный DX и swagger одной строчкой
-   express-compatibility plugin
-   Удобная обработка ошибок

</v-clicks>
<p class="text-red">Минусы</p>
<v-clicks>

-   Не идеал типизации

</v-clicks>

<!--
Давайте поговорим о FastifyJS. 

**Начнём с плюсов:**

[Click] Современный. Первая стабильная версия Fastify вышла 6 лет назад и новые версии всё ещё выходят. Вот-вот и выйдет v5
[Click] Значительно быстрее ExpressJS. В Koa нет таких значительных проблем как в Express'е.

**А теперь к минусам:**

[Click] Middleware. Так как это буквально перерождение Express паттерн Middleware никуда не исчез.
[Click] Плохая интеграция с OpenAPI. // TODO:
[Click] Не особо популярен. Многие не видят в нём преимуществ перед Express // TODO: дописать додумать
-->


---
layout: default
---

<SlideLogo framework="FastifyJS" title="Life-cycle hooks"/>

<!-- Request => Routing => Logger => onRequest Hook => preParsing Hook => Parsing => preValidation Hook => Validation => preHandler Hook => User Handler => Reply => preSerialization Hook => onSend Hook => Response => onResponse Hook -->

<div class="flex">

```ts
fastify.post("/", (request, reply) => {
    // some logic
});

fastify.addHook('preParsing', async (request, reply, payload) => {
  await asyncMethod();

  return newPayload;
});
fastify.addHook('preHandler', async (request, reply) => {
  await asyncMethod();
});
fastify.addHook('onSend', async (request, reply, payload) => {
  const newPayload = payload.replace('some-text', 'some-new-text');

  return newPayload;
});
```

<div class="flex flex-col ml-35">

- onRequest
- preParsing
- preValidation
- preHandler
- preSerialization
- onError
- onSend
- onResponse
- onTimeout
- onRequestAbort
</div>
</div>

<!-- // TODO: flow chart -->

---
layout: default
---
<SlideLogo framework="FastifyJS" title="Валидация и сериализация"/>

```ts twoslash
// @noErrors
import { TypeBoxValidatorCompiler, TypeBoxTypeProvider } from "@fastify/type-provider-typebox";
import { Type } from "@sinclair/typebox";
import Fastify from "fastify";

const fastify = Fastify().setValidatorCompiler(TypeBoxValidatorCompiler).withTypeProvider<TypeBoxTypeProvider>();

fastify.get(
  "/route",
  {
    schema: {
        querystring: Type.Object({
            foo: Type.Number(),
            fooBar: Type.String(),
        }),
    },
  },
  (request, reply) => {
    request.query.; // type safe!
    //       	    ^|
  },
);
```

<!-- 
В Fastify под коробкой используется AJV, который обеспечивает валидацию. 
 -->

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Миграция с Express"/>

```ts twoslash
import fastifyExpress from "@fastify/express";
import Express from "express";
import Fastify from "fastify";

const fastify = Fastify();

await fastify.register(fastifyExpress);

const router = Express.Router();
fastify.use(router);

router.use((req, res, next) => {
    res.setHeader("x-custom", "fastify + express");
    next();
});

router.get("/hello", (req, res) => {
    res.status(201).json({ hello: "world" });
});

fastify.listen({ port: 3000 }, console.log);
```

<!-- Очень интересно, что у Fastify есть плагин который обеспечивает совместимость с express -->

<!-- ---

https://github.com/fastify/fastify/issues/5116 -->

---
layout: default
title: Elysia
---

<SlideLogo framework="ElysiaJS" title="Фичи"/>

<p class="text-green">Плюсы</p>

<v-clicks>

- Умные типы
- e2e type-safety
- Производительность
- WinterCG совместим (Web API)
- Life-cycle hooks
- Тесно связан с swagger/OpenAPI
- JSX 
- Powered by Bun

</v-clicks>
<p class="text-red">Минусы</p>
<v-clicks>

- Powered by Bun
- Молодой

</v-clicks>

<!-- 
Рассказать о бенчмарке fast-json-stringify в бане

**Из плюсов:**

[Click] Умные типы. Elysia спроектирована так чтобы вам приходилось писать меньше типов.
[Click] e2e type-safety. Elysia даёт возможность экспортировать типы бекенда в монорепе и использовать в клиенте.
[Click] Производительность. Elysia производительна благодаря Static Code Analysis, and Dynamic Code Injection // TODO: покопаться и добавить что typebox такой же
[Click] WinterCG. Построен на Web API стандартах.
[Click] Life-cycle hooks. Удобная работа с жизненным циклом запроса.
[Click] OpenAPI. Elysia имеет встроенный валидатор TypeBox, который обеспечивает хорошую совместимость с OpenAPI.
[Click] JSX. Имеет плагин для работы с JSX
[Click] Powered by Bun. Bun предоставляет отличный DX и perfomance но он ещё не такой стабильный как хотелось бы что можно так же отнести и к минусу


-->

<!-- <img src="/feature-sheet.webp"/> -->

---
layout: default
---

<SlideLogo framework="ElysiaJS" title="Быстрый"/>

// TODO: Здесь стоит рассказать о Powered by Bun Static code generation+TypeBox and etc

---
layout: default
---

<SlideLogo framework="ElysiaJS" title="Валидация"/>

```ts twoslash
import { Elysia, t } from "elysia";

new Elysia().post(
    "/yandex/question",
    ({ body }) => ({ message: "Question created!" }),
    // ^?
    //
    //
    //
    //
    {
        body: t.Object({
            presenter: t.TemplateLiteral("{Доклад 1|Доклад 2}"),
            text: t.String(),
        }),
        response: t.Object({
            message: t.String(),
        }),
    },
);
```

<!-- Валидация же представляет из себя typebox c расширенными возможностями -->

---

<SlideLogo framework="ElysiaJS" title="OpenAPI / Swagger"/>

<!-- prettier-ignore -->
```ts twoslash
// @filename: routes.ts
import { Elysia } from "elysia";

export const feed = new Elysia();
export const users = new Elysia();
// @filename: index.ts
// ---cut---
import { swagger } from "@elysiajs/swagger";
import { Elysia, t } from "elysia";
import { feed, users } from "./routes";

new Elysia()
	.use(swagger())
	.use(feed)
	.use(users)
	.listen(3000);
```

---
layout: full
---

<img src="/scalar-dark-mode.webp" />

---

<SlideLogo framework="ElysiaJS" title="e2e type-safety | лучше разместить"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia()
    .post("/yandex/employee", () => {}, {
        body: t.Object({
            name: t.String(),
            stack: t.Array(t.TemplateLiteral("{Elysia|React|Effector}")),
        }),
    })
    .listen(1997);

export type App = typeof app;
```

```ts twoslash
// @filename: server.ts
import { Elysia, t } from "elysia";

const app = new Elysia()
    .post("/yandex/employee", () => {}, {
        body: t.Object({
            name: t.String(),
            stack: t.Array(t.TemplateLiteral("{Elysia|React|Effector}")),
        }),
    })
    .listen(1997);

export type App = typeof app;
// @filename: index.ts
// ---cut---
import { treaty } from "@elysiajs/eden";
import type { App } from "./server";

const eden = treaty<App>("http://localhost:1997");

await eden.yandex.employee.post({
    name: "Всеволод",
    stack: ["Elysia", "Svelte"],
});
```

---
layout: full
---

<img class="w-full" src="/WinterCG.png"/>

---

<SlideLogo framework="ElysiaJS" title="WinterCG"/>

```ts twoslash
import { Elysia } from "elysia";
import { Hono } from "hono";
// ---cut---
const elysia = new Elysia()
    .get(
        "/",
        "Hello from Elysia inside Hono inside Elysia",
    );

const hono = new Hono()
    .get("/", (c) => c.text("Hello from Hono!"))
    .mount("/elysia", elysia.fetch);

const main = new Elysia()
    .get("/", () => "Hello from Elysia")
    .mount("/hono", hono.fetch)
    .listen(3000);
```

---

<SlideLogo framework="ElysiaJS" title="Context"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app.get(
    "/:type",
    ({ query: { pageSize }, params: { type }, set }) => {
        set.status = "I'm a teapot";
        set.headers["Content-Type"] = "application/x-teapot";

        return "ok";
    },
    {
        query: t.Object({
            pageSize: t.Numeric(),
        }),
    },
);
```

---

<SlideLogo framework="ElysiaJS" title="State & Decorate"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app
    .state("requests", 1)
    .get("/increment", ({ store }) => ++store.requests);
```

<br/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
class Logger {
    log(text: string) {}
}
// ---cut---
app
    .decorate("logger", new Logger())
    .get("/", ({ logger }) => {
        logger.log("hi");

        return "hi";
    });
```

---

<SlideLogo framework="ElysiaJS" title="Derive"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app
    .derive(({ headers }) => {
        const auth = headers.Authorization;

        return {
            bearer: auth?.startsWith("Bearer ") ? auth.slice(7) : null,
        };
    })
    .get("/", ({ bearer }) => bearer);
```

---

<SlideLogo framework="ElysiaJS" title="Affix"/>

<!-- prettier-ignore -->
```ts twoslash
import { Elysia } from "elysia";

// ---cut---
const setup = new Elysia({ name: "setup" })
            .decorate({
                argon: "a",
                boron: "b",
                carbon: "c",
            });

const app = new Elysia()
    .use(setup.prefix("decorator", "setup"))
    .get("/", ({ setupCarbon }) => setupCarbon);
//                                  ^?

```

---

<SlideLogo framework="ElysiaJS" title="Elysia plugin"/>

<div class="flex justify-between">
<div/>
<img class="-mt-20 -mr-10 scale-90" width="500" src="/new-elysia-twitter.png" /> 

</div>
---

<SlideLogo framework="ElysiaJS" title="Elysia plugin"/>

```ts twoslash
import { Elysia } from "elysia";

const app = new Elysia();

class YandexController {
    createEvent(name: string) {}
}
// ---cut---
const plugin = new Elysia()
    .decorate("yandex", new YandexController())
    .get("/", () => "Hello, Yandex!");

app.use(plugin)
    .post("/event", ({ yandex }) => yandex.createEvent("Я 💛 Фронтенд"))
    .listen(3000);
```

---
layout: full
---

<img src="/lifecycle.webp" />

---

<SlideLogo framework="ElysiaJS" title="Life-cycle"/>

```ts twoslash
import { isHtml } from "@elysiajs/html";
import { Elysia } from "elysia";

// ---cut---
new Elysia()
    .get("/none", () => "<h1>Hello World</h1>")
    .onAfterHandle(({ response, set }) => {
        if (isHtml(response))
            set.headers["Content-Type"] = "text/html; charset=utf8";
    })
    .get("/", () => "<h1>Hello World</h1>")
    .get("/hi", () => "<h1>Hello World</h1>")
    .listen(3000);
```

---

<SlideLogo framework="ElysiaJS" title="Guard"/>

```ts twoslash
import { Elysia, t } from "elysia";

const signIn = (body: any) => "";
const signUp = (body: any) => "";
// ---cut---
new Elysia().guard(
    {
        body: t.Object({
            username: t.String(),
            password: t.String(),
        }),
    },
    (app) =>
        app
            .post("/sign-up", ({ body }) => signUp(body))
            .post("/sign-in", ({ body }) => signIn(body)),
);
```

---

<SlideLogo framework="ElysiaJS" title="Scoped плагины"/>

```ts twoslash
import { isHtml } from "@elysiajs/html";
import { Elysia } from "elysia";

// ---cut---
const html = new Elysia({ scoped: true })
    .onAfterHandle(({ set, response }) => {
        if (isHtml(response))
            set.headers["Content-Type"] = "text/html; charset=utf8";
    })
    .get("/inner", () => "<h1>Hello World</h1>");

new Elysia()
    .get("/", () => "<h1>Hello World</h1>")
    .use(html)
    .get("/outer", () => "<h1>Hello World</h1>")
    .listen(3000);
```

---

<SlideLogo framework="ElysiaJS" title="Загрузка файлов"/>

```ts twoslash
import { Elysia, t } from "elysia";

// ---cut---
new Elysia().post(
    "/upload/avatar",
    async ({ body: { avatar } }) => {
        await Bun.write(`${process.cwd()}/${avatar.name}`, avatar);
    },
    {
        body: t.Object({
            description: t.String(),
            avatar: t.File({
                type: "image/png",
                maxSize: "5m",
            }),
        }),
    },
);
```

---

<SlideLogo framework="ElysiaJS" title="Error handling"/>

```ts twoslash
import { Elysia } from "elysia";

// ---cut---
class APIError extends Error {
    constructor(public typeSafeCode: "UNAUTHORIZED" | "SOME_REASON") {
        super();
        this.message = "An Error occurred";
    }
}

new Elysia()
    .error({
        APIError,
    })
    .onError(({ code, error }) => {
        if (code === "APIError") {
            console.error(error.typeSafeCode);
            //                      ^?
        }
    })
    .get("/teapot", () => {
        throw new APIError("SOME_REASON");
    });
```

---

<SlideLogo framework="ElysiaJS" title="Cookie"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app.get(
    "/",
    ({ cookie: { user } }) => {
        user.value = {
            id: 1,
            company: "Yandex",
            name: "Summoning 101",
        };
    },
    {
        cookie: t.Cookie({
            user: t.Object({
                id: t.Numeric(),
                company: t.TemplateLiteral("{Yandex|Elytrium}"),
                name: t.String(),
            }),
        }),
    },
);
```

---

<SlideLogo framework="ElysiaJS" title="WebSocket"/>

```ts twoslash
import { Elysia, t } from "elysia";

// ---cut---
new Elysia()
    .ws("/ws", {
        body: t.Object({
            message: t.String(),
        }),
        message(ws, { message }) {
            ws.send({
                message,
                time: Date.now(),
            });
        },
    })
    .listen(8080);
```

---

<SlideLogo framework="ElysiaJS" title="Tests with eden"/>

```ts twoslash
import { treaty } from "@elysiajs/eden";
import { Elysia } from "elysia";
import { describe, it, expect } from "bun:test";

// ---cut---
const app = new Elysia().get("/", () => "hi").listen(3000);

const api = treaty<typeof app>(app);

describe("Elysia", () => {
    it("return a response", async () => {
        const { data } = await api.index.get();

        expect(data).toBe("hi");
    });
});
```

---

<SlideLogo framework="ElysiaJS" title="Macro"/>

```ts twoslash
import { Elysia } from "elysia";

// ---cut---
const plugin = new Elysia({ name: "plugin" }).macro(({ onBeforeHandle }) => {
    return {
        hi(word: string) {
            onBeforeHandle(() => {
                console.log(word);
            });
        },
    };
});

new Elysia()
    .use(plugin)
    .get("/", () => "hi", {
        hi: "Elysia",
    });
```

---

<SlideLogo framework="ElysiaJS" title="Websocket e2e type-safety"/>

```ts twoslash
import { Elysia, t } from "elysia";

// ---cut---
const app = new Elysia()
    .ws("/chat", {
        message(ws, message) {
            ws.send(message);
        },
        body: t.String(),
        response: t.String(),
    })
    .listen(8080);

export type App = typeof app;
```

<br/>

```ts twoslash
import { treaty } from "@elysiajs/eden";
import { Elysia, t } from "elysia";

const app = new Elysia()
    .ws("/chat", {
        message(ws, message) {
            ws.send(message);
        },
        body: t.String(),
        response: t.String(),
    })
    .listen(8080);

export type App = typeof app;

// ---cut---
const client = treaty<App>("http://localhost:8080");

const chat = client.chat.subscribe();

chat.subscribe((message) => {
    console.log("got", message);
});

chat.send("hello from client");
```

---

<SlideLogo framework="ElysiaJS" title="Model"/>

```ts twoslash
import { Elysia, t } from "elysia";

// ---cut---
new Elysia()
    .model({
        sign: t.Object({
            username: t.String(),
            password: t.String(),
        }),
    })
    .post(
        "/sign-in",
        ({ body }) => body,
        //            ^?
        //
        //
        //
        //
        {
            body: "sign",
            response: "sign",
        },
    );
```

---

<SlideLogo framework="ElysiaJS" title="JSX/HTML"/>

```tsx twoslash
// @jsx: react
// @jsxFactory: Html.createElement
// @jsxFragmentFactory: Html.Fragment
// ---cut---
import { Elysia } from 'elysia'
import { html } from '@elysiajs/html' 

new Elysia()
    .use(html()) 
    .get('/', () => (
        <html lang="en">
            <head>
                <title>Hello World</title>
            </head>
            <body>
                <h1>Hello World</h1>
            </body>
        </html>
    ))
```

<!-- 
Elysia имеет плагин для работы c HTML/JSX. Он так же вам позволит защищаться от xss атак 
 -->

---

<div class="flex items-center justify-center h-full text-5xl">

Спасибо за внимание! ✨

</div>

---

<QRCodesSlide/>