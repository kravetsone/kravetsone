---
theme: elysia
mdc: true
highlighter: shiki
layout: cover
export:
    format: pdf
    dark: true
    withClicks: true
---

<div class="h-full flex flex-col justify-between">

<h1 class="w-2xl">Познаём Elysia и Bun — фреймворк, сделанный для людей</h1>

<div class="flex items-center justify-between">

<div class="flex flex-col gap-0 pt-25 text-xl">
    <span class="font-bold">Всеволод Деткин</span>
    <span>Бекенд разработчик, Элитриум</span>
</div>

<img class="-mr-25" width="500px" height="500px" src="/fox.webp" />

</div>

</div>

---
layout: default
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

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Разработчики забили"/>

<div class="mt-7"/>

-   4.18.2 (latest) - год назад
-   4.18.1 - 2 года назад
-   4.18.0 - 2 года назад
-   4.17.3 - 2 года назад
-   5.0.0-beta.1 - 2 года назад
-   4.17.2 - 2 года назад
-   5.0.0-alpha.8 - 4 года назад
-   4.1.1 - 5 лет назад

// TODO: Придумать как обыграть покрасивее

---
layout: default
title: Koa
---

<SlideLogo framework="KoaJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

<v-clicks>

-   Написан командой ExpressJS
-   Значительно быстрее ExpressJS

</v-clicks>

<p class="text-red">Минусы</p>

<v-clicks>

-   Middleware
-   Плохая типизация
-   Плохая работа с валидацией

</v-clicks>

---
layout: default
title: Fastify
---

<SlideLogo framework="FastifyJS" title="Плюсы и минусы"/>

<p class="text-green">Плюсы</p>

-   Современный
-   Life-cycle hooks
-   Производительный
-   fast-json-stringify
-   Построен на JSON Schema и AJV
-   Отличный DX и swagger одной строчкой
-   express-compatibility plugin

<p class="text-red">Минусы</p>

-   Не идеал типизации

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Life-cycle hooks"/>

Request => Routing => Logger => onRequest Hook => preParsing Hook => Parsing => preValidation Hook => Validation => preHandler Hook => User Handler => Reply => preSerialization Hook => onSend Hook => Response => onResponse Hook

// TODO: flow chart

---
layout: default
---

<ShowTwoslash />
<SlideLogo framework="FastifyJS" title="Валидация и сериализация"/>

<div class="mt-7"/>

<ShowTwoslash />

```ts twoslash
// @noErrors
import { TypeBoxTypeProvider } from "@fastify/type-provider-typebox";
import { Type } from "@sinclair/typebox";
import Fastify from "fastify";

const fastify = Fastify().withTypeProvider<TypeBoxTypeProvider>();

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

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Миграция с Express"/>

<div class="mt-7"/>

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
router.use("*", (req, res) => {
    res.status(404).json({ msg: "not found" });
});

fastify.listen({ port: 3000 }, console.log);
```

---
layout: default
title: Elysia
---

<SlideLogo framework="ElysiaJS" title="Фичи"/>

<img src="/feature-sheet.webp"/>

---
layout: default
---

<SlideLogo framework="ElysiaJS" title="Быстрый"/>

// TODO:

---
layout: default
---

<ShowTwoslash />
<SlideLogo framework="ElysiaJS" title="Валидация"/>

<div class="mt-7"/>

```ts twoslash
import { Elysia, t } from "elysia";

new Elysia().post(
    "/yandex/question",
    ({ body }) => ({ message: "Question created!" }),
    // ^?
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

---

<SlideLogo framework="ElysiaJS" title="OpenAPI / Swagger"/>

<div class="mt-7"/>

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

<ShowTwoslash />

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
    .post("/yandex/another", () => {}, {
        body: t.Object({
            name: t.String(),
            stack: t.Array(t.TemplateLiteral("{Elysia|React|Effector}")),
        }),
    })
    .listen(1997);

export type App = typeof app;
// @filename: index.ts
// ---cut---
import { edenTreaty } from "@elysiajs/eden";
import type { App } from "./server";

const eden = edenTreaty<App>("http://localhost:1997");

await eden.yandex.employee.post({
    //            ^|
    name: "Всеволод",
    stack: ["Elysia", "Svelte"],
});
```

---

<SlideLogo framework="ElysiaJS" title="WinterCG"/>

<div class="mt-7"/>

```ts twoslash
import { Elysia } from "elysia";
import { Hono } from "hono";
// ---cut---
const elysia = new Elysia().get(
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

<div class="mt-7"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app.get(
    "/:type",
    ({ query: { pageSize }, params: { type }, set }) => {
        set.status = "I'm a teapot";
        set.headers["Content-Type"] = "application/x-teapot";
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

<div class="mt-7"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app.state("requests", 1).get("/increment", ({ store }) => ++store.requests);
```

<br/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
class Logger {
    log(text: string) {}
}
// ---cut---
app.decorate("logger", new Logger()).get("/", ({ logger }) => {
    logger.log("hi");

    return "hi";
});
```

---

<SlideLogo framework="ElysiaJS" title="Derive"/>

<div class="mt-7"/>

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app.derive(({ headers }) => {
    const auth = headers.Authorization;

    return {
        bearer: auth?.startsWith("Bearer ") ? auth.slice(7) : null,
    };
}).get("/", ({ bearer }) => bearer);
```

---

<ShowTwoslash />
<SlideLogo framework="ElysiaJS" title="Affix"/>

<div class="mt-7"/>

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

<div class="mt-7"/>

TODO: переделать кнш

<img width="500" src="/new-elysia-twitter.png" /> 

---

<SlideLogo framework="ElysiaJS" title="Elysia plugin"/>

<div class="mt-7"/>

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

## layout: full

<img src="/lifecycle.webp" />

---

<SlideLogo framework="ElysiaJS" title="Life-cycle"/>

<div class="mt-7"/>

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

<div class="mt-7"/>

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

<div class="mt-7"/>

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

## layout: full

<img src="/elysia-migration-issue.png" />

---

<SlideLogo framework="ElysiaJS" title="Загрузка файлов"/>

<div class="mt-7"/>

```ts twoslash
import { Elysia, t } from "elysia";

// ---cut---
new Elysia().post(
    "/upload/avatar",
    async ({ body: { avatar } }) => {
        await Bun.write(`${process.cwd()}/${avatar.originalName}`, avatar);
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

<ShowTwoslash />
<SlideLogo framework="ElysiaJS" title="Error handling"/>

<div class="mt-7"/>

```ts twoslash
import { Elysia } from "elysia";

// ---cut---
class APIError extends Error {
    constructor(public typeSafeCode: "UNAUTHORIZED" | "NOT_TEAPOT") {
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
        throw new APIError("NOT_TEAPOT");
    });
```

---

<SlideLogo framework="ElysiaJS" title="Cookie"/>

<div class="mt-7"/>

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

<div class="mt-7"/>

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

<div class="mt-7"/>

```ts twoslash
import { edenTreaty } from "@elysiajs/eden";
import { Elysia } from "elysia";
import { describe, it, expect } from "bun:test";

// ---cut---
const app = new Elysia().get("/", () => "hi").listen(3000);

const api = edenTreaty<typeof app>("http://localhost:3000");

describe("Elysia", () => {
    it("return a response", async () => {
        const { data } = await api.get();

        expect(data).toBe("hi");
    });
});
```

---

<SlideLogo framework="ElysiaJS" title="Macro"/>

<div class="mt-7"/>

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

const app = new Elysia().use(plugin).get("/", () => "hi", {
    hi: "Elysia",
});
```

---

<SlideLogo framework="ElysiaJS" title="Websocket e2e type-safety"/>

<div class="mt-7"/>

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
import { edenTreaty } from "@elysiajs/eden";
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
const client = edenTreaty<App>("http://localhost:8080");

const chat = client.chat.subscribe();

chat.subscribe((message) => {
    console.log("got", message);
});

chat.send("hello from client");
```

---

<ShowTwoslash />
<SlideLogo framework="ElysiaJS" title="Model"/>

<div class="mt-7"/>

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
