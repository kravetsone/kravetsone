---
theme: elysia
mdc: true
highlighter: shiki
layout: cover
addons:
  - slidev-addon-qrcode
  - slidev-component-progress
twoslash: true
export:
  timeout: 60000
  format: pdf
  dark: true
contextMenu: false
---

<div class="h-full flex flex-col justify-between">

<h1 class="w-2xl text-elysia-sky-indigo">Познаём Elysia и Bun — фреймворк, сделанный для людей</h1>

<div class="flex items-center justify-between">

<div class="flex flex-col gap-0 pt-70 text-xl">
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
---

<div class="flex justify-between">
<div class="flex flex-col flex-items-center w-full p-5">
    <h1 class="text-xl flex-self-start">Обо мне</h1>
    <ul class="flex-self-start">
    <v-clicks>
        <li>Победитель двух этапов хакатона «Цифровой прорыв»</li>
        <li>Люблю заниматься опенсорсом и пет-проектами</li>
        <li>Занимаюсь разработкой фреймворка для телеграм ботов - GramIO</li>
        <li>Заканчиваю «Московский индустриальный колледж»</li>
        <li>Начал джаваскриптить ещё в 14 лет</li>
        <li>И да я люблю перекладывать JSON'ы</li> 
    </v-clicks>
    </ul>
</div>

<img width="500" class="h-[450px]" src="/me.jpg" />
</div>

---

<h2 class="text-center">В этом докладе</h2>

<div class="flex items-center justify-center gap-5 text-8xl">

<skill-icons-expressjs-light class="text-7xl"/>

<simple-icons-koa />

<simple-icons-fastify />

<img src="https://elysiajs.com/assets/elysia.svg" width="90px" />

</div>

<div class="mt-7">

<v-clicks>

-   Поговорим почему ExpressJS уже не актуален
-   Посмотрим на существующие фреймворки и их плюсы/минусы
-   Попробуем фреймворк для Bun, который набирает популярность
</v-clicks>

</div>

<!-- *контекст - почему про это говорим, что хоти полезного дать (убеждающая агенда)
*общая интерпретация (зона применения, что ты думаешь об инструменте, что должен понять слушатель)

-   переходы!!!!
-   в рассказе про Элизию нужна внутренняя структура (типы преимуществ
-   выводы
-   общая структура (2 части - что было, до элизиума) -->

---
title: ExpressJS
---

<div class="flex justify-center items-center h-full text-6xl gap-5">
<skill-icons-expressjs-light />
<h1>ExpressJS</h1>
</div>

---
title: Express
src: ./pages/framework-cover/express/1.md
---

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

<div class="flex items-center justify-around h-full -mt-7">

<div class="flex flex-col justify-center items-center">
/some/:value

<formkit-arrowdown class="text-4xl" />
<div>
<logos-npm-icon /> path-to-regexp
</div>
<formkit-arrowdown class="text-4xl" />

/^\/some\/(?:([^\/]+?))\/?$/

<p class="font-bold">⚠️ potential ReDoS attacks</p>
</div>

<p class="font-bold text-4xl">VS</p>

<img src="/radix.png" width="400" class="invert -mr-5" />

</div>

<div class="absolute top-5 right-5">
<QRCode type="canvas" data="https://forbeslindesay.github.io/express-route-tester/"
            :dotsOptions="{ type: 'extra-rounded', color: 'purple' }" :width="150"
            :height="150" />
</div>

<!--
TODO: переделать слайд визуально.
В роутинге Express'а не применяется хитрых алгоритмов по типу RadixTree, которые можно встретить у конкурентов.
Express же использует библиотеку path-to-regexp которая превращает ваш текст в регулярное выражение
-->

---
title: Express
src: ./pages/framework-cover/express/2.md
---


---
layout: default
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

<SlideLogo framework="ExpressJS" title="Middleware"/>

<div class="flex items-center justify-around h-full -mt-7">

<img src="/middleware-never-again.webp" width="500" />

<QRCode type="canvas" data="https://youtu.be/RS8x73z4csI?si=BmdaXXEDSI2GUZ94" image="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='1.43em' height='1em' viewBox='0 0 256 180'%3E%3Cpath fill='%23f00' d='M250.346 28.075A32.18 32.18 0 0 0 227.69 5.418C207.824 0 127.87 0 127.87 0S47.912.164 28.046 5.582A32.18 32.18 0 0 0 5.39 28.24c-6.009 35.298-8.34 89.084.165 122.97a32.18 32.18 0 0 0 22.656 22.657c19.866 5.418 99.822 5.418 99.822 5.418s79.955 0 99.82-5.418a32.18 32.18 0 0 0 22.657-22.657c6.338-35.348 8.291-89.1-.164-123.134'/%3E%3Cpath fill='%23fff' d='m102.421 128.06l66.328-38.418l-66.328-38.418z'/%3E%3C/svg%3E" :imageOptions="{ margin: 10 }"
            :dotsOptions="{ type: 'extra-rounded', color: 'purple' }" :width="200"
            :height="200" />

</div>

---
title: Express
src: ./pages/framework-cover/express/3.md
---

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
title: Express
src: ./pages/framework-cover/express/4.md
---

---
layout: full
---

<img src="/swagger-autogen.png"/>

---
title: Express
src: ./pages/framework-cover/express/5.md
---

---
layout: default
---

<SlideLogo framework="ExpressJS" title="Разработка заглохла" />

-   4.19.2 (latest) - 2 месяца назад

...

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
layout: default
---

<SlideLogo framework="ExpressJS" title="HyperExpress"/>

<img src="/hyper-express.png" width="600"/>

<div class="absolute top-5 right-5">
<QRCode type="canvas" data="https://forbeslindesay.github.io/express-route-tester/" image="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='1em' height='1em' viewBox='0 0 256 256'%3E%3Cpath fill='white' d='M216 104v8a56.06 56.06 0 0 1-48.44 55.47A39.8 39.8 0 0 1 176 192v40a8 8 0 0 1-8 8h-64a8 8 0 0 1-8-8v-16H72a40 40 0 0 1-40-40a24 24 0 0 0-24-24a8 8 0 0 1 0-16a40 40 0 0 1 40 40a24 24 0 0 0 24 24h24v-8a39.8 39.8 0 0 1 8.44-24.53A56.06 56.06 0 0 1 56 112v-8a58.14 58.14 0 0 1 7.69-28.32A59.78 59.78 0 0 1 69.07 28A8 8 0 0 1 76 24a59.75 59.75 0 0 1 48 24h24a59.75 59.75 0 0 1 48-24a8 8 0 0 1 6.93 4a59.74 59.74 0 0 1 5.37 47.68A58 58 0 0 1 216 104'/%3E%3C/svg%3E"
            :dotsOptions="{ type: 'extra-rounded', color: 'purple' }" :imageOptions="{ margin: 5 }" :width="150"
            :height="150" />
</div>

---
title: KoaJS
---

<div class="flex justify-center items-center h-full text-6xl gap-5">
<simple-icons-koa  />
<h1>KoaJS</h1>
</div>

---
layout: default
title: Koa
src: ./pages/framework-cover/koa.md
---


---

<SlideLogo framework="KoaJS" title="Генераторы"/>

```ts
var koa = require("koa");
var app = koa();

app.use(function* (next) {
    var start = new Date();
    yield next;
    var ms = new Date() - start;
    console.log("%s %s - %s", this.method, this.url, ms);
});

app.use(function* () {
    this.body = "Hello world!";
});

app.listen(3000);
```

<!--
KoaJS назывался «фреймворком нового поколения» и решал проблему callback hell ещё до появления сахара в виде async/await
но после появления их появления люди так и остались на express
-->

---
title: FastifyJS
---

<div class="flex justify-center items-center h-full text-6xl gap-5">
<simple-icons-fastify  />
<h1>FastifyJS</h1>
</div>

---
layout: default
title: Fastify
src: ./pages/framework-cover/fastify/1.md
---

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Life-cycle hooks"/>

<!-- Request => Routing => Logger => onRequest Hook => preParsing Hook => Parsing => preValidation Hook => Validation => preHandler Hook => User Handler => Reply => preSerialization Hook => onSend Hook => Response => onResponse Hook -->

<div class="flex">

```ts
fastify.addHook("preParsing", async (request, reply, payload) => {
    await asyncMethod();

    return newPayload;
});
fastify.addHook("preHandler", async (request, reply) => {
    await asyncMethod();
});
fastify.addHook("onSend", async (request, reply, payload) => {
    const newPayload = payload.replace("some-text", "some-new-text");

    return newPayload;
});

fastify.post("/", (request, reply) => {
    // some logic
});
```

<div class="flex flex-col ml-35">

-   onRequest
-   preParsing
-   preValidation
-   preHandler
-   preSerialization
-   onError
-   onSend
-   onResponse
-   onTimeout
-   onRequestAbort
</div>
</div>

<!--
// TODO: flow chart
-->

---
layout: default
title: Fastify
src: ./pages/framework-cover/fastify/2.md
---

---
layout: default
---

<SlideLogo framework="FastifyJS" title="fast-json-stringify"/>

<div class="flex justify-around items-center -mt-7">

```ts
import fastJson from "fast-json-stringify";

const stringify = fastJson({
  type: 'object',
  properties: {
    firstName: {
      type: 'string'
    },
    age: {
      description: 'Age in years',
      type: 'integer'
    },
  }
});

console.log(stringify({
  firstName: "Name",
  age: 32,
}));
```

<img src="/fast-json-stringify.png" width="230" />
</div>

---
layout: default
---

<SlideLogo framework="FastifyJS" title="Type-provider"/>

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
src: ./pages/framework-cover/fastify/3.md
---


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

<!--
https://github.com/fastify/fastify/issues/5116
-->

---

<SlideLogo framework="FastifyJS" title="Миграция с Express"/>

<div class="text-center">

| <skill-icons-expressjs-light /> Middleware |  | <simple-icons-fastify /> Plugin |
| ------------------------------------------ | ------------------------------------------ | :-----------------------------: |
| helmet                                     | |        @fastify/helmet         |
| cors                                       | |         @fastify/cors          |
| serve-static                               | |        @fastify/static         |
| Passport.js                                | |       @fastify/passport        |
| multer                                     | |        fastify-multer          |

<p>И так далее...</p>

</div>

---
layout: default
src: ./pages/framework-cover/fastify/4.md
---

---

<SlideLogo framework="FastifyJS" title="Matteo Collina"/>

<div class="flex gap-10 justify-center items-center">

<img src="https://nodeland.dev/profile.jpg" width="400px">

<div>
<v-clicks>

- Член Node.js Technical Steering Committee
- Активно выступает с докладами (спикер более **60** конференций)
- Со-автор книги по разработке с Fastify

</v-clicks>
</div>


</div>

---

<SlideLogo framework="FastifyJS" title="Matteo Collina"/>

<div class="flex flex-col gap-10 justify-center items-center">



<img src="/fastify-youtube.png" width="500px"/>


</div>

<div class="absolute top-5 right-5">
<QRCode type="canvas" data="https://www.youtube.com/watch?v=gltzZjKYK1I" image="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='1.43em' height='1em' viewBox='0 0 256 180'%3E%3Cpath fill='%23f00' d='M250.346 28.075A32.18 32.18 0 0 0 227.69 5.418C207.824 0 127.87 0 127.87 0S47.912.164 28.046 5.582A32.18 32.18 0 0 0 5.39 28.24c-6.009 35.298-8.34 89.084.165 122.97a32.18 32.18 0 0 0 22.656 22.657c19.866 5.418 99.822 5.418 99.822 5.418s79.955 0 99.82-5.418a32.18 32.18 0 0 0 22.657-22.657c6.338-35.348 8.291-89.1-.164-123.134'/%3E%3Cpath fill='%23fff' d='m102.421 128.06l66.328-38.418l-66.328-38.418z'/%3E%3C/svg%3E" :imageOptions="{ margin: 10 }"
            :dotsOptions="{ type: 'extra-rounded', color: 'purple' }" :width="180"
            :height="180" />
</div>

---
layout: default
src: ./pages/framework-cover/fastify/5.md
---

---

<SlideLogo framework="FastifyJS" title="Decorate"/>

<!-- https://fastify.dev/docs/latest/Reference/Decorators/#decorators -->

```ts {all|13-17}
fastify.decorateRequest("user", "");


fastify.addHook("preHandler", (req, reply, done) => {
    req.user = "Bob Dylan";
    done();
});

fastify.get("/", (req, reply) => {
    reply.send(`Hello, ${req.user}!`);
});

declare module "fastify" {
    export interface FastifyRequest {
        user: string;
    }
}
```

<!-- Декорирование основных объектов с помощью этого API позволяет оптимизировать обработку объектов сервера, запросов и ответов. Это достигается путем определения формы всех таких экземпляров объектов до их создания и использования -->

---

<div class="flex justify-center items-center h-full text-6xl">
<img src="https://kita.js.org/logo.svg" width="100px" />
<h1>KitaJS</h1>
</div>

---

<SlideLogo framework="KitaJS" title="Fastify роутер"/>

<pre>
src/routes
├── auth.ts               (/auth)
├── posts
│   └── index.ts          (/posts)
└── users
    ├── [id]
    │   ├── index.ts      (/users/:id)
    │   └── posts.ts      (/users/:id/posts)
    ├── index.ts          (/users)
    └── me
        ├── index.ts      (/users/me)
        └── posts.ts      (/users/me/posts)
</pre>

---

<SlideLogo framework="KitaJS" title="Endpoint"/>

```ts
// src/routes/users.ts
import type { Body, Header } from "@kitajs/runtime";

interface CreateUser {
    /**
     * The name of the user
     *
     * @minLength 3
     * @maxLength 20
     */
    name: string;
}

// Creates a new user
export function post(
    data: Body<CreateUser>,
    userAgent: Header<"user-agent">,
    rawRequest: FastifyRequest,
) {
    // ...
}
```

---

<SlideLogo framework="KitaJS" title="Provider"/>

```ts
import type { FastifyRequest } from "fastify";

/**
 * When this type is used as a route parameter, the function below will be
 * called
 */
export type UserAgent = string | undefined;

/**
 * We can also use almost any other parameter type, like FastifyRequest or even
 * other providers
 */
export default function ({ headers }: FastifyRequest): UserAgent {
    return headers["user-agent"];
}
```

---
title: ElysiaJS
---

<div class="flex justify-center items-center h-full text-6xl gap-5">
<img src="https://elysiajs.com/assets/elysia.svg" width="100" />
<h1>ElysiaJS</h1>
</div>


---

<SlideLogo framework="ElysiaJS" title="Факты"/>

<div class="flex justify-between">

<div class="flex flex-col items-center gap-8">

<div>

- Новый фреймворк с интересными идеями
- `Elysia` (и не только) названа в честь персонажа из игры `Honkai impact 3`
- В разработке около **2** лет
- Раньше назывался `KingsWorld`
- Можно сказать самый популярный `Bun` фреймворк
</div>




<img src="https://elysiajs.com/assets/elysia_v.webp" width="300px" class="opacity-80"/>

</div>

<SlidevVideo autoplay muted loop class="h-full mt--30 mr--14" width="280px">
  <!-- Anything that can go in a HTML video element. -->
  <source src="/subway.mp4" type="video/mp4" />
  <p>
    Your browser does not support videos. You may download it
    <a href="/myMovie.mp4">here</a>.
  </p>
</SlidevVideo>
<!-- <img class="-mt-20 -mr-10 scale-90" width="450" src="/elysia-stack.png" /> -->

</div>

---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/1.md
---

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

<!--
Валидация же представляет из себя typebox c расширенными возможностями
-->

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
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/2.md
---

---

<SlideLogo framework="ElysiaJS" title="Life-cycle"/>

<div class="flex items-center justify-between">

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

<div class="flex flex-col mr-25">

-   Request
-   Parse
-   Transform
-   Before Handle
-   After Handle
-   Map Response
-   Error
-   Response
-   Trace

</div>
</div>




---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/3.md
---

---
layout: default
---

<SlideLogo framework="ElysiaJS" title="Производительность"/>

<img class="mt-7" src="/benchmark.png"/>

---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/4.md
---



---

<SlideLogo framework="ElysiaJS" title="о Bun"/>

<div class="flex gap-5 justify-around items-center h-full mt--7"> 

<div class="flex flex-col items-center">

<logos-bun class="text-9xl"/> 

<p class="font-bold text-4xl text-bun">Bun</p>

</div>

<div class="flex gap-3 list-square center-icon">
<v-clicks>

- Новый <span mx-2>**runtime**</span> основанный на <span ml-2>**JavaScriptCore**</span>
- <span mr-2>`1.0`</span>   вышел 8 сентября 2023 года
- Предоставляет <span mx-2>**Bun.\* API**</span>
- Из коробки работает с <span ml-2>**TS**, **JSX**</span>
- Главные принципы - «**Just works**»
- Выступает как <span ml-2>**All-in-one toolkit**</span>
- Может работать как <span mx-2>**Bundler**</span> и <span ml-2>**пакетный менеджер**</span>
- <span mr-2>**Node.js@22**</span> перенял опыт <span mx-2>**Bun**</span> по <span mx-2>`require(esm)`</span>
- "<logos-nodejs-icon class="text-4xl"/>".replace("<logos-nodejs-icon class="text-4xl"/>", "<logos-bun class="text-4xl"/>")
- Написан на <devicon-zig-wordmark class="text-5xl ml-2"/>
</v-clicks>

</div>

</div>

---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/5.md
---

---

<SlideLogo framework="ElysiaJS" title="Построен на Web API"/>

```ts
new Elysia().all("/download", async ({ request }) => {
	if (request.method !== "GET")
		return new Response(
			`You cannot download the file using ${request.method}`,
			{
				status: 418,
				statusText: "use GET",
				headers: {
					"x-required-method": "GET",
				},
			},
		);

	return new File([await fs.readFile("./cute-cat.png")], "cute-cat.png");
});
```
❌

```ts
import { FastifyRequest } from "fastify";
// or
import { Request } from "express"; // Перекрывает Request из Web API
// or
import { Request } from "koa"; // Перекрывает Request из Web API
```

---

<SlideLogo framework="ElysiaJS" title="Построен на Web API"/>

<!-- prettier-ignore -->
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
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/6.md
---

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

<SlideLogo framework="ElysiaJS" title="chainable typings & type-safety"/>

<!-- Один из кор принципов этого фреймворка это chainable типизация
Поэтому вам необходимо соблюдать эту цепочку

И если вы вдруг поделили то дедупликация происходит // подробнее -->

✅

<!-- prettier-ignore -->
```ts twoslash
import { Elysia } from "elysia";
// ---cut---
const app = new Elysia()
    .state('build', 1)
    .get('/', ({ store: { build } }) => build)
    //                                     ^?
```

<br/>

❌ Типы потеряны

```ts twoslash
import { Elysia } from "elysia";
// ---cut---
const app = new Elysia()

app.state('build', 1)

app.get('/', ({ store: { build } }) => build)
```


---

<SlideLogo framework="ElysiaJS" title="State & Decorate"/>

<!-- prettier-ignore -->
```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia();
// ---cut---
app
    .state("requests", 1)
    .get("/increment", ({ store }) => ++store.requests);
```

<br/>

<!-- prettier-ignore -->
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

<!-- prettier-ignore -->
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

<SlideLogo framework="ElysiaJS" title="Macro"/>

<!-- prettier-ignore -->
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
        {
            body: "sign",
            response: "sign",
        },
    );
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

<!-- prettier-ignore -->
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

app
    .use(plugin)
    .post("/event", ({ yandex }) => yandex.createEvent("Я 💛 Фронтенд"))
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

<SlideLogo framework="ElysiaJS" title="Global hooks в плагинах"/>

```ts twoslash
import { isHtml } from "@elysiajs/html";
import { Elysia } from "elysia";

// ---cut---
const html = new Elysia()
    .onAfterHandle({ as: "global" }, ({ set, response }) => {
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
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/7.md
---

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

<SlideLogo framework="ElysiaJS" title="e2e type-safety"/>

<div class="flex gap-2 justify-around">

```ts twoslash
import { Elysia, t } from "elysia";

const app = new Elysia()
    .post("/yandex/employee", () => {}, {
        body: t.Object({
            name: t.String(),
            stack: t.TemplateLiteral("{effector|react}"),
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
            stack: t.TemplateLiteral("{effector|react}"),
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
    name: "Alex",
    stack: "svelte",
});
```

</div>


---

<SlideLogo framework="ElysiaJS" title="Websocket e2e type-safety"/>

<div class="flex justify-between gap-2">

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

<skill-icons-typescript class="flex-self-center text-6xl" />

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
    //                  ^?
});

chat.send("hello from client");
```

</div>

<!-- ---

<SlideLogo framework="ElysiaJS" title="Переиспользование models на клиенте"/>

<div class="flex gap-2 justify-around">

```ts
new Elysia()
    .model({
        signIn: t.Object({
            username: t.String(),
            password: t.String(),
        }),
    })
    .post(
        "/sign-in",
        ({ body }) => body,
        {
            body: "signIn",
            response: "signIn",
        },
    );

export const { models } = app;
```

```ts
import { models } from "./server";

const value = models.sign.parse({
    username: "admin",
    password: "admin"
});

const { data, error } = models.sign.safeParse({
    username: "admin",
    password: "admin"
});
```

</div> -->

---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/8.md
---

---

<SlideLogo framework="ElysiaJS" title="JSX/HTML"/>

<!-- prettier-ignore -->
```tsx twoslash
// @jsx: react
// @jsxFactory: Html.createElement
// @jsxFragmentFactory: Html.Fragment
// ---cut---
import { Elysia } from "elysia";
import { html } from "@elysiajs/html";

new Elysia()
    .use(html())
    .get("/", () => (
        <html lang="en">
            <head>
                <title>Hello World</title>
            </head>
            <body>
                <h1>Hello World</h1>
            </body>
        </html>
    ));
```

<!-- мб привести пример с ejs -->

---
layout: default
title: Elysia
src: ./pages/framework-cover/elysia/9.md
---

---
layout: full
---

<img class="w-full" src="/documentation.png"/>

---

<div class="flex items-center justify-center h-full text-5xl">

Спасибо за внимание! ✨

</div>

---

<QRCodesSlide/>
