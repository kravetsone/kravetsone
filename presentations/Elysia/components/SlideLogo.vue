<script setup lang="ts">
const props = defineProps<{
    framework: "ExpressJS" | "KoaJS" | "FastifyJS" | "KitaJS" | "ElysiaJS",
    title: string
}>()

</script>

<template>
    <div class="flex flex-row items-center font-bold gap-2">
        <div class="text-3xl">
            <skill-icons-expressjs-light v-if="props.framework === 'ExpressJS'" />
            <simple-icons-koa v-if="props.framework === 'KoaJS'" />
            <simple-icons-fastify v-if="props.framework === 'FastifyJS'" />
            <img src="https://kita.js.org/logo.svg" width="40px" v-if="props.framework === 'KitaJS'" />
            <img src="https://elysiajs.com/assets/elysia.svg" width="40px" v-if="props.framework === 'ElysiaJS'" />
        </div>

        <div class="flex flex-row items-center gap-1">
            <span class="text-xl">{{ props.framework }} - </span>
            <span class="text-elysia-purple">
                {{ props.title }}
            </span>
        </div>
    </div>
    <div class="mt-7"></div>
</template>